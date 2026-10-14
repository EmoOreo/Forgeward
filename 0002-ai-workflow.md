# Decision 0002 — AI-assisted development workflow

**Status:** Accepted  
**Date:** 2026-10-14

## Context
The project will rely heavily on AI/LLM assistance. Large tasks can cause scope drift, unrelated changes, and unverified claims of success.

## Decision
Use one design source of truth, a separate report-only reviewer, and one implementation task at a time. Each coding task requires acceptance criteria, tests, a change summary, and actual build/test evidence.

## Workflow
1. ChatGPT prepares and reconciles project specifications.
2. DeepSeek or another independent reviewer audits for contradictions and risks in report-only mode.
3. The design lead resolves accepted findings and updates authoritative documents.
4. Jules or another coding agent implements one bounded task.
5. Verify the diff, run tests/build, record actual results, and checkpoint the repository.

## Consequences
- Less parallel duplication and fewer competing specifications.
- More review overhead per task, but lower risk of broad regressions.
- Agents must not silently alter core design decisions.
- Unverified build/test claims are not accepted as completion evidence.
