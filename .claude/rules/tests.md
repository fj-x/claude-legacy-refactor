---
globs: "**/test_*,**/tests/**,**/*_test.*,**/*Test.*,**/*_spec.*"
---

# Characterization Testing Rules

## Purpose

Characterization tests capture **current behaviour as-is**, not desired behaviour. They are the objective signal that refactoring preserves functionality.

## Rules

- Test the public interface of each module being refactored
- Include edge cases discovered during archeology (documented in `LEGACY/*.md`)
- Known bugs get their own test with a comment: `# Known defect: <description>` — do NOT fix during refactoring
- Name tests descriptively: `test_<module>_<scenario>_<expected_outcome>`
- Each test must be independently runnable and deterministic (no sleep, no network, no shared mutable state between tests)
- Cover both happy paths and error paths — legacy code often has undocumented error handling that callers depend on
- When the legacy module has side effects (DB writes, API calls), mock/stub the side effect boundary, not the module itself — you're testing the module's logic, not the infrastructure

## Before Cutover

All characterization tests must pass on the legacy code BEFORE any refactoring begins. If a test fails on legacy code, the test is wrong — fix the test, not the code.
