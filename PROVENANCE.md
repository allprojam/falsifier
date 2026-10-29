# PROVENANCE — every claim binds to a file:line

Rule **A-22**: no figure is quoted without a resolvable pointer.
`python3 src/check_provenance.py` verifies this mechanically and exits non-zero on failure (A-23).

Format: `| claim | value | path:line | token |`

JSON rows use `path/to/file.json` in the third column and the **key name** in the fourth; the key
is searched recursively.

---

## Verified claims

| claim | value | path:line | token |
|---|---|---|---|
| _(example)_ | 42 | docs/HYPOTHESIS.md:1 | HYPOTHESIS |

## Superseded — never cite as current (A-21)

| stale figure | stale value | current value | source of truth |
|---|---|---|---|
| | | | |

## Withdrawn — no source exists anywhere in the project

| claim | status |
|---|---|
| | |
