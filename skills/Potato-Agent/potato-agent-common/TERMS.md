# Potato Agent Terms

## Runtime Nodes

- `craft`: active Node kind that implements one bounded scope and checks its own readiness.
- `review`: active Node kind that independently tests behavior and inspects code quality for a bounded scope.
- `context_id`: packet context id for the active craft or review Node agent context. Craft contexts are resumable for same-Node repair; review contexts are drained after the returned review stage.
- `Phase Settlement`: one settlement anchored by `review_node_id` that covers the review Node plus `review_plan.reviewed_craft_node_ids[]`.

## Live Skill Sequences

- Craft Nodes dispatch to `potato-agent-craft` until the Node is ready, blocked, or must replan.
- Review Nodes dispatch to `potato-agent-review` for functional review first. Inspector review dispatch happens only after functional pass.

## Testing Spine

- `Use-Case Claim`: canonical user-facing source claim owned by use-case design.
- `Architecture Route Authority`: approved route id plus selected implementation details and cross-cutting contract content owned by architecture-confirm. Runtime/review cite route implementation details, implementation-detail-local `purpose_summary`, implementation-detail-local `content[]`, implementation-detail-local `forbidden_simplifications[]`, implementation-detail-local `technical_final_review_focus[]`, and cross-cutting contract `content[]`.
- `Review Scenario`: independent attack target derived from approved use-case claims, selected route implementation details, and cross-cutting contracts.
- `Review Target`: chain-owned wiring from source use-case claim ids and review scenario ids to reviewed craft Nodes, required review path, forbidden substitutes, repair targets, and stale successors.
- `Craft Self-Check Gate`: chain-created craft-side readiness gate derived from source ids, architecture constraints, and repair deltas.
- `Review Functional Run`: review-owned hostile functional/spec attack of review targets through review scenarios.
- `Review Inspection Finding`: review-owned code-quality or architecture finding.
- `Phase Integration`: module worktree commit for one passed phase settlement before successor phase scheduling.
- `Human Final Review`: user's final functional-experience gate after runtime review scenarios close.
- `Technical Final Review`: execution-stage code and route-quality review after Human Final Review passes.

## Authority

- Craft self-check results are readiness/support only.
- Review functional runs own automated functional/spec evidence.
- Inspector review can pass a phase only after functional pass; a phase cannot pass without inspector pass.
- Human Final Review owns explicit user functional-experience evidence.
- Review inspection owns code-quality and architecture fitness findings.
- Technical Final Review owns final evidence for implementation detail refs, cross-cutting contract refs, implementation-detail-local `content[]`, implementation-detail-local `forbidden_simplifications[]`, implementation-detail-local `technical_final_review_focus[]`, cross-cutting contract `content[]`, changed files, resource ownership, debug/test residue, and performance-sensitive loops.
- Acceptance decides final module acceptance after use-case claim judgments, review scenario closure, architecture route-conformance state, Human Final Review, Technical Final Review, and integration are assembled.

## Historical Surface

Older runtime artifacts, packet kinds, and attempt/settlement shapes are historical/provenance only. They are not current runtime authority or aliases.
