---
name: review-scenario-design
description: Define independent review scenarios from approved use-case claims and AOS architecture route implementation details before Potato Agent chain planning.
compatibility: opencode
---

# Review Scenario Design (Potato Agent)

Use this Skill after `architecture-confirm` and before `chain-plan`.

This stage is a per-use-case dialogue workflow that writes independent review scenario truth one approved gate at a time. It does not add new requirements, choose Node cuts, write review targets, write packets, or create test scripts.

## Goal

Turn each approved `use_case_claims[]` row, one round at a time, plus related selected route implementation details and cross-cutting contracts into reviewable scenario attacks that later chain and review Nodes can cite.

Each scenario answers how an independent reviewer should attack the claim through a normal entry path and observable behavior:

- normal entry path
- starting state
- main user, operator, or system action
- expected visible or otherwise observable result
- likely failure examples
- forbidden substitutes
- review surfaces

## Read Boundary

Read from:

- approved `use-cases/<module_id>.md` and minimal json index
- approved `decision/<module_id>.json`
- direct project evidence only when needed to avoid impossible scenario wording
- registered read-only knowledge sources only as recall aids, confirmed by direct reads before use

## Write Boundary

Write only after explicit user approval:

- `review/<module_id>/<review_scenario_gate_id>.json` after each per-claim review scenario approval gate

Per-gate files under `review/<module_id>/` are the approved review scenario artifacts. They are written once per approved per-claim gate so the next agent can resume without relying on chat memory. There is no final review-scenarios assembly artifact.

## Scenario Standard

Each `review_scenarios[]` row must include:

- `review_scenario_id`
- `source_use_case_claim_ids[]`
- `source_route_implementation_detail_ids[]`
- `source_cross_cutting_contract_ids[]`
- `architecture_route_refs[]`
- `normal_entry_path`
- `start_state`
- `operator_action`
- `expected_observation`
- `failure_examples[]`
- `forbidden_substitutes[]`
- `review_surface[]`

The source/ref id fields are internal source wiring. Keep them in the approved gate file, but do not print them in chat or in user-facing scenario lists.

`operator_action` must name the one main user, operator, or system action being reviewed. `expected_observation` must name the expected visible or otherwise observable result. If the result is not visual, describe the concrete observation surface, such as restored state, emitted user-facing message, persisted value visible through normal product UI, or documented external integration output.

For player-facing scenarios, `review_surface[]` includes `normal_player_game_path` unless the approved claim is explicitly non-player internal behavior. Use additional surface values only when they are needed to review the same scenario. `normal_entry_path` must describe how the reviewer reaches the behavior through the ordinary product path before performing the action.

Do not write a command sequence, file list, test scene, Verify/Headless check, MCP script, or review prompt. Scenario text is the target; review Nodes choose the attack.

`source_route_implementation_detail_ids[]` must cite the selected AOS route implementation details that shape this scenario. `source_cross_cutting_contract_ids[]` must cite cross-cutting contracts that apply to this scenario, such as save/load identity, debug/proof boundary, normal-map observation, source-graph boundary, or lifecycle/resource contracts.

`architecture_route_refs[]` must cite the specific AOS route authority rows that shape the scenario attack, such as `selected_route_implementation_details[].implementation_detail_id`, implementation-detail-local `content[].id`, implementation-detail-local `forbidden_simplifications[].id`, implementation-detail-local `technical_final_review_focus[].focus_id`, `cross_cutting_contracts[].contract_id`, or cross-cutting contract `content[].id`. Cross-cutting contracts do not have separate forbidden-simplification or Technical Final Review focus arrays. Do not use this as a second requirement source.

## Per-Use-Case Dialogue

Handle exactly one `use_case_claim` per round:

1. Read existing `review/<module_id>/*.json` approved gate files before proposing the next claim round, if any exist.
2. Read the claim text and identify only the selected route implementation details and cross-cutting contracts that directly shape how that claim should be reviewed.
3. Convert the claim into one or more proposed `review_scenarios[]` rows.
4. Present only those proposed scenarios for that source claim. Do not show source claim ids, route implementation detail ids, cross-cutting contract ids, or architecture route refs in chat.
5. Ask the user to approve, reject, or revise the proposed scenarios for that claim with a gate id in the form `DG-REVIEW-SCENARIO-<module_id>-<round_or_claim_slug>-*` before moving to the next claim.
6. After approval for that claim, immediately write `review/<module_id>/<review_scenario_gate_id>.json`, validate it against `review-scenario-gate.schema.json`, and report only the gate file path and validation result.
7. After the gate file validates, continue with the next use-case claim until all claims are covered or explicitly returned upstream.

The gate file name must use the approval gate id. Do not overwrite a previous file for a different gate. If the same gate is revised after user feedback, write a new gate id or a clearly versioned file name rather than silently replacing prior approved scenarios.

Each gate file contains only the scenarios approved by that one gate plus references to prior gate paths and remaining claim ids. Do not make one growing mega file. Downstream skills read the complete set of approved gate files under `review/<module_id>/`.

If one use case contains multiple different actions or multiple different observations, split it into multiple scenarios. A scenario must have one normal entry path, one main action, and one expected result.

If a proposed scenario only names an internal implementation step without a normal entry path, normal user/operator/system action, and observable result, do not write it as a review scenario. Treat it as craft readiness check material or Technical Final Review material, depending on whether it proves implementation readiness or final route conformance.

Do not replace details with labels. Write the normal entry path, one main action, expected result, failure examples, forbidden substitutes, and review surface directly.

## Hard Rules

- Do not change use-case claims.
- Do not change architecture route implementation details or cross-cutting contracts.
- Do not create chain Nodes.
- Do not write craft self-check gates.
- Do not write review targets; `chain-plan` wires scenarios into review targets after Node cuts exist.
- Do not leave route implementation detail linkage to `chain-plan` to infer. Every scenario must already name the implementation detail ids and contract ids it depends on.
- Do not let scenario text become implementation-specific when the claim is operator-facing.
- Do not accept `headless bool`, generated JSON, backend counters, craft self-checks, test scenes, or editor-only paths as substitutes for proving a player-facing scenario through its required normal entry path, main action, and expected observation.
- Return to the owning upstream stage when approved sources cannot fill `normal_entry_path`, `start_state`, `operator_action`, `expected_observation`, `failure_examples[]`, `forbidden_substitutes[]`, and `review_surface[]` without inventing facts.

## Output

1. Present proposed `review_scenarios[]` for one source claim at a time.
2. For each scenario, show normal entry path, start state, main action, expected observation, failure examples, forbidden substitutes, and review surfaces. Do not show source claim ids, source route implementation detail ids, cross-cutting contract ids, or architecture route refs in chat.
3. Call out any source claim whose approved text and selected route implementation details cannot fill scenario fields without inventing normal entry path, start state, action, expected observation, failure example, forbidden substitute, review surface, or implementation detail linkage.
4. Ask for explicit approval for that claim's scenarios with `DG-REVIEW-SCENARIO-<module_id>-<round_or_claim_slug>-*` before proceeding to the next claim.
5. After each claim approval gate, write and validate the per-gate approved scenario file before continuing.
6. After all claim rounds are approved, report the completed approved gate file set under `review/<module_id>/`. Do not assemble a separate review-scenarios markdown or json file.
