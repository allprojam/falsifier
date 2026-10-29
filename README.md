# falsifier

An executable falsification and provenance harness for quantitative research.

Prose rules do not stop backtest leakage or citation drift; executables do. `falsifier` is a
minimal, domain-agnostic starter template that gates quantitative claims behind three
non-negotiable checks (`python3 src/check_all.py`):

1. **Append-only pre-registration** (`src/prereg.py`, `I-5`, `I-6`) — locks `docs/HYPOTHESIS.md`
   with a SHA-256 digest before results exist, and **refuses to re-lock** if the specification was
   tampered with afterwards. Re-locking is the backdoor that makes pre-registration theatre; this
   script closes it with a non-zero exit and no override flag.
2. **Executable provenance** (`src/check_provenance.py`, `A-22`, `A-23`) — parses every registered
   numeric claim in `PROVENANCE.md`, opens the cited `file:line` or nested JSON key, asserts exact
   token presence, and exits non-zero on any drift.
3. **Namespace and invariant architecture** (`src/check_decisions.py`, `ARCHITECTURE.md`,
   `I-1..I-10`) — enforces repo-scoped decision IDs (`falsifier:A-NN`) so two projects cannot
   silently share an identifier space, alongside the invariants that caught real bugs: 1-bar
   causality perturbation, IID and contiguous block-shifted duty-cycle nulls, and mode-matched
   pre/post-tax baselines.

## Why it exists

It was extracted from a 250-file research repository that produced **eight documented prose
drifts** in five working turns, including five in the summaries *while actively auditing for them*.
Representative failures, all found by the scripts rather than by review:

| failure | what it cost |
|---|---|
| pre-registration re-locked after seeing results | one historical violation, disclosed |
| `pct_change()` applied twice to a growth factor | bond CAGR collapsed; two tables invalidated |
| tax baseline not mode-matched | a −149 bp/yr "tax drag" that was a benchmark mismatch |
| decision IDs colliding across two repos | `A-06..A-08` resolved to different rules depending on which repo was open |
| **7 of 7 citations wrong** on the first write | off-by-one lines, tokens from adjacent rows, pointers into files that did not contain the figure |

That last row happened **twice**, in repositories of 17 and 19 files. It is not a
large-context problem; it is what happens when a claim is written from memory about a file the
author just edited.

## Quick start

```bash
python3 src/check_all.py                      # verify the locked state
python3 src/prereg.py lock docs/HYPOTHESIS.md  # once, before any results
python3 src/prereg.py verify docs/HYPOTHESIS.md
```

## Layout

```
ARCHITECTURE.md   the contract: I-1..I-10, both null generators, cross-project ID rules
STATE.md          live board and report registry (A-21)
DECISIONS.md      ADR ledger; corrections are kept, never deleted
PROVENANCE.md     the citation registry (A-22)
src/prereg.py     append-only sha256 lock
src/check_provenance.py  citation checker
src/check_decisions.py   repo:A-NN namespace guard
src/check_all.py  runs everything; non-zero if any check fails
docs/HYPOTHESIS.md       template — fill this in first
docs/RESULT_TEMPLATE.md
```

## The two null generators

Both are required before any signal-based rule is believable.

**Block-shifted duty-cycle null.** For a rule that is in cash *x*% of the time: circularly shift the
signal mask by a random offset, preserving duty cycle and run-length distribution, then re-run. A
rule that cannot beat this has found only that cash has lower variance. In the source project, an
SMA200 filter beat a naive IID null on 6/9 assets and the block null on 4/9, clearing **both on
1/9**.

**1-bar causality perturbation.**
```python
r_new = r.copy(); r_new[t:] += 10.0          # perturb the future
w_new = strategy(r_new)
assert (w_new[:t+1] == w_old[:t+1]).all()    # the past must be untouched
```
This catches same-bar lookahead, and it turned a fake 9/9 PASS into a 1/9 FAIL.

## What this does not do

It checks **referential integrity and gate integrity**, not correctness of your research. A
registered claim can still be a bad claim. The scripts guarantee that a number quoted in a summary
is the number that exists in the artifact — nothing more. Every repository built on this harness
must state its own non-coverage boundary.

## Reference studies built with this harness

- **[gold_macro](https://github.com/allprojam/gold_macro)** — 11-pass audit of retail timing and
  55-year Eq/Au/Bd allocation.
- **[vrp_audit](https://github.com/allprojam/vrp_audit)** — pre-registered BTC variance risk
  premium audit.

## License

MIT.
