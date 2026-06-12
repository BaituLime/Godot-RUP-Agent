---
title: Potato Agent runner rules
status: active-rebuild
language: general
---

# Potato Agent Runner Rules

This document defines the root unattended runner shell only.

## 1. Covered surfaces

Use this for:

- `potato-agent-runner`
- `allocate-worktrees`

## 2. Root context rule

- unattended execution must start from the root conversation context that can open first-level producer children
- do not host `orchestrate-runtime` inside a child agent

## 2.1 Artifact authority rule

- runners and scheduler-facing surfaces must follow `ARTIFACT-AUTHORITY-CONTRACT.md`
- artifact form comes only from the current schema, current owning skill contract, or current skeleton; old handoff/session artifacts may provide facts and provenance only
- before creating or refreshing runtime artifacts, read current authority first, then read predecessor artifacts only for factual lineage context
- copying an old packet, attempt, settlement, session file, or scheduler return as a template is invalid even if the result later validates
- older runtime artifacts, packet kinds, attempts, and settlements are provenance only, not live dispatch authority

## 3. Session Resolution

- a runner invocation is an entry surface, not a session boundary
- resolve one active session per requested module; never create one shared multi-module session
- if a lawful session already exists for the requested module and current chain revision, resume that same session
- create a fresh session only when no lawful session exists for that module iteration, or when visible replanning from approved use-case, architecture, review-scenario, or chain changes supersedes planning truth
- if visible replanning supersedes planning truth, create a fresh successor session for that module instead of mutating the old session in place
- if multiple candidate sessions appear lawful for the same module and chain revision, stop for explicit `session_id` selection instead of guessing from timestamps

## 4. Lineage rule

- a lineage-root session writes `lineage_session_id = session_id` and `previous_session_id = null`
- resuming a stopped session preserves the same `session_id`, `lineage_session_id`, and `previous_session_id`
- a lawful successor session preserves the predecessor lineage id and records `previous_session_id`
- fresh successor sessions inherit only truthful historical context, not live state from the predecessor session

## 5. Core runner sequence

The runner may do only this:

1. resolve handoff root and requested module set
2. resolve or create one session per requested module
3. register the active session when a concrete `session_id` exists
4. initialize or refresh `session.json` directly from current session truth; there is no DAG-first or head-weave composition phase
5. run `allocate-worktrees`
6. run `orchestrate-runtime`
7. reread runtime artifacts after every returned scheduler slice
8. write `scheduler-return.json`
9. follow only the receipt-authorized next action

## 6. Reverse gate

- only `scheduler-return.json.next_action` may decide the runner's post-scheduler action
- legal actions are:
  - `surface_stop_to_user`
  - `fail_closed_protocol_blocker`
- scheduler prose never authorizes surfacing by itself

## 7. Stop surfacing

- the runner may surface to the user only when reread runtime truth still shows a real stop
- a returned `current_phase = orchestrating` state is protocol-invalid, not an internal stop or refresh point
- for `parked_modules_pending`, the runner surfaces the single parked module and its `park_reason`
- when the parked module reason is `user_clarification_needed`, the runner surfaces `modules/<module_id>.json.pending_user_clarification` and records the answer as a detail override when it remains `detail_only`
- when the parked module reason is `replan_needed`, the runner surfaces visible replanning; it may not invoke delegated delta planning to rewrite approved planning truth

## 8. Cleanup timing

- smooth completion stops such as `ready_for_acceptance` are lawful only after required staged reviews, Human Final Review, and Technical Final Review have passed; they do not trigger cleanup by themselves because active runtime cleanup belongs to the explicit shipping/cleanup flow
- Human Final Review is a separate runner/orchestrator-owned gate after required runtime craft and staged review work. The runner may prepare a concise prompt and record the user's explicit result, but it may not substitute its own judgment for the user's feedback.
- A failed Human Final Review enters paired repair: preserve the user quote/failure signature, map it to review scenarios and responsible craft Node(s) when possible, compile craft repair delta gates, repair through craft only, and repeat Human Final Review. Do not rerun review Nodes during this paired repair mode unless the user explicitly chooses visible replanning back to review-scenario or chain planning.
- Technical Final Review runs only after Human Final Review passes. If it triggers craft repair, expose the repair impact to the user and let the user decide whether Human Final Review must be repeated.
- blocked or stalled stops such as `parked_modules_pending` or `global_blocked` preserve module worktrees and the same session for inspection or later continuation unless a later explicit cleanup/shipping flow or superseding chain revision changes the module iteration

## 9. Hard bans

- do not branch on scheduler prose instead of reread artifacts
- do not stop after stop-type handling unless session/module state and scheduler return record `waiting_for_human_final_review`, `ready_for_acceptance`, `parked_modules_pending`, `global_blocked`, or `all_done`
- do not create a new session merely because the runner was invoked again or resumed after a lawful stop
- do not reuse a superseded or shipped session as the active live session for a new module chain iteration
- do not create a multi-module execution session
- do not use historical handoff artifacts as form examples for current artifacts
