# DECISIONS — the ADR ledger

Locked decisions in order. Each has a status, the reason it exists, and where it can be checked.
Entries marked **CORRECTED** record a decision that was made, published, then found wrong by
measurement. They are kept, never deleted — a ledger with no corrections is a ledger nobody
checked.

---

## Method

### A-01 · Pre-register, hash, then run — LOCKED
No study runs before its prereg exists and its sha256 is in `runs/`. Check: `src/prereg.py verify`.

### A-02 · A gate is read exactly as written — LOCKED
A failed gate is reported as failed. A friendlier wording that would have passed is noted in an
addendum and **not adopted**.

### A-03 · Multiple metrics, all required — LOCKED
No single-metric wins. Calmar alone rewards raw drift; pair it with Sharpe, MaxDD and Ulcer.

### A-04 · Classify failures by mechanism, not by verdict — LOCKED
Separate *overfitting* (fixable by pruning) from *non-stationarity* (not fixable). They point at
different remedies.

### A-05 · Fixed seeds, no seed-shopping — LOCKED

## Data

### A-06 · Record provenance including what failed — LOCKED
A source that 403s is a finding. It saves the next session an hour.

### A-07 · Never splice two sources without proving they agree on the overlap — LOCKED
Record the correlation on the overlap window.

### A-08 · Prefer exact mechanics to convenient constants — LOCKED
A constant bond duration of 7.5 is wrong at 15% yields (true modified duration 4.72, 37% less rate
sensitivity). The constant overstated both the crash and the rally.

## Enforcement

### A-21 · A superseded report must banner itself — LOCKED
Any report whose numbers came from a since-fixed engine carries a banner naming what changed and
pointing at the current artifact, plus an entry in `STATE.md`'s report registry.

### A-22 · Every quoted figure resolves to a pointer — LOCKED
`| claim | value | file:line | token |` in `PROVENANCE.md`. No exceptions.

### A-23 · A-22 is enforced by a script, not by discipline — LOCKED
`src/check_provenance.py`, non-zero exit on failure.
**This is the most important entry in this file.** The prose rule was agreed first, and the first
registry written under it had **15 of ~43 citations wrong** — off-by-one lines, tokens from the
adjacent row, pointers into files that did not contain the figure at all. A human or model
applying a rule from memory to the thing the rule constrains will fail at a double-digit rate.
Only an executable catches it.

### A-24 · Corrections are permanent and visible — LOCKED
A corrected finding keeps its original entry, gains a **CORRECTED** marker, and states both the
wrong and the right number. Deleting the error destroys the evidence that the process works.
