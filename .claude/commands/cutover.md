Execute the refactoring plan from PLAN.md.

Read `PLAN.md` and follow it step by step.

For each use case in the plan:

1. Implement the new code (or restructure existing)
2. Update DI bindings / imports to point to the new implementation
3. Run ALL tests (characterization + any new tests)
4. If tests pass: `git add` + `git commit` with conventional commit format `refactor(module): description`
5. If tests fail: revert changes, diagnose, fix, and retry. NEVER proceed on a broken state
6. Log the result in `REFACTOR_LOG.md`:
   ```
   ## Commit N: <hash>
   - Description: <what was done>
   - Tests: <pass count>/<total count>
   - Notes: <any issues encountered>
   ```

After all use cases are done:
1. Run the full test suite one final time
2. Record final status in REFACTOR_LOG.md
3. Summarize what was accomplished and what remains (if anything)
