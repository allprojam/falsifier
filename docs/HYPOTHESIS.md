# HYPOTHESIS — <name>

> Fill every field **before** running anything. Lock with
> `python3 src/prereg.py lock docs/HYPOTHESIS.md`. Do not edit after locking (I-6).

## Claim

One sentence. Falsifiable. State what would prove it wrong.

## Data

| series | source | span | verified overlap | notes |
|---|---|---|---|---|
| | | | | |

- Any splice between two sources must be validated on the overlap window; record the correlation.
- Record sources that failed. A 403 is a finding.

## Arms

Exact, numbered. No "and similar".

| arm | definition | parameters |
|---|---|---|
| | | |

## Metrics

| metric | definition | direction |
|---|---|---|
| Sharpe | | higher |
| MaxDD | | higher (less negative) |
| Ulcer | | lower |

## Gates — state them as boolean conditions

1. …
2. …

**Gate reading rule (I-7).** The gate is evaluated exactly as written. If a reworded gate would
have passed, record that in the addendum; do not adopt it.

## Nulls

Which null generators run against each arm? Both are required for any signal-based rule:
- block-shifted duty-cycle null (`n` shifts)
- 1-bar causality perturbation (I-…)

## Anti-shopping commitments

- Number of configurations tested: ___
- If more are tried later, they are reported as a second, separately-locked pass.
- Seed(s): ___

## What would make me abandon this

Write it now, while it is still uncomfortable.
