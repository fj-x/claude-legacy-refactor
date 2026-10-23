Create a refactoring plan based on the archeology artifacts.

Read these files (do NOT read raw legacy source):
- `SPIKE.md` — scope and verdict
- `LEGACY/*.md` — compressed module summaries
- `CRITIC.md` — cross-cutting risks and recommended order

Then:

1. **Write characterization tests** that capture the current behaviour of every public interface being refactored. Follow the rules in `.claude/rules/tests.md`. Run the tests — they must all pass on the existing code before any refactoring begins.

2. **Write PLAN.md** following the format in `.claude/rules/cutover.md`:
   - Migration order (from CRITIC.md recommendation)
   - For each use case: files changed, new files, DI changes, covering tests, rollback strategy
   - Estimated number of atomic commits

3. Present the plan for human review. Do NOT proceed to cutover without approval.
