---
name: integrate-phase
description: Commit one passed phase settlement in the module worktree and record the resulting phase integration commit.
compatibility: opencode
---

# Integrate Phase (Potato Agent)

Use this Skill to checkpoint one passed phase settlement in its module worktree.

Each active single-module session owns one module worktree record. There are no session-scoped per-agent worktrees and no separate module integration worktree in the active model.

Canonical references:

- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/SCHEDULER-RULES.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/RUNTIME-ARTIFACT-CONTRACT.md`

## Goal

- integrate exactly one passed `settlement_id` anchored by `review_node_id`
- commit the module worktree at that phase boundary
- record the real resulting commit hash when integration succeeds

## Read Boundary

Read from:

- the relevant current phase settlement
- `sessions/<session_id>/worktree-table.json`
- `sessions/<session_id>/modules/<module_id>.json`
- `sessions/<session_id>/session.json`

## Write Boundary

You may write only:

- the module worktree git state
- `sessions/<session_id>/integrations/<module_id>/<settlement_id>.json`
- `sessions/<session_id>/worktree-table.json`
- `sessions/<session_id>/modules/<module_id>.json`
- `sessions/<session_id>/session.json`

## Hard Rules

- Operate on one passed phase settlement only.
- Use the module's single worktree recorded in `worktree-table.json.module_worktree`, and require its `module_id` to match `session.json.module_id`.
- Verify the worktree contains only reviewed changes covered by the settlement's `settled_node_ids[]` and `source_scope_ids[]`. Compare git-changed paths to the reviewed craft attempt changed-file records and inspector coverage surfaces. If any changed path cannot be tied to the settled phase, block instead of committing.
- Record the real resulting commit hash.
- Do not claim integration by editing runtime state alone; integration requires the declared git commit and resulting commit hash or an explicit blocker.
- Do not integrate a phase whose settlement advanced review from debug aids or craft self-check output. The settlement must show review scenario closure came from review functional evidence and inspector findings as applicable, not proof hooks, Verify/env proof modes, generated files, craft self-checks, self-check readiness, file existence, or status/pass text.
- Do not integrate a phase with unresolved `repair_deltas[]` or stale affected-successor truth. Integration is allowed only after settlement/module state shows the phase's live repair deltas are closed, blocked according to approved boundary, or routed to explicit visible replanning/manual handling.
- Do not schedule a successor phase before this integration succeeds. If integration blocks, stop with the blocker recorded.
- Do not perform explicit ship/mainline integration here.
- Do not set a module status to `done` here.
- Do not invent `continue_reason`, `resume_reason`, or a final session stop here.

## Consistency Gate

- Reread `sessions/<session_id>/integrations/<module_id>/<settlement_id>.json` and confirm it matches `~/.config/opencode/skills/Potato-Agent/potato-agent-common/schemas/integration-record.schema.json`.
- Confirm `modules/<module_id>.json` records the resulting `last_integration_commit` and does not overclaim post-acceptance ship state.
- Confirm `worktree-table.json` still records the module worktree truthfully.
- Confirm the source settlement did not depend on debug aids, craft-originated proof output, or craft self-check readiness as review closure authority.
- Confirm the source settlement/module state has no unresolved live repair deltas or stale affected-successor checks for the phase being integrated.
