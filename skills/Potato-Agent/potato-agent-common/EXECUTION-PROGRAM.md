# Execution Program

## Runtime Spine

1. Runtime packet preparation writes one active Node packet.
2. Scheduler dispatches a `craft` Node to `potato-agent-craft` or a `review` Node to `potato-agent-review`.
3. Craft implements, self-checks, and repairs in one resumable packet `context_id`; scheduler controls continuation by resuming the same subagent context.
4. Review functional runs first for one review Node phase.
5. If functional fails or blocks, phase settlement records rework or blocker and inspector does not run.
6. If functional passes, review inspector runs for the same review Node phase.
7. Phase settlement gates and creates repair deltas when needed.
8. Passing phase settlement is integrated as a module worktree commit before successor phase scheduling.
9. Scheduler backtracks repair deltas to craft.

## Craft Check

Craft gates are mandatory self-check gates. The gates come from use-case claims, selected route implementation details/cross-cutting contracts, chain truth, and repair deltas.

Passing craft gates means ready for independent review, not acceptance.

## Review Functional

Review functional attacks chain review targets through review scenarios independently. It must attack the source-derived truth path before reading debug aids, and debug aids cannot prove pass.

## Review Inspector

Review inspector enters through review scenario implementation paths, then examines false-pass routes, selected route implementation detail/contract fit, cited `content[]`, cited `forbidden_simplifications[]`, performance-sensitive loops, debug aid leftovers, and test asset integrity.

Review inspector only runs after functional pass. A phase cannot pass without inspector pass.

## Final Reviews

After planned runtime review scenarios close, the runner prepares Human Final Review. A failing Human Final Review enters craft-only paired repair and does not dispatch a review Node. A passing Human Final Review is required before Technical Final Review. Technical Final Review checks implementation detail refs, cross-cutting contract refs, implementation-detail-local `content[]`, implementation-detail-local `forbidden_simplifications[]`, implementation-detail-local `technical_final_review_focus[]`, cross-cutting contract `content[]`, changed files, resource ownership, debug/test residue, and performance-sensitive loops before acceptance dossier assembly.
