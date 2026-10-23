---
globs: "PLAN.md,REFACTOR_LOG.md"
---

# Cutover Planning Rules

## PLAN.md Structure

```markdown
# Refactoring Plan

## Scope
What is being refactored and why (link to SPIKE.md verdict).

## Migration Order
Ordered list of use cases, each becoming one atomic commit:

### 1. <Use Case Name>
- **Files changed:** list
- **New files:** list
- **DI changes:** old binding → new binding
- **Tests:** which characterization tests cover this
- **Rollback:** how to undo (typically: revert commit + restore old DI binding)

### 2. <Next Use Case>
...
```

## Execution Rules

- One use case = one atomic commit. Never batch unrelated changes
- Each commit must leave the project in a green state (all tests pass)
- DI swap (old implementation → new implementation) is the last line of each commit — so the swap is easily revertable by changing one line
- Log every commit in `REFACTOR_LOG.md` with: hash, description, test result (pass/fail count), notes
- If a commit breaks tests: revert immediately, diagnose, fix, re-commit. Do NOT stack commits on a broken state
- When all use cases are done: run the full test suite one final time and record in REFACTOR_LOG.md
