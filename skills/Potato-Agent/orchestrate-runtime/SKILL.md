---
name: orchestrate-runtime
description: Schedule one module runtime through craft/review Nodes, Human Final Review, Technical Final Review, repair, settlement, and integration.
compatibility: opencode
---

# Orchestrate Runtime (Potato Agent)

Use this Skill only in the root runner context. It schedules Node-agent invocations and final review skills; it does not implement, self-check, inspect, accept, ship, or call MCP directly.

## Internal Skills

- `prepare-packet`
- `settle-phase`
- `integrate-phase`
- `prepare-human-final-review`
- `record-human-final-review`
- `run-technical-final-review`

## Node-Agent Dispatch

- `craft` -> `potato-agent-craft`
- `review` -> `potato-agent-review`

The scheduler passes the prepared Node packet plus minimal invocation context. It may not compile a replacement prompt that changes the packet contract.

## Runtime Model

- `craft`: scheduler-owned continuation over one `potato-agent-craft` context for execution, self-check, repair, and re-check until ready, blocked, or replanning is required
- `review-functional`: one `potato-agent-review` invocation context that runs the functional behavior/spec stage for one review Node
- `review-inspector`: one `potato-agent-review` invocation context that runs only after functional pass and inspects the same review Node's phase for technical/detail/garbage-code issues

Craft continuation is scheduler-controlled. Review-origin failures end the drained review context, backtrack to craft, then later rerun review in a new review invocation when required by runtime truth. Inspector is never scheduled when functional review fails or blocks.

## Direct Writes

The scheduler may write only:

- `sessions/<session_id>/session.json`
- `sessions/<session_id>/modules/<module_id>.json`

All Node packets, Node results, phase settlements, phase integrations, Human Final Review records, and Technical Final Review records are written by their owning skills or Node agents.

## Hard Rules

- Schedule from current session/module state and active chain truth.
- Validate Node packets before dispatch.
- Dispatch only `potato-agent-craft` for `kind = craft`.
- Dispatch only `potato-agent-review` for `kind = review`.
- Do not dispatch command wrappers for craft or review work.
- Node-agent invocations are terminal leaves.
- MCP is Node-agent-owned inside an authorized Node packet.
- Do not edit project files.
- Do not run compile, headless, tests, or MCP directly.
- Do not write authoritative evidence.
- Do not mark `ready_for_acceptance` until runtime review scenarios, Human Final Review, Technical Final Review, and required integrations are satisfied.

## Dispatch Rules

1. Read session/module state and active chain truth.
2. For a ready craft Node, prepare one craft Node packet and dispatch `potato-agent-craft`.
3. If craft self-check fails or returns `continue_repair`, resume the same craft subagent context with updated structured failures.
4. If craft is ready, record craft readiness as supporting input, then schedule the required successor review phase. Craft readiness alone never settles a phase or permits integration.
5. For a ready review Node, prepare one functional review packet with review scenario slices and dispatch `potato-agent-review` for the functional stage.
6. If functional review fails or blocks, call `settle-phase` for that review Node and its `review_plan.reviewed_craft_node_ids[]`; do not run inspector. Backtrack repair deltas to craft or stop on blocker.
7. If functional review passes, prepare one inspector packet for the same review Node phase and dispatch `potato-agent-review` for the inspector stage.
8. Call `settle-phase`. A passing phase requires both functional pass refs and inspector pass refs. Inspector failure creates repair deltas or a blocker.
9. If phase settlement creates repair deltas from review functional or inspector, end the drained review context and backtrack each delta to craft.
10. After craft repair readiness, rerun stale review targets, review scenarios, or inspection focus in a new `potato-agent-review` invocation.
11. If phase settlement passes, call `integrate-phase` and commit the module worktree for that phase boundary before scheduling any successor phase.
12. If phase integration blocks, stop; do not proceed with uncommitted reviewed changes.
13. When runtime review scenarios are closed and phase integration boundaries are satisfied, prepare Human Final Review and surface the prompt to the user.
14. Record the user's explicit Human Final Review result.
15. If Human Final Review fails, enters `continue_hfr_paired_repair`: create craft-only repair deltas. Do not dispatch a review Node for the user-observed issue unless the user explicitly chooses visible replanning.
16. If Human Final Review passes, run Technical Final Review.
17. If Technical Final Review fails, route blocking findings to craft repair and tell the user the repair may stale Human Final Review; the user decides whether to repeat Human Final Review after repair.
18. Only after Human Final Review and Technical Final Review pass may the module stop as `ready_for_acceptance`.

## Consistency Gate

Before returning, reread session/module state and confirm:

- no in-flight invocation is hidden
- current packets target `potato-agent-craft` or `potato-agent-review` by Node kind
- inspector review was scheduled only after functional pass
- no successor phase was scheduled before passed phase integration
- unresolved repair deltas are dispatched, parked, or recorded as replanning/manual blockers
- Human Final Review passed before Technical Final Review
- Technical Final Review passed before `ready_for_acceptance`
- no review pass summary closes a player-facing scenario with check names, counters, booleans, generated artifacts, file paths, or implementation seams
