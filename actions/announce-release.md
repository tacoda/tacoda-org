# announce-release

**Post a release announcement.** Non-strict at the tacoda-org tier — teams and projects may ship `harness/actions/announce-release.md` or `harness/policies/<team>/actions/announce-release.md` to replace this with their own announcement style.

The org-default is intentionally minimal: a one-line message with the version, date, and a link to the CHANGELOG entry. Teams that want richer formats (Slack blocks, internal docs, customer-facing posts) override.

## Activities

1. **Read the CHANGELOG entry** for the version being released. Pull the heading line and the first paragraph.
2. **Format the announcement** as one line:

   ```
   <project> vX.Y.Z released (YYYY-MM-DD) — <first sentence of changelog entry>. Notes: <link to CHANGELOG.md at the tag>.
   ```
3. **Print to stdout.** No upload, no API call at this tier — the org default is paste-friendly. Teams override to wire this into Slack, email, etc.

## When to invoke

- Immediately after the release tag is pushed.
- Manually, when re-announcing or correcting an earlier announcement.

## Output

A single line of text, ready to paste into wherever the project announces releases.
