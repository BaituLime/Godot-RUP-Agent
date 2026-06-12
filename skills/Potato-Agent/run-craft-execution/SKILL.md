---
name: run-craft-execution
description: Internal craft execution step used by potato-agent-craft inside one craft Node.
compatibility: opencode
---

# Run Craft Execution

Use this only inside `potato-agent-craft`.

## Purpose

- implement the bounded production slice for the assigned craft Node
- preserve craft context for self-check and repair continuation
- serve named review scenarios without claiming scenario closure
- add or update durable test assets only when required by route, policy, or repair
- declare debug aids for deletion or test-asset promotion

Craft builds. Craft self-check proves readiness only. Craft never owns functional pass, code-quality pass, Human Final Review, Technical Final Review, or acceptance.

## Inputs

The craft Node packet must provide:

- packet `context_id`
- `source_truth_refs` with source use-case claim ids, review scenario ids, route implementation detail ids, cross-cutting contract ids, and chain refs
- `runtime_directive_refs[]` when present
- route slice
- `served_review_scenario_ids[]`
- production seams
- allowed file scope
- required test asset policy
- debug aid policy
- prior check failures
- active repair deltas
- read/write boundary and role contract

## Required Work

1. Validate the packet before changing code.
2. For every id/ref in `source_truth_refs`, source-artifact `must_read[]` entries, `runtime_directive_refs[]`, `active_repair_deltas[]`, `served_review_scenario_ids[]`, and `self_check_gates[]` (`gate_id`, `review_target_ids[]`, `source_*_ids[]`), find the matching source row or section in the approved source artifact.
3. Read each matched row/section carefully end to end before implementation work. Read enough surrounding text to understand what it allows, forbids, and leaves out. Do not skim for the id and move on.
4. If a required id/ref cannot be found, cannot be read inside the read boundary, or conflicts with packet prose, return `partial` with `packet_insufficient` or `blocked` with the concrete conflict. Do not guess from old attempts or summaries.
5. Write a concise route readback from the rows you read: what this slice lands, which review scenarios it serves, which route details/contracts bind it, and which production seams it touches.
6. Implement the bounded production slice.
7. Add or update persistent tests only when required by route, test policy, or repair delta.
8. Use debug aids only as scratch tools for repair or failure triage, never in production paths.
9. Delete each debug aid as soon as it is no longer needed. If neither deletion nor valid test-asset promotion is possible, block.
10. Run mandatory self-check gates inside the craft Node before reporting ready.
11. Return one craft Node result through the live Node-agent contract.

## Forbidden Actions

- Do not substitute source-only proof, generated artifacts, hard-coded success paths, test-only scenes, backend-only counters, editor/MCP shortcuts, or route bypasses for the approved route.
- Do not treat debug aids, probe output, generated JSON, Verify output, env proof modes, or test scenes as player-facing pass evidence.
- Do not write review functional, review inspector, Human Final Review, Technical Final Review, acceptance, or settlement conclusions.
- Do not dispatch nested agents or scheduler-like helpers.
- Do not write hard-coded special cases, fixture-conditioned branches, review/proof-only switches, route-specific shims, scratch-only success paths, harness JSON status, or code that exists only to satisfy one scenario without implementing the approved path.

## Output Result

Return one craft Node result with:

- `status = ready_for_review`, `continue_repair`, `partial`, or `blocked`
- route readback
- served review scenario ids
- changed production files
- changed test files
- test assets changed
- debug aids
- prior check failures addressed
- self-check gate results keyed to applicable review target ids or review scenario ids
- bounded `read_audit[]`, with entries for each packet id/ref source row or section read and any missing/conflicting source
- bounded `search_audit[]`

`ready_for_review` means only that the craft Node passed required self-check gates and is ready for independent review.
