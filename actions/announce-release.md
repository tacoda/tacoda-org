# announce-release

**Post a release announcement.** Non-strict — the project may ship `harness/actions/announce-release.md` to replace this with its own announcement style, and the project's version will win.

The default is intentionally minimal: a one-line message with the version, date, and a link to the CHANGELOG entry. Projects that want richer formats (Slack blocks, internal docs, customer-facing posts) override.

## Activities

1. **Read the CHANGELOG entry** for the version being released. Pull the heading line and the first paragraph.
2. **Format the announcement** as one line:

   ```
   <project> vX.Y.Z released (YYYY-MM-DD) — <first sentence of changelog entry>. Notes: <link to CHANGELOG.md at the tag>.
   ```
3. **Print to stdout.** No upload, no API call by default — the output is paste-friendly. Projects override to wire this into Slack, email, etc.

## When to invoke

- Immediately after the release tag is pushed.
- Manually, when re-announcing or correcting an earlier announcement.

## Output

A single line of text, ready to paste into wherever the project announces releases.
