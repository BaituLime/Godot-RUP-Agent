# Acceptance Program

Acceptance consumes already-settled runtime truth. It does not rerun the runtime chain.

## Inputs

- review scenario closure state
- review functional source-judgment results
- Human Final Review record
- review inspection results
- Technical Final Review record
- integration/mainline state

## Rules

- Craft checks are readiness records, not acceptance evidence.
- Review functional can support functional/spec evidence only when review targets survived independent source-first attack.
- Review inspection can block acceptance for code quality but cannot prove functional behavior.
- Human Final Review can support operator-visible evidence only for items explicitly checked.
- Technical Final Review can support evidence for implementation detail refs, cross-cutting contract refs, implementation-detail-local `content[]`, implementation-detail-local `forbidden_simplifications[]`, implementation-detail-local `technical_final_review_focus[]`, cross-cutting contract `content[]`, changed files, resource ownership, debug/test residue, and performance-sensitive loops, but cannot substitute for functional behavior settlement.
- Acceptance fails or blocks when relevant review scenarios are open, stale, or missing.
