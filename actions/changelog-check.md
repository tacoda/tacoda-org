# changelog-check

**Verify a CHANGELOG entry exists for the version being released.** Strict — no consumer (nested plugin or project) may override this action's enforcement.

## Activities

1. **Determine the target version.** Read it from `git tag --sort=-version:refname | head -1` (the most recent tag, if releasing the next minor/patch), or accept it as an argument ("run changelog-check for 0.12.0").
2. **Open `CHANGELOG.md`.** Confirm it exists at the repo root. If absent, fail.
3. **Confirm an entry for the target version.** Look for a heading matching `## [X.Y.Z]` followed by the date. If absent, fail and report the missing version.
4. **Confirm the entry is non-trivial.** A heading with no body below it (or only "TBD") fails. Releases require an actual description of what changed.
5. **Confirm the entry leads with the change**, not with marketing. Lines starting with "We're excited" / "Introducing" / "Powerful" trigger a warning (not a hard fail — judgment-call territory).

## Gate

Hard gate. No release proceeds without a real changelog entry. Strict — `keystone verify` blocks any consumer file at `harness/actions/changelog-check.md` that would override this.

## Output

```
✓ CHANGELOG entry for 0.12.0 (dated 2026-06-05)
✓ entry has substantive body (47 lines)
? entry opens with "We're excited" — consider revising to lead with the change
```
