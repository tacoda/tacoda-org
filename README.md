# tacoda-org

An example **Keystone plugin** for [Keystone](https://github.com/tacoda/keystone) 1.0+. Demonstrates all four content kinds — corpus, guides, playbooks, actions — plus the `strict` override-block and the `required` gap-surfacing field.

Every plugin has the same shape; precedence in the cascade is determined by where each plugin sits in the consumer's `keystone.json` nesting. Nest one plugin inside another to give the outer one strict-locking authority over the inner.

## Install

Declare the plugin and let `install` vendor it into `<harness-root>/plugins/tacoda-org/`:

```bash
keystone plugin add tacoda/tacoda-org@v1.0.0
```

Or — if you already edited `keystone.json` by hand:

```bash
keystone install
```

The vendored tree is read-only on POSIX, gitignored, and hash-pinned in `<harness-root>/keystone.lock.json`. `keystone verify` reports any drift; `keystone install` re-applies the pinned state.

Update an installed copy:

```bash
keystone plugin update tacoda-org                       # re-resolve the recorded ref
keystone plugin update tacoda-org tacoda/tacoda-org@v1.1.0   # bump to a new ref
```

Remove it:

```bash
keystone plugin remove tacoda-org
```

## What's inside

```
keystone-plugin.json    # manifest: name, version, keystone_min, strict, required, description
corpus/                 # reasoning (on-demand)
  documentation.md
  todos.md
  release-process.md
guides/                 # rules (always loaded)
  documentation.md      # STRICT
  todos.md              # STRICT
  release-process.md    # non-strict — lower-precedence plugins or the project may override
playbooks/              # ordered action chains
  release.md            # non-strict — projects may extend
actions/                # single units of lifecycle work
  changelog-check.md    # STRICT
  static-analysis.md    # STRICT — language-agnostic; runs registered static-analysis sensors
  announce-release.md   # non-strict — projects pick their own announcement surface
```

The whole tree (minus `.git` and this README) is copied verbatim into `<harness-root>/plugins/tacoda-org/` at install time.

## Strictness in this example

| Item | Strict? | Why |
|---|---|---|
| `guides/documentation` | ✓ | Hemingway-test discipline applies broadly |
| `guides/todos` | ✓ | Ownership + context + trigger is non-negotiable |
| `actions/changelog-check` | ✓ | No release ships without a real CHANGELOG entry |
| `actions/static-analysis` | ✓ | Static analysis is mandatory; consumers plug in the tool (rubocop, staticcheck, eslint) via a sensor |
| `guides/release-process` | ✗ | Consumers refine the release flow (auth, mobile, infra differ) |
| `playbooks/release` | ✗ | Consumers add steps; this plugin provides the spine |
| `actions/announce-release` | ✗ | Consumers pick their own announcement surface (Slack, email, blog) |

## The `required` field

`tacoda-org` does **not** ship a `release-notes` action — but it declares that one should exist somewhere in the install:

```json
"required": {
  "actions": ["release-notes"]
}
```

After `keystone plugin add tacoda-org`, `keystone verify` reports:

```
? plugin "tacoda-org" requires actions/release-notes — define it at <harness-root>/actions/release-notes.md
```

The project (or a lower-precedence plugin) is on the hook to provide it. The gap is **advisory**, not a hard error — solo installs can defer it until a release.

## Precedence — what happens when the project (or another plugin) ships the same basename

Precedence in 1.0 is **pre-order over the nested `keystone.json` plugin tree**, with project files taking effect first and plugin content layered underneath. Non-strict items can be overridden; strict items refuse the override and `keystone verify` errors.

For example, with `tacoda-org` installed and a project-level `harness/playbooks/release.md`:

- `playbooks/release` is non-strict in this plugin → project's file wins.
- `actions/changelog-check` is strict in this plugin → a project `harness/actions/changelog-check.md` is refused by verify.

See the upgrade guide and `docs/conventions.md` in the keystone repo for the full precedence rules.

## Releasing this plugin

A change here ships as a git tag — `keystone-min` in the manifest declares the binary floor.

```bash
git commit -m "feat: add foo"
git commit -m "docs(changelog): release 1.1.0"
git tag -a v1.1.0 -m "v1.1.0"
git push origin main v1.1.0
```

Consumers pick it up with `keystone plugin update tacoda-org tacoda/tacoda-org@v1.1.0`.
