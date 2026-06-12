---
name: run-craft-check
description: Internal craft self-check step used by potato-agent-craft without a separate attempt file.
compatibility: opencode
---

# Run Craft Check

Use this only inside `potato-agent-craft`.

## Purpose

- run mandatory craft self-check gates
- run applicable unit, integration, regression, runtime, build/import, and debug/test cleanup checks
- produce structured failures for scheduler continuation in the same craft context
- return self-check results inside the craft Node result

Craft self-check is readiness only. It does not decide independent functional pass or write acceptance evidence.

## Inputs

The craft Node packet must provide:

- packet `context_id`
- `source_truth_refs` with source use-case claim ids, review scenario ids, route implementation detail ids, cross-cutting contract ids, and chain refs
- `self_check_gates[]`
- `served_review_scenario_ids[]`
- `test_layers[]`
- `changed_files_under_check[]`
- `debug_test_cleanup_policy[]`
- active repair deltas
- read boundary and role contract

## Required Work

1. Validate the packet.
2. For every id/ref in `source_truth_refs`, source-artifact `must_read[]` entries, `runtime_directive_refs[]`, `active_repair_deltas[]`, `served_review_scenario_ids[]`, and `self_check_gates[]` (`gate_id`, `review_target_ids[]`, `source_*_ids[]`), find the matching source row or section in the approved source artifact.
3. Read each matched row/section carefully end to end before running checks. Read enough surrounding text to understand what the check must prove and what substitutes it must reject. Do not skim for the id and move on.
4. If a required id/ref cannot be found, cannot be read inside the read boundary, or conflicts with packet prose, return `partial` with `packet_insufficient` or `blocked` with the concrete conflict. Do not guess from old attempts or summaries.
5. Run every mandatory external check gate or report a lawful blocker.
6. Run only the applicable test layers named by the packet and affected scope.
7. Audit debug aids and test assets: no debug aid may live in a production path, and no raw debug aid may survive into review.
8. Detect downgrade attempts: source-only, probe-only, generated JSON, scratch-only success path, env-proof, headless bool, test-scene-only, or wrong-surface substitutes for runtime-visible scenarios.
9. Do not repair code during self-check. Return structured failures for scheduler continuation in the same craft subagent context.
10. Return self-check results in the craft Node result; do not write a separate persistent craft-check attempt file.

## Failure Output Rules

Each failed gate or cleanup issue must include:

- stable failure id
- gate id or cleanup policy id
- failure signature
- affected review target ids or review scenario ids
- rejected substitutes when relevant
- affected files/surfaces when known
- recommended rerun scope

## Forbidden Actions

- Do not edit production code.
- Do not author or weaken mandatory gates.
- Do not convert a failed runtime-visible gate into source inspection.
- Do not write review, final review, acceptance, or settlement conclusions.
- Do not dispatch nested agents or scheduler-like helpers.

## Output Result

Return craft Node self-check fields with:

- `status = ready_for_review`, `continue_repair`, `partial`, or `blocked`
- gate results keyed to applicable review target ids or review scenario ids
- test layer results
- debug/test cleanup results
- structured failures
- rerun scope recommendation
- bounded `read_audit[]`, with entries for each packet id/ref source row or section read and any missing/conflicting source
- bounded `search_audit[]`

`ready_for_review` is legal only when required gates and required cleanup checks pass or are lawfully not applicable by packet truth.
