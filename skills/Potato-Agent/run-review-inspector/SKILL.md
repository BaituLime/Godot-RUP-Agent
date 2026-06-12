---
name: run-review-inspector
description: Internal review inspector step used by potato-agent-review to inspect scenario implementation paths for false pass routes and route-bypassing shortcuts.
compatibility: opencode
---

# Run Review Inspector

Use this only inside `potato-agent-review` when the packet has `review_stage = inspector`, after prior functional pass.

## Purpose

- enter implementation review through review scenario paths
- attack shortcut pass paths after functional review: hard-coded outputs, skipped UI paths, seeded state, generated reports, backend counters, editor-only routes, or source-claim downgrades
- inspect whether the code implements the approved route or swaps in fixture dependencies, debug-only branches, hard-coded outputs, duplicated pass-through wrapper layers, or test-only paths
- audit hard-coded scenario branches, proof-only flags, fixture dependencies in production, duplicated wrapper layers, conflicting ownership for the same fact or resource, leaked debug aids, unbounded hooks, unowned cleanup, and architecture drift
- emit compact inspection findings and repair deltas

## Inputs

The review Node packet must provide:

- packet `context_id`
- `source_truth_refs` with source use-case claim ids, review scenario ids, route implementation detail ids, cross-cutting contract ids, and chain refs
- `review_targets[]`
- `review_scenario_slices[]`
- `route_conformance_scope`
- `scenario_path_hypotheses[]`
- `changed_surface_scope[]`
- `test_asset_cleanup_scope[]`
- prior functional results from the same review Node context
- non-empty `prior_functional_result_refs[]` that passed
- non-empty `architecture_inspection_items[]`
- `affected_rerun_scope`

## Required Work

1. Validate the packet. If prior functional pass refs are missing or not passed, return `blocked` with `packet_insufficient`.
2. For every id/ref in `source_truth_refs`, source-artifact `must_read[]` entries, `runtime_directive_refs[]`, `active_repair_deltas[]`, `review_targets[]`, `review_scenario_slices[]`, `prior_functional_result_refs[]`, and `architecture_inspection_items[]`, find the matching source row, source section, or prior result in the approved artifacts. This includes every `review_target_id`, `source_*_ids[]`, `architecture_route_refs[]`, `inspection_item_id`, and `route_authority_ref`.
3. Read each matched row/section/result carefully end to end before inspection. Read enough surrounding text to understand the route detail, contract, forbidden simplification, scenario path, target, prior functional observation, and limit. Do not skim for the id and move on.
4. If a required id/ref cannot be found, cannot be read inside the read boundary, or conflicts with packet prose, return `partial` with `packet_insufficient` or `blocked` with the concrete conflict. Do not guess from summaries or old attempts.
5. Loop over `architecture_inspection_items[]`; do not use a prose `route_conformance_scope` sentence as the main gate.
6. For each item, use the source rows already read for its decision/contract IDs and refs.
7. For each item, identify the implementation path or a lawful `not_applicable` reason.
8. For each item, inspect implementation match, forbidden simplifications, cross-cutting contract fit, fake-pass/debug/proof residue, stale fallback, duplicated wrapper, and unowned cleanup.
9. For each item, write one `architecture_coverage[]` row with concrete files/surfaces, result, evidence, linked finding ids, and coverage limit.
10. Attack the functional pass path only for fake-pass and route-bypass evidence: MCP/editor-only routes, review-only seeds, backend counters posing as player-visible results, Verify/Headless or test-scene routes posing as normal player paths, generated proof artifacts, and source claim downgrades.
11. Inspect only bounded changed files, scenes, resources, test assets, and named architecture seams unless a route-grounded issue requires narrow expansion.
12. Do not re-decide functional pass/fail, but block settlement when the functional pass depended on an illegitimate review path.
13. Emit compact findings only when grounded in concrete route, file, scene, resource, test asset, functional observation, or source-claim context.

## Finding Rules

Each finding must include:

- `finding_id`
- `severity = blocking | warning`
- `category` must use the schema enum, and the finding text must explain the concrete mechanism: route mismatch, false pass route, route-bypassing shortcut, debug/test residue, conflicting ownership or cleanup issue, maintainability/performance issue, or scope creep.
- `anchor`
- `route_authority_refs[]`
- `source_review_scenario_ids[]`
- `source_review_target_ids[]`
- `issue`
- `impact`
- `repair_target`
- `affected_rerun_scope`

If no finding is found, inspector is not closed unless every `architecture_inspection_items[]` entry has an `architecture_coverage[]` row. `coverage_limits` must say which key scenario paths were inspected and which were not.

## Forbidden Actions

- Do not use grep or things like this.
- Do not edit implementation code.
- Do not decide functional/spec pass.
- Do not re-run or re-decide functional pass/fail.
- Do not dispatch nested agents or scheduler-like helpers.
- Do not accept lowest-effort code just because it makes the narrow review target green.
- Do not accept generated readability, recognizability, or status booleans as proof of player-visible success.

## Output Result

Return review Node inspector fields with:

- `status = ready_for_settlement`, `partial`, or `blocked`
- inspection summary with scenario path hypotheses and coverage limits
- `architecture_coverage[]` with one row per inspector packet item
- inspection findings
- repair deltas
- bounded `read_audit[]`, with entries for each packet id/ref source row, section, or prior result read and any missing/conflicting source
- bounded `search_audit[]`
