# release

**Shared release playbook.** Walks the mandated release activities in order. **Non-strict** — consumers may ship `harness/plugins/<nested-plugin>/playbooks/release.md` with nested-plugin extensions; projects may ship `harness/playbooks/release.md` for project-specific steps.

**Invoke as:** "run release" or "run the release playbook."

## Activities

1. **changelog-check** — read [`changelog-check.md`](../actions/changelog-check.md). Verify `CHANGELOG.md` has an entry for the version being released. Hard gate: no release without a changelog entry.
2. **release-notes** — read `release-notes.md`. **Required but not shipped by tacoda-org** — the project (or another plugin in the cascade) must provide this action. `keystone verify` will surface it as a gap when missing.
3. **verify** — read [`verify.md`](../../../actions/verify.md). Run the standard lint / type-check / test / build / drift / commit-message sensors against the release commit.
4. **announce-release** — read [`announce-release.md`](../actions/announce-release.md). Default is a single CHANGELOG link; consumers can override with their own announcement style.

## Iron law

**No release without a tag.** After every activity above succeeds, the release is the `git tag -a vX.Y.Z` step. Untagged commits are not releases.

## When *not* to use release

- Hotfix branches that don't bump the version — run **task** with a `fix(...)` commit and skip the release playbook.
- Pre-release work in progress on a feature branch — wait until the branch lands.
