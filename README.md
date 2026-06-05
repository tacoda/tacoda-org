# tacoda-org

An example **org-tier** policy for [Keystone](https://github.com/tacoda/keystone) 0.12+. Demonstrates all four policy components — corpus, guides, playbooks, actions — plus the `strict` override-block and the `required` gap-surfacing field.

Pair this with [`tacoda-team`](https://github.com/tacoda/tacoda-team) to see the full three-tier cascade in action (org → team → project).

## Install

```bash
keystone init --policy git+https://github.com/tacoda/tacoda-org.git#v0.2.0
```

Track the default branch instead of a pinned tag for rolling updates:

```bash
keystone init --policy git+https://github.com/tacoda/tacoda-org.git#main
```

Update an installed copy:

```bash
keystone policy update tacoda-org                 # re-resolve the recorded ref
keystone policy update tacoda-org git+https://github.com/tacoda/tacoda-org.git#v0.3.0
```

## What's inside

```
keystone-policy.yaml         # manifest: name, version, tier=org, strict, required
policy/
  harness/policies/tacoda-org/
    corpus/                  # reasoning (on-demand)
      documentation.md
      todos.md
      release-process.md
    guides/                  # rules (always loaded)
      documentation.md       # STRICT
      todos.md               # STRICT
      release-process.md     # non-strict — teams/projects may override
    playbooks/               # ordered action chains
      release.md             # non-strict — teams/projects may extend
    actions/                 # single units of lifecycle work
      changelog-check.md     # STRICT
      static-analysis.md     # STRICT — language-agnostic; runs registered static-analysis sensors
      announce-release.md    # non-strict — teams/projects may override
```

## Strictness in this example

| Item | Tier | Strict? | Why |
|---|---|---|---|
| `guides/documentation` | org | ✓ | Hemingway-test discipline applies across every team |
| `guides/todos` | org | ✓ | Ownership + context + trigger is non-negotiable org-wide |
| `actions/changelog-check` | org | ✓ | No release ships without a real CHANGELOG entry |
| `actions/static-analysis` | org | ✓ | Static analysis is mandatory; teams plug in the tool (rubocop, staticcheck, eslint) via a sensor |
| `guides/release-process` | org | ✗ | Teams refine the release flow (auth, mobile, infra differ) |
| `playbooks/release` | org | ✗ | Teams add steps; the org provides the spine |
| `actions/announce-release` | org | ✗ | Teams pick their own announcement surface (Slack, email, blog) |

## The `required` field

`tacoda-org` does **not** ship a `release-notes` action — but it declares that one should exist somewhere in the cascade:

```yaml
required:
  actions:
    - release-notes
```

After `keystone policy add tacoda-org`, `keystone policy verify` reports:

```
? policy "tacoda-org" (tier org) requires actions/release-notes — define it at harness/actions/release-notes.md
```

The project (or a team policy) is on the hook to provide it. The verify gap is **advisory**, not a hard error — solo installs can defer it until a release.

## Overrides — what happens when a team or project ships the same file

```
harness/
├── playbooks/
│   └── release.md                            # ← project's release playbook (wins)
└── policies/
    ├── tacoda-team/
    │   └── playbooks/release.md              # ← team's release playbook (used if project absent)
    └── tacoda-org/
        └── playbooks/release.md              # ← org default (used if both above absent)
```

`release` is non-strict at the org tier, so this cascade is fine. Compare with `changelog-check`: if a project shipped `harness/actions/changelog-check.md`, `keystone policy verify` would error and the install/update would refuse.

## Cascade behavior at a glance

- **Solo project** (no policies installed) — none of the above applies; the harness works as if this policy didn't exist.
- **Solo + tacoda-org** — strict items lock in immediately; the `required` gap is advisory.
- **Solo + tacoda-org + tacoda-team** — team extends/overrides the non-strict pieces; team-level strict adds another lock.

See [`harness/policies/README.md`](https://github.com/tacoda/keystone/blob/main/harness/policies/README.md) in the keystone repo for the full cascade documentation.

## Releasing this policy

A change here ships as a git tag:

```bash
git commit -m "feat: add foo"
git commit -m "docs(changelog): release 0.3.0"
git tag -a v0.3.0 -m "v0.3.0"
git push origin main v0.3.0
```

Consumers pick it up with `keystone policy update tacoda-org git+https://github.com/tacoda/tacoda-org.git#v0.3.0`.
