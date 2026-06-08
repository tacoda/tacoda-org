# static-analysis

**Run every static-analysis sensor registered in the cascade and gate on the combined result.** Strict — no consumer (nested plugin or project) may override this action's enforcement.

The action is intentionally language-agnostic. Each consumer picks the tool that fits its stack (`rubocop` for Ruby, `staticcheck` for Go, `eslint` for TypeScript) and ships it as a sensor under `harness/sensors/` or `harness/plugins/<nested-plugin>/sensors/`. This action is the spine that invokes them.

## Activities

1. **Discover registered static-analysis sensors.** Walk `harness/sensors/` and `harness/plugins/*/sensors/` for sensor files tagged as static-analysis (the file is named after the tool, e.g. `rubocop.md`, `staticcheck.md`). Read each sensor's contract.
2. **Confirm at least one sensor is registered.** If none exist in the cascade, fail and report the gap — this plugin requires that static analysis be wired up somewhere. (A nested plugin or the project must provide it.)
3. **Run each sensor's command** as declared in its contract — the tool command lives in `corpus/state/CODEBASE_STATE.md`. Collect exit codes and structured output.
4. **Aggregate findings.** A non-zero exit from any sensor fails the gate. Findings are reported per-sensor so the developer knows which tool flagged what.

## Gate

Hard gate. Strict — `keystone verify` blocks any consumer file at `harness/actions/static-analysis.md` that would override this. A clean run means every registered static-analysis sensor passed; a missing sensor (no static analysis wired up anywhere) also fails.

## Output

```
✓ static-analysis: 1 sensor registered (rubocop@harness/sensors/rubocop.md)
✓ rubocop: 0 offenses across 47 files
```

Or, on failure:

```
✗ rubocop: 3 offenses
  app/models/user.rb:14:5: Style/GuardClause: Use a guard clause instead of wrapping the code inside a conditional expression.
  app/models/user.rb:42:3: Lint/UselessAssignment: Useless assignment to variable - `result`.
  app/services/billing.rb:88:7: Metrics/MethodLength: Method has too many lines. [12/10]
keystone: static-analysis gate failed
```

Or, on the no-sensor gap:

```
✗ static-analysis: no static-analysis sensors registered in the cascade
  Add a sensor at harness/sensors/<tool>.md or harness/plugins/<nested-plugin>/sensors/<tool>.md.
keystone: static-analysis gate failed
```
