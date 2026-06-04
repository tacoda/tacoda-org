# tacoda-policy

An example [Keystone](https://github.com/tacoda/keystone) policy. Installs into a project's `harness/policies/tacoda/` namespace and adds team preferences on top of the universal baseline that ships with keystone.

## Install

```bash
keystone init --policy git+https://github.com/tacoda/tacoda-policy.git#v0.1.0
```

Track the default branch instead of a pinned tag if you want updates on every `policy update`:

```bash
keystone init --policy git+https://github.com/tacoda/tacoda-policy.git#main
```

Update an installed copy:

```bash
keystone policy update tacoda                 # re-resolve the recorded ref
keystone policy update tacoda '#v0.2.0'       # bump to a new ref
```

## What's inside

Two pairs, each with a corpus file (reasoning) and a guide file (rules):

| Pair | Corpus | Guides |
|---|---|---|
| Documentation | `corpus/documentation.md` | `guides/documentation.md` |
| TODOs | `corpus/todos.md` | `guides/todos.md` |

Guides are loaded ambient; corpus is reached on-demand via the forward-link in each paired guide.

## Layout

```
keystone-policy.yaml         # manifest (name, version, description)
policy/
  harness/policies/tacoda/
    corpus/
      documentation.md
      todos.md
    guides/
      documentation.md
      todos.md
```

The `policy/` directory mirrors how the content lands inside a consumer's harness. Files outside that prefix (this README, the manifest) are ignored at install time.
