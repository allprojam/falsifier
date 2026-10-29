# ARCHITECTURE — the falsification harness

A starter template for research that must survive adversarial review. It contains **no data and
no strategy**. It contains the ten invariants and the executable citation checker that were
derived the hard way: eleven research passes in which every one of fifteen defects produced a
wrong published number before it was caught.

Copy this directory, replace `docs/HYPOTHESIS.md` with your claim, and delete nothing else.

---

## The one idea

**A result is only a result if the test was frozen before the answer was known, and if every
number in the write-up resolves to a pointer in the repository.**

Everything below exists to enforce those two sentences.

## The loop

1. Write `docs/HYPOTHESIS.md`. State: data sources, exact arms, metric set, **every gate
   condition**, and the anti-shopping commitments. No results may exist yet.
2. `python3 src/prereg.py lock` → writes `runs/<name>_prereg.lock` with the sha256 and a UTC
   timestamp.
3. **Only now** write and run code. Output to `runs/<name>.json`.
4. Write `docs/<NAME>_RESULT.md`. Re-verify the hash with `python3 src/prereg.py verify`.
5. Register every quoted figure in `PROVENANCE.md` as `| claim | value | path:line | token |`.
6. `python3 src/check_provenance.py` must print **0 broken**. It exits non-zero otherwise.
7. Report the gate verdict **exactly as written**, including when it fails.

## Layout

```
falsifier/
├── ARCHITECTURE.md        this file: the contract
├── STATE.md               live board + report registry (A-21)
├── DECISIONS.md           ADR ledger; corrections are kept, not deleted
├── PROVENANCE.md          the citation registry (A-22)
├── src/
│   ├── prereg.py          lock / verify a preregistration by sha256
│   └── check_provenance.py  executable citation checker (A-23)
├── docs/
│   ├── HYPOTHESIS.md      the template to fill in
│   └── RESULT_TEMPLATE.md
├── runs/                  locks + result JSON
└── data/                  inputs; committed, never edited in place
```

## The ten invariants

Each was violated once in the source project and produced a published error. They are not style
preferences.

| id | invariant | what it caught |
|---|---|---|
| **I-1** | Weights sum to 1; assert before running | `[.5,.5,.5]` = 150% invested |
| **I-2** | Never `pct_change()` a growth factor | bond CAGR collapsed to ~1%/yr, invalidating two tables |
| **I-3** | `post_tax_cagr <= pre_tax_cagr`, enforced by `assert` | a −149 bp/yr "tax drag" that was really a baseline mismatch |
| **I-4** | Plausibility-band every sleeve before quoting | a 55-year 1%/yr bond leg is a bug, not a finding |
| **I-5** | Verify the prereg hash before quoting any figure | makes a re-lock impossible to hide |
| **I-6** | Never re-lock a prereg after seeing results; write an addendum | one historical violation, disclosed |
| **I-7** | Read the gate as written; do not adopt a friendlier wording | `5/9` does not become a pass at a `7/9` bar |
| **I-8** | Fixed seeds, no seed-shopping | Dirichlet nulls reproducible |
| **I-9** | Compare like with like: after-tax vs pre-tax **from the same execution policy** | drift vs fixed-weight manufactured a negative tax |
| **I-10** | Every quoted figure resolves to `file:line` or a JSON key, checked by script | 15 of 43 citations wrong on the first run |

## Two null generators you will need

These are the two that killed every timing strategy in the source project. Implement both before
believing any result.

**1. Block-shifted duty-cycle null** — for any rule that sits in cash *x*% of the time.
Circularly shift the signal mask by a random offset, preserving the exact duty cycle and run
length distribution, then re-run. A rule that does not beat this null has found nothing but the
fact that cash has lower variance. In the source project the SMA200 rule beat a naive IID null on
6/9 assets and a block null on only 4/9, clearing **both** in **1/9**.

**2. 1-bar causality perturbation** — for any execution rule.
```python
r_new = r.copy(); r_new[t:] += 10.0          # perturb the future
w_new = strategy(r_new)
assert (w_new[:t+1] == w_old[:t+1]).all()    # the past must be untouched
```
This catches same-bar lookahead, which is the single most common way a backtest lies. In the
source project it turned a fake 9/9 PASS into a 1/9 FAIL.

## Data rules

- Inputs are committed CSVs. **No network at analysis time** — the run must reproduce offline.
- Record provenance for every series, including what did *not* work. A source that 403s is worth
  writing down; it stops the next session from burning an hour on it.
- Validate any spliced series on its overlap window and record the correlation. Never splice two
  sources without proving they agree where both exist.

## Reporting rules

- A failed gate is reported as a failure. An addendum may explain it; it may not replace it.
- Any report whose numbers came from an engine that has since been fixed **carries a banner**
  naming what changed and pointing at the current artifact (A-21).
- Corrections live in `DECISIONS.md` permanently. Do not delete a wrong finding; a ledger with no
  corrections in it is a ledger nobody checked.

## Environment note (verified on the source project's host)

Python 3.12.3, pandas 3.0.6, numpy 2.4.4. Two traps: `Series.append` no longer exists (use
`pd.concat`), and a float weight matrix that does not sum to 1 will silently produce a 150%-invested
portfolio rather than an error.
