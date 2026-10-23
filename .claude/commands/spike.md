Perform a feasibility spike for refactoring the module or area I'll describe.

Use Plan mode — read-only, no code changes.

Analyze:
1. **Churn** — how frequently are files in this area changed? (git log --oneline --since="6 months ago" on relevant paths)
2. **Debt signals** — code smells, duplicated logic, missing types, god classes/functions, test coverage gaps
3. **Isolation** — how coupled is this module to the rest of the codebase? Count incoming and outgoing dependencies
4. **Blast radius** — what breaks if this module's interface changes?

Write findings to `SPIKE.md` with this structure:

```
# Spike: <Module/Area Name>

## Churn Analysis
<findings with data>

## Debt Signals
<findings>

## Isolation Score
- Incoming deps: <count> (<list>)
- Outgoing deps: <count> (<list>)
- Shared state: <DB tables, globals, caches>

## Blast Radius
<what depends on this module>

## Verdict: GO / NO-GO / CONDITIONAL
<one paragraph justification with conditions if applicable>
```
