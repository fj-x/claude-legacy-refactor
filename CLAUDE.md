# Legacy Refactoring Project

## Context

This project follows the 4-Step Legacy Refactoring Protocol. Every refactoring goes through: Spike → Archeology → Plan → Cutover. Each step = one session, `/clear` between steps. Continuity through files, not chat history.

## Architecture

<!-- Replace with your project's bounded context map -->
```
src/
  ModuleA/    # Description of module A
  ModuleB/    # Description of module B
```

Cross-module communication: describe how modules interact (events, direct calls, shared DB, etc.)

## Protocol Artifacts

| File | Step | Purpose |
|------|------|---------|
| `SPIKE.md` | 1. Spike | Feasibility verdict: churn, debt, isolation, go/no-go |
| `LEGACY/*.md` | 2. Archeology | Compressed summaries per legacy module (≤2k tokens each) |
| `CRITIC.md` | 2. Archeology | Cross-module critic pass: hidden deps, shared state, risks |
| `PLAN.md` | 3. Plan | Ordered migration plan: one use case = one atomic commit |
| `REFACTOR_LOG.md` | 4. Cutover | Execution log: commit hashes, test results, DI swap status |

## Conventions

- One step = one session. `/clear` between steps
- Write English in all artifact files
- Conventional commits: `refactor(module): description`
- Every code change must pass existing + new characterization tests before commit
- Characterization tests fix current behaviour including known bugs — refactor ≠ bug fix

## Verify commands

<!-- Replace with your project's actual commands -->
```
make test          # Run all tests
make lint          # Linter / static analysis
make type-check    # Type checker (if applicable)
```

## Guardrails

- Do NOT read raw legacy code directly in the main agent session — always delegate to subagent and work from compressed summaries in `LEGACY/*.md`. Why: raw legacy in main context = attention dilution for the entire remaining session + confabulation surface where the model mixes old and new code.
- Do NOT mix refactoring with bug fixing in the same commit. Why: you cannot distinguish "broke behaviour" from "fixed bug" when both change in one diff. Known bugs get their own ticket and cycle.
- Do NOT modify files outside the current refactoring scope without explicit approval. Why: blast radius control — one bad edit in an unrelated module can cascade.
- Do NOT skip the characterization test step. Why: tests are the only objective signal that behaviour is preserved. "It looks correct" = confabulation (the model has no confidence meter).
- Do NOT proceed to Cutover without a reviewed PLAN.md. Why: the plan is the contract between human judgment and agent execution.
- After every code change, run verify commands before considering done.
