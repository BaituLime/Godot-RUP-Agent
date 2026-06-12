---
name: prepare-packet
description: Compile one craft or review Node packet from approved chain source wiring, review scenarios, and active repair truth.
compatibility: opencode
---

# Prepare Node Packet (Potato Agent)

Use this Skill to compile exactly one runtime Node packet for one craft or review Node.

The packet is the Node agent invocation's only live contract. It must be compiled from approved use-case claims, approved review scenarios, architecture route implementation details/cross-cutting contracts, chain truth, module state, active repair deltas, and the current craft/review context. It may not invent tests, relax tests, or use old attempts as templates.

## Read Boundary

Read from:

- current `prepare-packet` skill and active Node packet schema
- active chain handoff
- active module/session state
- worktree table
- approved use-case design
- approved `review/<module_id>/*.json` review scenario files, including original scenario prose
- approved architecture/decision handoff, including `selected_route_implementation_details[]`, implementation-detail-local `purpose_summary`, implementation-detail-local `content[]`, implementation-detail-local `forbidden_simplifications[]`, implementation-detail-local `technical_final_review_focus[]`, and cross-cutting contract `content[]`
- chain-owned source wiring: node source ids, review scenario ids, review targets, and self-check gates
- active repair deltas and stale-successor state
- current craft/review context results as facts

Historical attempts are facts only after current schema and current chain truth are established.

## Write Boundary

Write only the active Node packet path for this session/module/scope.

## Common Packet Rules

- Write one packet for `node.kind = craft` or `node.kind = review`.
- Do not write model-tier dispatch fields, command-wrapper names, or step skill dispatch fields.
- Include `source_truth_refs` naming use-case refs, review-scenario refs, architecture route implementation detail/contract refs, source use-case claim ids, source review scenario ids, source route implementation detail ids, source cross-cutting contract ids, and chain refs.
- Include bounded `read_plan` with `forbidden_roots` containing `/` and `/home/bunny`.
- Include `artifact_source_ledger`; current packet schema and this skill are shape sources, old attempts are facts only.
- Include `active_repair_deltas[]`; ordinary packets use an empty array.
- Include exactly one Node payload: `craft_node_packet` or `review_node_packet`.
- Validate the written packet before returning.

## Craft Node Packet

Build `craft_node_packet` from the cited route implementation detail slice, served review scenario ids, implementation-detail-local `content[]`, self-check gates, active repair/stale-successor gates, and current craft context.

It must contain:

- `route_slice`
- `execution_scope`
- `served_review_scenario_ids[]`
- `allowed_file_scope[]`
- `required_test_asset_policy[]`
- `prior_check_failures[]`
- `debug_aid_policy[]`
- `self_check_gates[]`
- `test_layers[]`
- `changed_files_under_check[]`
- `debug_test_cleanup_policy[]`
- `rerun_scope[]`

Craft must read use-case, review scenarios, route implementation details/cross-cutting contracts, and chain slice before gates or old attempts. Craft self-check gates are readiness constraints, not functional pass or claim settlement.

If the chain Node lacks implementation detail ids or route content needed to form `must_read[]`, `likely_read[]`, `allowed_file_scope[]`, and `files_expected_to_change[]`, return packet insufficiency instead of guessing files from old attempts.

## Review Node Packet

Build `review_node_packet` for exactly one `review_stage`: `functional` or `inspector`. Review packet preparation is stage-specific; do not prepare a packet that asks one review invocation to run both stages.

For `review_stage = functional`, build from chain `review_targets[]`, review scenario slices, regression/debug policy, changed files, and stale successor focus. Functional packets contain review targets, review scenario slices, regression/debug policy, and no architecture inspection items.

For `review_stage = inspector`, prepare the packet only after a same-review-Node functional attempt passed. Build `architecture_inspection_items[]` by ID from review scenario `architecture_route_refs[]`, source route implementation detail ids, source cross-cutting contract ids, and review target ids. Include `prior_functional_result_refs[]`. Do not copy long architecture prose into the packet; use refs and ids so the inspector reads the source rows directly.

Read the approved source artifacts by ID to recover original use-case, review scenario, and architecture text; the chain does not carry reference slices.

It must contain:

- `review_targets[]`
- `review_stage`
- `review_scenario_slices[]` with the original scenario text fields, including `review_surface[]`
- each review scenario slice's `source_route_implementation_detail_ids[]`
- each review scenario slice's `source_cross_cutting_contract_ids[]`
- each review scenario slice's `architecture_route_refs[]`
- `regression_scope[]`
- `debug_aid_policy`
- `route_conformance_scope`
- `scenario_path_hypotheses[]`
- `changed_surface_scope[]`
- `test_asset_cleanup_scope[]`
- `prior_functional_result_refs[]`
- `architecture_inspection_items[]`
- `affected_rerun_scope`

Rules:

- The review Node packet is stage-driven. Functional review runs first. Inspector review may run only from a separate inspector packet after functional pass.
- Functional packets must set `review_stage = functional`, may have empty `prior_functional_result_refs[]`, and must have empty `architecture_inspection_items[]`.
- Inspector packets must set `review_stage = inspector`, must have non-empty `prior_functional_result_refs[]`, and must have non-empty `architecture_inspection_items[]` derived from cited route/scenario/target ids.
- Review must read use-case claims, review scenarios, architecture route implementation details/cross-cutting contracts, and review targets before debug aids.
- Debug aids, generated JSON, Verify output, probes, and logs cannot prove pass. They may only help explain failures after independent attack.
- Player-visible scenario targets whose `review_surface[]` includes `normal_player_game_path` must use `required_review_path = normal_player_game_path`, not a path script. Read forbidden substitutes from the approved review scenario gate by ID when needed.
- The packet must preserve the positive player obligation in plain language. If it can only describe a check file, boolean, generated report, or implementation seam, return packet insufficiency.
- MCP may be authorized as a tool, but packet preparation must not make MCP the proof surface.

## Repair Packet Rules

- Preserve each active repair delta's failure signature.
- Preserve `review_target_ids[]`, `review_scenario_ids[]`, source use-case claim ids, source route implementation detail ids, cross-cutting contract ids, and `inspection_finding_ids[]` when present.
- For functional failures, write craft self-check gates that require the failed review target and review scenario to be checked before any later review rerun.
- For Human Final Review failures, write craft-only repair gates and do not prepare a review packet unless the user chose visible replanning.
- For Technical Final Review failures, write craft repair gates and mark whether Human Final Review may be stale.
- Do not turn a repair delta into prose only.

## Consistency Gate

After writing the packet, confirm:

- current packet contract validates it
- exactly one Node payload is present
- scheduler can derive the Node agent from `node.kind`
- read/search boundaries are bounded
- craft packets are route-first and contain served review scenario ids
- review packets contain review targets plus original review scenario slices
- no obsolete route-source, review-plan, witness, gate-ref, or old final-review fields remain
