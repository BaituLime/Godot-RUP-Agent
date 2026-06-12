---
name: run-review-functional
description: Internal review functional step used by potato-agent-review to attack review scenarios inside one review Node.
compatibility: opencode
---

# Run Review Functional

Use this only inside `potato-agent-review` when the packet has `review_stage = functional`.

## Purpose

- attack approved review scenarios from source truth, not from craft or packet hints
- independently choose the user/production path most likely to expose a false pass caused by hard-coded output, skipped UI path, generated report, backend counter, editor-only route, seeded state, or source-claim downgrade
- reject forbidden substitutes
- emit repair deltas tied to review targets, review scenarios, and source claims
- return functional results inside the review Node result
- never run architecture coverage, route garbage-code inspection, fake-pass code inspection, or inspector settlement work

## Inputs

The review Node packet must provide:

- packet `context_id`
- `source_truth_refs` with source use-case claim ids, review scenario ids, route implementation detail ids, cross-cutting contract ids, and chain refs
- `review_targets[]`
- `review_scenario_slices[]`
- `regression_scope[]`
- `debug_aid_policy`
- read plan, resources, role contract, and active repair deltas

## Required Work

1. Validate the packet. If it lacks original review scenario slices for the targeted ids, return `partial` with `packet_insufficient`.
2. For every id/ref in `source_truth_refs`, source-artifact `must_read[]` entries, `runtime_directive_refs[]`, `active_repair_deltas[]`, `review_targets[]`, and `review_scenario_slices[]`, find the matching source row or section in the approved source artifact. This includes every `review_target_id`, `source_*_ids[]`, and `architecture_route_refs[]` carried by the target or scenario slice.
3. Read each matched row/section carefully end to end before choosing the attack. Read enough surrounding text to understand the claim, scenario, route detail, contract, failure examples, and forbidden substitutes. Do not skim for the id and move on.
4. If a required id/ref cannot be found, cannot be read inside the read boundary, or conflicts with packet prose, return `partial` with `packet_insufficient` or `blocked` with the concrete conflict. Do not guess from craft claims, old attempts, or packet summaries.
5. For each review scenario, reconstruct the real claim path from the rows you read: start state, operator action, expected observation, failure examples, and forbidden substitutes.
6. Identify likely cheat routes before observing: MCP/editor-only paths, backend counters, logs, generated JSON, probe booleans, Verify/Headless checks, test scenes, craft-authored proof, review-only seeds, and single-sample success.
7. If `review_surface[]` or `required_review_path` includes `normal_player_game_path`, attack the game as a normal player would see or operate it.
8. Before judging pass/fail, write a transient plain-language readback: entry path, player/operator action, visible/interactable result, and why that result carries the source claim meaning.
9. Pass only when the result records the scenario id, entry point, operator actions, observed result, required path used, forbidden substitutes checked, and the attack bounds used: files/resources/scenes examined, data variations attempted, attempt count, and time/tool limits.
10. If failure occurs, classify whether it failed at `start_state`, `operator_action`, `expected_observation`, `failure_example`, or `forbidden_substitute`.
11. Do not read debug aids before independent attack. After attack, they may only help explain failures.
12. Return functional results inside the review Node result.

## Pass Rules

- Player-facing behavior passes only through review-owned attack on `normal_player_game_path` unless the approved scenario explicitly narrows the surface.
- Debug aids, craft self-check output, Verify/Headless checks, probe booleans, generated JSON, logs, counters, MCP/editor-only paths, source inspection, screenshots without normal-player path, and env proof modes cannot prove player-facing pass.
- A backend state change does not prove a player-visible scenario unless the review also closes the player path from entry point to visible/interactable result.
- If the required review path is unavailable and no stronger approved path exists, return `partial` or `blocked`; do not substitute a weaker path.

## Output Result

Return review Node functional fields with:

- `status = ready_for_inspector`, `continue_repair`, `partial`, or `blocked`
- functional results keyed by `review_target_id` and `review_scenario_id`
- ordinary-language observation first
- regression results
- debug aid review
- repair deltas
- bounded `read_audit[]`, with entries for each packet id/ref source row or section read and any missing/conflicting source
- bounded `search_audit[]`

`ready_for_inspector` is legal only when every functional result passes. Any functional fail returns `continue_repair` with repair deltas. Missing packet truth returns `partial` with `packet_insufficient`; true environment/manual blockers return `blocked`.
