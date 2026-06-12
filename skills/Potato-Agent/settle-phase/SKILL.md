---
name: settle-phase
description: Settle one review-anchored phase after functional and inspector review stages and route repair or integration.
compatibility: opencode
---

# Settle Phase (Potato Agent)

Use this Skill to settle one review-anchored phase. The phase is anchored by one review Node and covers that review Node plus `review_plan.reviewed_craft_node_ids[]`.

## Read Boundary

Read from:

- the review Node's current packet and `review_plan`
- the reviewed craft Node packets and results named by `review_plan.reviewed_craft_node_ids[]`
- functional review stage results when run
- inspector review stage results when run
- active chain truth
- active review scenario truth
- active module/session state
- current schemas

## Write Boundary

Write only:

- authoritative evidence summaries for review-functional or settlement-audit scopes when allowed
- `sessions/<session_id>/settlements/<module_id>/<settlement_id>.json`
- `sessions/<session_id>/modules/<module_id>.json`
- `sessions/<session_id>/session.json`

Do not write evidence summaries for craft self-check readiness or inspector-only findings.

## Phase Boundary

- `review_node_id` anchors the phase.
- `reviewed_craft_node_ids[]` comes from `review_plan.reviewed_craft_node_ids[]`.
- `settled_node_ids[]` must include the review Node and its reviewed craft group.
- Craft readiness is supporting input only. A craft self-check never closes a phase, review scenario, or use-case claim.
- Human Final Review and Technical Final Review are not phase settlements.

## Stage Gates

- Functional review is the vertical behavior/spec gate.
- Inspector review is the horizontal technical/detail/garbage-code gate.
- If functional review fails or blocks, settle the phase as `rework_needed` or `blocked`; inspector must not run.
- If functional review passes, inspector must run before the phase can pass.
- A phase cannot pass unless functional passed and inspector passed.
- Inspector failure after functional pass settles the phase as `rework_needed` or `blocked` with repair deltas.

## Functional Settlement

Functional settlement may advance only when:

- every required review target has a functional result
- every required review scenario referenced by the review target has a result or lawful blocker
- pass results use the required or stronger review path
- player-facing pass results identify the normal-player game path attacked in ordinary language
- failures emit repair deltas tied to `review_target_id`, `review_scenario_id`, source use-case ids, or functional result id

Reject pass when a player-visible scenario is closed by craft self-check, API/probe/log/counter/source-only evidence, generated artifacts, MCP/editor-only paths, Verify/Headless checks, test scenes, production probes, weaker substitutes, or forbidden substitutes.

## Inspector Settlement

Inspector settlement may advance only after functional pass and only when:

- `architecture_coverage[]` covers every inspector packet `architecture_inspection_items[]` entry
- scenario path hypotheses were inspected or lawfully limited
- passing inspector has no `drifted` or `blocked` architecture coverage rows
- passing inspector has no blocking findings and no repair deltas
- inspector findings that need code changes settle the phase as `rework_needed`, and each repair delta targets the responsible craft Node

Inspector review does not prove functional behavior. It may block code that functionally passes when the pass path depends on hard-coded output, skipped UI path, generated report, backend counter, editor-only route, seeded state, debug-only dependency, conflicting ownership for the same fact or resource, unowned cleanup, or approved-route bypass.

## Gate Rules

- `pass`: phase is ready for immediate phase integration before any successor phase is scheduled.
- `rework_needed`: write `repair_deltas[]` and stale-successor state, then let scheduler backtrack to craft.
- `blocked`: stop only for true environment/manual/packet blockers.
- `runtime_review_complete`: legal only after required phases are settled, integrated, and relevant review scenarios are closed; it does not mean final review or acceptance is done.

## Consistency Gate

After writing settlement:

- validate against `node-settlement.schema.json`
- confirm `reviewed_craft_node_ids[]` matches the review plan
- confirm `settled_node_ids[]` includes the review Node and reviewed craft group
- confirm craft self-check readiness did not close the phase
- confirm inspector was not run when functional failed or blocked
- confirm a passing phase includes functional pass and inspector pass refs
- confirm a passing phase has no blocking findings, no repair deltas, and no failed architecture coverage rows
- confirm review functional evidence is review-owned, not craft-owned
- confirm stale successors name pending review target ids, review scenario ids, or inspection focus
