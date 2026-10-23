Perform archeology on the legacy module I'll specify.

**Critical rule: delegate each file/submodule analysis to a subagent.** Do NOT read raw legacy code in this main session.

For each file/submodule in the target area:
1. Spawn a subagent to analyze it
2. Subagent reads the code and produces a compressed summary following the format in `.claude/rules/legacy-analysis.md`
3. Write each summary to `LEGACY/<module-name>.md`

After all summaries are done, perform a cross-cutting critic pass:
- Read all `LEGACY/*.md` summaries (NOT the source code)
- Identify hidden dependencies, shared state, circular deps, integration risks
- Write findings to `CRITIC.md`
- Recommend migration order (least coupled → most coupled)

Create the `LEGACY/` directory if it doesn't exist.
