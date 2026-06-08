# Release process — rules

The rules from [`corpus/release-process.md`](../corpus/release-process.md).
Loaded ambient; enforced by the [drift sensor](../../../sensors/drift.md). Non-strict at the org tier — teams and projects may layer refinements with the same basename.

## GOLDEN RULES

- **Every release updates `CHANGELOG.md`.** No release without a changelog entry naming the version and date.
- **Every release is a git tag.** Untagged commits are not releases.
- **Releases describe what shipped, not what's coming.** Aspirational changelog entries are not releases.

## RULES

- **Changelog entries lead with the change**, not with marketing. "Added X" / "Removed Y" / "Fixed Z" — not "We're excited to announce."
- **Pre-1.0 releases follow semver minor-as-breaking.** Minor versions may include breaking changes; document them under a `### Migration` section.
- **Tag format is `vX.Y.Z`.** Consumers pin by tag; format consistency matters.
- **Release commits are minimal.** The version-bump and changelog edit live in their own commit, separate from the feature work. Tag at the release commit.

---

Traces to: [`corpus/release-process.md`](../corpus/release-process.md).
