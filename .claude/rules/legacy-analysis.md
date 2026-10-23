---
globs: "LEGACY/**/*.md,SPIKE.md,CRITIC.md"
---

# Legacy Analysis Rules

When analyzing legacy code (via subagent delegation):

## Compression format for LEGACY/*.md

Each legacy module summary must follow this structure and stay ≤2k tokens:

```markdown
# Module: <name>

## Responsibility
One sentence: what this module does.

## Public Interface
- List of public methods/endpoints/exports with signatures
- What other modules call into this one

## Dependencies
- External: libraries, services, APIs
- Internal: which other project modules it imports/calls

## State & Side Effects
- What state it mutates (DB tables, files, caches, queues)
- What side effects it triggers (emails, webhooks, external API calls)

## Known Issues
- Code smells, violations of current conventions
- Known bugs (with test references if they exist)
- Coupling points that will complicate extraction

## Migration Risk: LOW | MEDIUM | HIGH
One sentence justification.
```

## Critic pass (CRITIC.md)

After all module summaries are written, do a cross-cutting review:
- Hidden dependencies not captured in individual summaries
- Shared mutable state (DB tables written by multiple modules, global singletons)
- Circular dependency chains
- Integration points that will break during cutover
- Recommended migration order (least coupled → most coupled)
