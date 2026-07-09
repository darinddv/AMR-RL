# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project

`resistance-control` — reinforcement learning for antimicrobial resistance control.
Python package, src layout, installed as `resistance_control`.

## Commands

```bash
pip install -e ".[dev]"   # install with dev tooling
pytest tests/unit         # Tier-1 oracle
pytest tests/validation   # Tier-2 oracle
pytest                    # both
ruff check .              # lint
ruff format .             # format
mypy                      # typecheck (strict, src/ only)
```

## The two-tier test oracle

This is the central convention of the repo. The two test trees are not "fast tests"
and "slow tests" — they check different kinds of claim, and conflating them is the
main failure mode to avoid.

**`tests/unit/` — Tier 1: code-correct.** Does the implementation match its own
contract? Deterministic, no free parameters, fails for exactly one reason. Fix a
seed, assert on shapes, invariants, boundary conditions, conservation laws that
follow from the code as written. A Tier-1 failure is always a bug in the code.

**`tests/validation/` — Tier 2: science-correct.** Does the model reproduce
behavior the literature or theory says it should? Assert on qualitative regimes
and published quantities, with tolerances stated in the test and justified in a
comment or a linked ADR. A Tier-2 failure may mean the code is wrong, or it may
mean the model is wrong — say which in the failure message where you can.

Passing Tier 1 licenses no claim about the science. Do not describe a change as
"verified" or "working" on the strength of Tier-1 tests alone; say "unit tests
pass" and name what remains unchecked.

Mark tests with the matching `@pytest.mark.unit` / `@pytest.mark.validation`.
`--strict-markers` is on, so a typo in a marker is an error rather than a silent skip.

## Decision records

Non-obvious choices — a discretization, a reward shaping term, a tolerance in a
Tier-2 test, a departure from a paper's parameterization — go in `docs/adr/` as
`NNNN-short-title.md`, numbered sequentially. Record the alternatives considered
and why they lost. If a Tier-2 tolerance is loose, the ADR is where the reason lives.

Planning documents live in `docs/plans/`. They are inputs to the work, not a record
of it; when a plan and the code disagree, the code is what shipped.

## Conventions

- Python 3.11+. Type annotations on public functions; `mypy` runs strict over `src/`.
- Simulation and analysis code goes in `src/`; `notebooks/` is exploratory and is
  not imported by the package or the tests.
- Prefer explicit units in names or docstrings (`dose_mg_per_l`, not `dose`).
  Resistance dynamics are easy to get numerically right and dimensionally wrong.
- Seed every stochastic path explicitly. An unseeded run in a test is a bug.
