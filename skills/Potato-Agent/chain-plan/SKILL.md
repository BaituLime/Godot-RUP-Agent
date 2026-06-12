---
name: chain-plan
description: Compile approved use-case claims, architecture route implementation details, and review scenarios into craft and review Nodes.
compatibility: opencode
---

# Chain Plan (Potato Agent)

Use this Skill after approved use-case design, approved architecture route implementation details/cross-cutting contracts, and approved review scenario files exist under `review/<module_id>/`.

Read the complete approved `review/<module_id>/*.json` set before cutting Nodes. If no approved gate file shows `remaining_use_case_claim_ids = []`, stop and return to `review-scenario-design` instead of building a partial chain.

There is no separate route-synthesis or review-plan gate before chain. If approved artifacts cannot assign each Node source claim ids, review scenario ids, readiness gates, review targets, required review path, inspector scope, and repair routing, stop and return to the owning upstream stage or request bounded evidence for one named uncertainty.

## Runtime Node Kinds

Active Node kinds are only:

- `craft`
- `review`

The scheduler derives dispatch from `node.kind`: `craft` -> `potato-agent-craft`, `review` -> `potato-agent-review`.

## Required Chain Shape

Write `chain_format = potato_agent_chain_handoff/v18`.

The chain must include:

- `chain_contract.current_head_node_id`
- `chain_contract.node_repair_policy`
- `chain_contract.local_insertion_policy`
- Nodes with explicit `kind = craft | review`
- craft Nodes with `craft_plan`
- review Nodes with `review_plan`
- `source_route_implementation_detail_ids[]` and `source_cross_cutting_contract_ids[]` for every Node
- `must_read[]` and `likely_read[]` derived from the cited route implementation details' `content[]`, `forbidden_simplifications[]`, and `technical_final_review_focus[]`
- resource/MCP posture for every Node

Do not write alternate route-source fields, realization ids, witness refs, gate refs, or separate review-plan fields.

## Scenario-Based Phase Planning

First explain the work as scenario-based conceptual phases. A phase is a planning and user-explanation group, not a schema field, and must not be written to chain JSON.

Approved review scenarios from `review/<module_id>/*.json` are the source of conceptual phases. Each approved review scenario normally drives one conceptual phase: the phase's craft group implements the production behavior needed for that scenario, then that phase's review Node records whether that scenario passed, failed, or needs repair through its required review path and inspector scope.

Multiple scenarios may be grouped into one conceptual phase only when they share the same normal entry path, the same main action, and the same expected result, and the user explicitly confirms the grouping. If any of those differ, keep them as separate conceptual phases.

Do not write phase ids, labels, acceptance fields, top-level Node goals, success criteria, or ownership boundaries into chain JSON. Instead, wire the scenario behavior through source ids, the phase craft Nodes, the phase review Node's `reviewed_craft_node_ids[]`, and `review_targets[]`.

## Craft Node Cutting

Craft Nodes are implementation, repair, or context units under one scenario-based conceptual phase. One scenario may need multiple craft Nodes before its review Node. A phase craft Node must serve exactly that phase's driving review scenario, or the scenarios in that phase's user-confirmed scenario group. Do not make one craft Node feed several later review Nodes for different phases.

If several scenarios appear to need the same production work, choose one of these routes before writing chain JSON:

- group the scenarios into one conceptual phase only when they share the same normal entry path, main action, and expected result, and the user explicitly confirms the grouping
- otherwise keep separate phases, with each phase carrying its own scenario-owned craft group and review Node

Foundation work that is genuinely shared may be a predecessor craft Node, but it is not a scenario phase by itself, must not be the only reviewed craft for several scenario review Nodes, and must not claim any scenario pass. Each later scenario phase still needs its own phase-owned craft group, even if that group is small.

Each craft Node must state:

- which single review scenario, or user-confirmed scenario group, it serves
- which architecture route implementation details and cross-cutting contracts it implements or preserves
- which route implementation details or next-step files must be read before editing
- which self-check gates prove readiness only
- what cannot be claimed as passed until the review Node checks the scenario

Every phase craft Node must carry the driving `served_review_scenario_id` and at least one `source_route_implementation_detail_ids[]` entry. Foundation work should cite the later scenario it enables and the route implementation detail it implements or protects, while stating that it does not prove that scenario by itself.

Do not split work merely by file, function, variable, fixture, check, or parameter. Keep coupled production pieces together when one craft context must understand and repair them together.

## Review Node Placement

Plan review after the craft group for a scenario-based conceptual phase, not after every craft Node by default. Do not make one craft Node plus one review Node per scenario mandatory; use as many craft Nodes as the scenario needs, then exactly one review Node for that scenario or user-confirmed scenario group.

Do not produce a topology where one non-foundation craft Node is reviewed by multiple later review Nodes for different scenarios. That means the phase was cut at an implementation layer instead of at the approved scenario layer.

Each review Node must:

- name the minimal `reviewed_craft_node_ids[]` that create the phase behavior
- carry `review_targets[]` tied to `source_use_case_claim_ids[]` and `source_review_scenario_ids[]`
- include inspector scope that enters through scenario paths and architecture route implementation details/contracts
- include cited implementation detail ids and contract ids for the implementation details/contracts under review

Packet preparation reads approved source artifacts by the Node and review target source ids; do not duplicate use-case, review-scenario, or architecture reference slices into the chain.

Foundation-only work can still be reviewed for selected implementation detail conformance, test asset state, or repair risk, but it must not prove a player-facing use-case claim by itself.

## Review Target Rules

- A review target is wiring, not a new requirement and not a test script.
- Every review target must include `source_use_case_claim_ids[]` and `source_review_scenario_ids[]`.
- Every review target must include `source_route_implementation_detail_ids[]` and `source_cross_cutting_contract_ids[]` copied from the cited review scenarios and narrowed only when the chain node covers a strict subset of a scenario.
- Review target ids point packet preparation to the approved source artifacts that preserve the actual obligation; do not copy source prose or reference slices into the chain.
- Player-facing, operator-visible, visual readability, recognizability, normal launch, click, restore, or map semantics scenarios whose `review_surface[]` includes `normal_player_game_path` must use `required_review_path = normal_player_game_path`.
- `production_integration_probe` is allowed only for non-player internal integration/regression behavior.
- Do not write `setup`, `stimulus`, `observation`, `observation_order`, or red-team prompt text for review.
- Do not name Verify/Headless checks, generated JSON, status booleans, screenshots, or test-scene output as the primary pass route for player-facing scenarios.
- Every repairable functional failure must be routable to a craft Node repair self-check gate.
- Every repairable inspector finding must be routable to a craft Node repair self-check gate.
- Do not add new JSON fields to mark scenario ordering or horizontal check types.

## Repair Rules

- Review functional failures backtrack through `review_target_id`, `review_scenario_id`, source use-case claim ids, and implementation detail ids.
- Review inspector failures backtrack through `inspection_finding_id`, implementation detail ids, cross-cutting contract ids, and route-conformance scope.
- Backtracking targets craft Nodes, not review Nodes.
- If repair changes downstream behavior or implementation seams, mark affected successor review targets, review scenarios, or inspection focus stale.
- If the failure changes use-case, architecture, review scenarios, proof rigor, resource rules, or chain structure, stop for visible replanning.

## Consistency Gate

After writing `chain/<module_id>.json`, confirm:

- it validates against `chain-handoff.schema.json`
- every Node kind is `craft` or `review`
- every craft Node has self-check gates and served review scenario ids
- every review Node has review targets and inspector step plans
- every review target maps to approved use-case claim ids and approved review scenario ids
- every review target and craft self-check gate maps to approved implementation detail ids, plus cross-cutting contract ids when applicable
- every Node carries implementation detail ids, contract ids, and bounded `must_read[]`/`likely_read[]` file or surface anchors derived from those implementation details
- no alternate route-source, separate review-plan, witness, or gate-ref fields remain

## Output

1. First list the scenario-based phase explanation used for sequencing; state that phases are not written as chain schema fields.
2. For every phase, name the driving review scenario or user-confirmed scenario group, and summarize the normal entry path, main action, expected result, phase-owned craft group, and exactly one review Node that records pass, fail, or repair-needed result.
3. Then present the proposed chain compactly.
4. Summarize each craft and review Node inline in topological order.
5. Call out review scenarios, review targets, and repair routing.
6. After approval, write the chain and archive any superseded revision.
7. Validate the written chain before returning success.
