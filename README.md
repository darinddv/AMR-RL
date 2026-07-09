# resistance-control

Reinforcement learning for antimicrobial resistance control.

## Install

```bash
pip install -e ".[dev]"
```

## Test

The suite is split into two oracles, and they answer different questions.

```bash
pytest tests/unit         # Tier 1: is the code correct?
pytest tests/validation   # Tier 2: is the science correct?
pytest                    # both
```

A green Tier-1 run means the implementation matches its own contract. It says
nothing about whether the model is a faithful account of resistance dynamics —
that is what Tier 2 is for.

## Layout

```
docs/adr/      architecture decision records
docs/plans/    planning documents
src/           resistance_control package
tests/unit/    Tier-1 oracle: code-correct
tests/validation/  Tier-2 oracle: science-correct
notebooks/     exploratory analysis
```
