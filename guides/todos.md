# TODOs — rules

The rules from [`corpus/todos.md`](../corpus/todos.md).
Loaded ambient; enforced by the [drift sensor](../../../sensors/drift.md). The corpus file holds the full reasoning and anti-patterns.

## GOLDEN RULES

- **Every TODO names an owner.** A person (`@handle`) or, for shared work, a team handle. No owner → not a TODO; either fix it now or file an issue.
- **Every TODO has context.** *Why* this is a TODO, enough that a reader two months from now can decide if it's still live. If there's an issue, link it.
- **Every TODO has a trigger.** A date, a release, an unblocking event. Without one, TODOs accumulate as dead weight.

## RULES

- **No bare `TODO: fix this` comments.** A TODO without owner + context + trigger is noise; either upgrade it or delete it.
- **Blockers are not TODOs.** If something blocks the current change, fix it before merge or revert; never ship a TODO marking known-broken behavior.
- **Cross-team work goes in the tracker, not in comments.** A TODO in your code is invisible to the team that owns the fix.
- **Periodic triage.** TODOs are reviewed at least once per release: still-live entries stay, stale ones move to the tracker or get deleted.

---

Traces to: [`corpus/todos.md`](../corpus/todos.md).
