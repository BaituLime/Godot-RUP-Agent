# Session Contract

## Runtime Records

- Active Node packets record the work contract.
- Craft and review context outputs record Skill results.
- Phase settlements translate one review Node plus its reviewed craft group into phase state.
- Module session state tracks active craft, pending review, pending phase integration, stale successors, Human Final Review, Technical Final Review, blockers, and acceptance readiness.

## State Semantics

- `waiting_for_review`: craft is ready, review remains.
- `waiting_for_integration`: a review-anchored phase passed and must be committed before successor phase scheduling.
- `waiting_for_human_final_review`: runtime review scenarios are closed and the user must do Human Final Review.
- `waiting_for_technical_final_review`: Human Final Review passed and Technical Final Review remains.
- `ready_for_acceptance`: integration, Human Final Review, and Technical Final Review are satisfied.

## Forbidden Shortcut

Do not mark a module ready because craft self-checks passed. Review functional, review inspection, phase integration, Human Final Review, and Technical Final Review requirements must settle independently. Inspector review only runs after functional pass, and a phase cannot pass without inspector pass.
