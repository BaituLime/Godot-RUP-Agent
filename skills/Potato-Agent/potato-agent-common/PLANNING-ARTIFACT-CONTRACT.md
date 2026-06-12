# Planning Artifact Contract

## Use Case

Use-case design owns operator-facing behavior, success experience, and source `use_case_claims[]`.

## Architecture

Architecture owns the approved route id, `selected_route_implementation_details[]`, implementation-detail-local `purpose_summary`, implementation-detail-local `content[]`, implementation-detail-local `forbidden_simplifications[]`, implementation-detail-local `technical_final_review_focus[]`, cross-cutting contract `content[]`, and open risks. Route choice discussion happens in chat before approval and is not stored as the main Decision Handoff body. Architecture does not create a second source-id checklist.

## Review Scenario

Review-scenario design owns per-gate approved scenario files under `review/<module_id>/`: independent attack targets derived from approved use-case claims, selected route implementation details, and cross-cutting contracts. It does not create new requirements, write review targets, write packets, or cut Nodes. Downstream planning and runtime skills consume the complete set of approved `review/<module_id>/*.json` files; there is no separate review-scenarios assembly artifact.

## Chain

Chain owns Node cuts, craft self-check gates, and review targets that wire source use-case claims plus review scenarios into craft and review Nodes while preserving architecture route implementation details and cross-cutting contracts through ids and inspector scope. Phase remains represented by the review Node boundary: phase settlement is anchored by `review_node_id` and covers `review_plan.reviewed_craft_node_ids[]`. Chain does not copy source prose; packet preparation reads source artifacts by id.

## Runtime

Runtime handoffs copy planning truth; they do not author new tests, weaker surfaces, new review scenarios, or new acceptance bars.
