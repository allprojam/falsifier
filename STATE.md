# STATE — the live board

**Last updated:** <date> · **Phase:** <phase> · **Verdict:** <one line>

---

## Status

| | |
|---|---|
| Prereg hashes | `python3 src/prereg.py verify docs/HYPOTHESIS.md` |
| Provenance registry | `python3 src/check_all.py` → expect `3/3 checks passed` |
| Open blockers | |
| Deliverable | |

## Report registry (A-21) — which figures are current

| report | pass | status |
|---|---|---|
| | | |

Statuses: **current** · **pre-fix engine** (verdict stands, numbers unverified) ·
**superseded** (carries a banner; do not quote).

## Findings

| claim | measurement | verdict |
|---|---|---|
| | | |

## Blockers

## Next steps — in priority order

## Do not do

- Do not re-run a failed gate with reworded thresholds.
- Do not quote a report marked *superseded* or *pre-fix*.
- Do not trust a narrative explanation of a surprising number until an invariant proves the
  number is not an accounting bug.

## How to verify the whole board

```bash
python3 src/check_all.py            # expect: 3/3 checks passed
```
