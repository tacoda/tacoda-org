# Changelog

All notable changes to this plugin are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
this plugin uses semantic versioning. Releases are git tags of the form `vX.Y.Z`.

## [1.1.1] - 2026-06-08

Cascade-model wording fix, matching keystone v1.0.2's clarification. The plugin's prose described several override scenarios that aren't actually possible under the cascade — specifically, that nested plugins could override an outer plugin's non-strict items. They can't: outer plugins win over plugins nested inside them; only the project can override an outer plugin's non-strict item. This release tightens the README, the release playbook, the release-process guide, and the announce-release action to describe the actual semantics.

### Changed

- README cascade section rewritten as a three-step model (project wins; outer wins over inner; strict locks absolutely) with a corrected worked example.
- `playbooks/release.md`: removed the misleading "nested-plugin extensions" path; the project is the only layer that can override a non-strict item from this plugin.
- `guides/release-process.md`: dropped the "nested plugins or the project" framing; only the project can layer refinements on a non-strict item.
- `actions/announce-release.md`: same tightening; the project (not "consumers" generally) overrides the announcement style.
- README `# What's inside` listing line for `release-process.md`: replaced "lower-precedence plugins or the project" with the specific "the project may override."
- README `required` field section: clarified that `required` is about existence, not about which layer wins.

### Behavior

- No file structure or strict/required declarations changed. The vendored content is byte-identical to v1.1.0 except for the prose mentioned above.

## [1.1.0] - 2026-06-08

### Changed

- Removed `tier` from the manifest and from all framing throughout the plugin. Every plugin now shares a single shape; precedence in the cascade is determined by `keystone.json` nesting, not a tier label.
- Reworded "org tier / team tier / org plugin" references in the README, the release playbook, the action prose (`changelog-check`, `static-analysis`, `announce-release`), the release-process guide, and the release-process corpus to talk about consumers (nested plugins or the project) instead.

### Removed

- The dead `tacoda-team` reference in the README and in `actions/static-analysis.md`.
- The obsolete `harness/policies/<team>/...` paths that came along with the old tier model. Updated to `harness/plugins/<nested-plugin>/...` where the cascade is still relevant.

## [1.0.0] - 2026-06-08

### Changed

- Restructured for the Keystone 1.0 plugin format: top-level `guides/ corpus/ actions/ playbooks/` directories at the repo root, replacing the nested `policy/` wrapper.
- Rewrote the README around the 1.0 layout and CLI surface (`keystone plugin add`, `keystone install`, `keystone verify`).

## [0.3.0] - 2026-06-05

### Added

- `actions/static-analysis` — strict, language-agnostic spine that invokes every registered static-analysis sensor in the cascade.
- Worked example demonstrating all four content kinds, `strict`, and `required` against the Keystone 0.12 plugin format.

## [0.1.0] - 2026-06-04

### Added

- Initial scaffold of the example plugin: documentation guide, todos guide, release-process guide, and the `changelog-check` action.

[1.1.1]: https://github.com/tacoda/tacoda-org/releases/tag/v1.1.1
[1.1.0]: https://github.com/tacoda/tacoda-org/releases/tag/v1.1.0
[1.0.0]: https://github.com/tacoda/tacoda-org/releases/tag/v1.0.0
[0.3.0]: https://github.com/tacoda/tacoda-org/releases/tag/v0.3.0
[0.1.0]: https://github.com/tacoda/tacoda-org/releases/tag/v0.1.0
