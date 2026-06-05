# TODOs

A TODO is a promise to a future reader — usually a future version of yourself — that something is unfinished or wrong and someone (named) is going to do something about it. Most TODOs in real codebases break this promise: they have no owner, no context, no deadline, and no way for the reader to decide whether the TODO is stale or live.

> **Rules extracted:** [`guides/todos.md`](../guides/todos.md). This file holds the reasoning and anti-patterns.

## What makes a TODO actionable

Three things, every time:

1. **Owner** — a person (or, for shared work, a team handle) who has agreed to act on it. Without an owner, "do this later" is a wish, not a TODO.
2. **Context** — *why* this is a TODO, not just *what* needs to happen. The reader needs enough context to decide whether the TODO is still live. Link to an issue if one exists.
3. **A trigger** — when should this be done? A deadline, a release, an unblocking event ("once we move off Postgres 14"). Without a trigger, TODOs accumulate forever.

```python
# TODO(@ian, 2026-07): swap to the streaming endpoint once the
# v2 API ships — tracking in INGEST-412. Current batch endpoint
# rate-limits at 200/min which hurts the backfill flow.
```

Compare to the version that gets written most often:

```python
# TODO: fix this
```

The second one is noise. It's worse than no comment, because it implies someone agreed to fix it. Nobody did.

## Why ownerless TODOs are worse than no TODO

A TODO without an owner does three bad things:

1. **Diffuses responsibility.** Everyone thinks someone else is on it. Nobody is.
2. **Inflates apparent debt.** The codebase looks like it has a backlog when really it has a graveyard.
3. **Trains readers to ignore TODOs.** After enough stale ones, the team stops reading them. The next *real* TODO gets ignored too.

The grep test: `git grep -n TODO` should return a list every entry of which is currently actionable by a specific person on a specific timeline. If it returns a list that the team would describe as "we'll get to those eventually," the entries should be moved to the issue tracker or deleted.

## When a TODO is the wrong tool

- **Anything blocking the current change.** Fix it now or revert; don't ship a TODO.
- **Anything that affects another team.** File an issue against their tracker; a TODO in your code is invisible to them.
- **Anything with a real deadline.** Issues and milestones, not source comments. Comments don't trigger calendar reminders.

A TODO is right for: a real near-future improvement, with an owner who'll act on it, that doesn't justify the overhead of a tracker ticket. That's a narrow band — narrower than most codebases reflect.

## Periodic prune

Treat TODOs like state: empirical, ages out. A periodic sweep ("TODO triage", monthly or per-release) re-checks each entry — is the owner still here, is the context still true, is the trigger still meaningful? Anything that fails the test gets either filed in the tracker or deleted. The discipline is what keeps the TODOs you *do* keep meaningful.
