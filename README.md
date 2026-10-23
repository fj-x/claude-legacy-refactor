# Claude Code Legacy Refactoring Scaffold

A production-ready scaffold for **safe, systematic legacy code refactoring** using Claude Code. Based on the 4-Step Legacy Refactoring Protocol.

Copy this scaffold into any project before starting a legacy refactoring. Claude Code will automatically load the protocol, guards, and rules at every session start.

## Quick Start

```bash
# 1. Copy scaffold into your project
cp -r .claude /path/to/your-project/.claude
cp CLAUDE.md /path/to/your-project/CLAUDE.md
# Merge .gitignore with your existing one (don't overwrite)

# 2. Customize CLAUDE.md
#    - Replace the Architecture section with your project's actual module map
#    - Replace Verify commands with your project's test/lint commands

# 3. Add stack-specific permissions to .claude/settings.json
#    - Add your test runner to "allow" (e.g., "Bash(pytest *)", "Bash(npm test *)")
#    - Add stack-specific dangerous commands to "deny"

# 4. Commit the scaffold
cd /path/to/your-project
git add .claude/ CLAUDE.md .gitignore
git commit -m "chore: add legacy refactoring scaffold"

# 5. Start Claude Code from project root and run the protocol
claude
# Then use: /project:spike, /project:archeology, /project:plan, /project:cutover
```

## The 4-Step Protocol

Each step runs in its own Claude Code session. Between steps: `/clear`. Continuity lives in files, not chat history.

### Step 1: Spike → `SPIKE.md`

**Command:** `/project:spike`

Feasibility check in Plan mode (read-only). Analyzes churn, technical debt signals, module isolation, and blast radius. Outputs a GO / NO-GO / CONDITIONAL verdict.

**When to stop:** If verdict is NO-GO, the module isn't ready for refactoring. Address the blocking issues first.

### Step 2: Archeology → `LEGACY/*.md` + `CRITIC.md`

**Command:** `/project:archeology`

Deep analysis of the legacy code via **subagent delegation** — the main agent never reads raw legacy source directly. Each file/submodule gets analyzed by a subagent that produces a compressed summary (≤2k tokens). Then a cross-cutting critic pass identifies hidden dependencies, shared state, and recommends migration order.

**Why subagents?** Raw legacy code in the main context = attention dilution for the entire remaining session + confabulation surface where the model mixes old and new code. Subagents isolate this: the main agent works from clean summaries, not messy source.

### Step 3: Plan → Characterization Tests + `PLAN.md`

**Command:** `/project:plan`

Two deliverables:
1. **Characterization tests** that capture current behaviour as-is (including known bugs — refactoring ≠ bug fixing)
2. **Migration plan** with ordered use cases, each becoming one atomic commit

**Human gate:** Review PLAN.md before proceeding. This is the contract between your judgment and the agent's execution.

### Step 4: Cutover → `REFACTOR_LOG.md`

**Command:** `/project:cutover`

Executes the plan step by step. Each use case = one atomic commit with DI swap + green tests. If tests fail: revert, diagnose, fix, re-commit. Never stack commits on a broken state.

## File Inventory

```
your-project/
├── .gitignore                            # Layer 1: hygiene
├── CLAUDE.md                             # Layer 3: protocol + guardrails
├── .claude/
│   ├── settings.json                     # Layer 2: enforcement (deny/allow/env)
│   ├── commands/
│   │   ├── spike.md                      # /project:spike — Step 1
│   │   ├── archeology.md                 # /project:archeology — Step 2
│   │   ├── plan.md                       # /project:plan — Step 3
│   │   └── cutover.md                    # /project:cutover — Step 4
│   └── rules/
│       ├── legacy-analysis.md            # Layer 4: analysis format (globs: LEGACY/**)
│       ├── tests.md                      # Layer 4: characterization test rules
│       └── cutover.md                    # Layer 4: plan/execution rules
├── SPIKE.md                              # [generated] Step 1 output
├── LEGACY/                               # [generated] Step 2 output
│   ├── module-a.md
│   └── module-b.md
├── CRITIC.md                             # [generated] Step 2 output
├── PLAN.md                               # [generated] Step 3 output
└── REFACTOR_LOG.md                       # [generated] Step 4 output
```

**Layer 1 (`.gitignore`)** — Keeps secrets and personal config out of git.
**Layer 2 (`settings.json`)** — Client-side enforcement. Deny rules intercept tool calls BEFORE execution. The model cannot bypass them, even under prompt injection.
**Layer 3 (`CLAUDE.md`)** — Persistent system-prompt injection. Re-read from disk on every session, survives `/clear` and `/compact`.
**Layer 4 (`rules/`)** — Conditional behaviour loaded only when the agent touches matching files. Zero attention cost when irrelevant.

## Customization Guide

### Adding stack-specific tools

Edit `.claude/settings.json`:

```jsonc
{
  "permissions": {
    "allow": [
      // Add your verify-tools here so they auto-approve:
      "Bash(pytest *)",           // Python
      "Bash(npm test *)",         // Node.js
      "Bash(go test ./...)",      // Go
      "Bash(php vendor/bin/phpunit *)",  // PHP
      "Bash(cargo test *)"        // Rust
    ],
    "deny": [
      // Add stack-specific dangerous commands:
      "Bash(python -c *)",        // Python eval escape
      "Bash(npm install -g *)",   // Node.js global install
      "Bash(composer global *)"   // PHP global install
    ]
  }
}
```

### Adding stack-specific rules

Create `.claude/rules/your-language.md`:

```markdown
---
globs: "**/*.py"
---
# Python conventions for this project
- Type hints on all public functions
- Use Pydantic v2 for data validation
- ...
```

### Personal overrides (not in git)

Create `.claude/settings.local.json`:

```json
{
  "model": "opus",
  "permissions": {
    "allow": ["Bash(my-personal-tool *)"]
  }
}
```
