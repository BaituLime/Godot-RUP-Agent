# Handoff Contract

## Planning To Runtime

Approved use-case, architecture, review-scenario, and chain truth must be carried into runtime handoffs explicitly. Runtime may not infer behavior, review scenarios, final review state, or acceptance from chat memory or old artifacts.

## Source Wiring

Use-case design owns `use_case_claims[]`. Architecture owns the approved route id, `selected_route_implementation_details[]`, implementation-detail-local `purpose_summary`, implementation-detail-local `content[]`, implementation-detail-local `forbidden_simplifications[]`, implementation-detail-local `technical_final_review_focus[]`, cross-cutting contract `content[]`, and open risks. Review-scenario design owns approved gate files under `review/<module_id>/`. Chain cuts phase-shaped craft/review Nodes, creates craft self-check gates, and writes review targets that wire use-case claim ids plus review scenario ids to review and repair while preserving architecture through implementation detail ids, contract ids, and inspector scope. Runtime packets read source artifacts by those ids and carry the subset needed by the current stage, including review scenario source slices when a review functional stage is dispatched.

Craft runs chain-created readiness gates inside `potato-agent-craft`. Review attacks chain `review_targets[]` through the cited review scenarios and inspects whether code and test support can be trusted inside `potato-agent-review`.

## Runtime Roles

- craft Nodes dispatch to `potato-agent-craft`, which implements, self-checks, and repairs in one resumable craft context.
- review Nodes dispatch to `potato-agent-review` in stages: functional first, inspector only after functional pass. The review context drains after each returned stage result.

## Repair Handoff

Review functional failures produce repair deltas with `review_target_ids[]` and source ids.
Review inspection failures produce repair deltas with `inspection_finding_ids[]`.
Scheduler backtracks those deltas to craft, where they become craft self-check gates. If functional fails or blocks, inspector is not run. The drained review context is not resumed for implementation repair. Repair runs through `potato-agent-craft`; the target craft Node's existing craft context is resumed when lawful, otherwise a fresh craft context is started for that target craft Node. Stale review targets or inspection focus rerun later in a new review invocation.

After runtime review closes planned review scenarios, the runner runs Human Final Review. Human Final Review failure enters craft-only paired repair and does not dispatch a review Node. Human Final Review pass is required before Technical Final Review. Technical Final Review failure routes to craft repair and may make the prior Human Final Review stale by user decision.

## Forbidden Handoff Shapes

- template-generated producer prompt authority
- craft-generated pass output as evidence authority
- old packet/attempt/settlement shapes as current form authority
- command-wrapper dispatch
- model-tier dispatch fields
