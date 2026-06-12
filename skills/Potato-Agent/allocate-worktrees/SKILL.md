---
name: allocate-worktrees
description: Create or refresh the module worktree for one single-module execution session.
compatibility: opencode
---

# Allocate Worktrees (Potato Agent)

Use this Skill to create or refresh the module worktree for one single-module execution session.

The active runtime has exactly one worktree for `session.json.module_id`. It does not allocate session-scoped per-agent worktrees and it does not maintain a separate integration worktree.

Canonical references:

- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/RUNNER-RULES.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/RUNTIME-ARTIFACT-CONTRACT.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/PLANNING-ARTIFACT-CONTRACT.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/CHAIN-RUNTIME-CONTRACT.md`

## Goal

- materialize the module worktree for `session.json.module_id`
- write `sessions/<session_id>/worktree-table.json`
- seed or refresh `sessions/<session_id>/modules/<module_id>.json` without resetting live progress for an existing session

## Read boundary

Read from:

- direct git/project evidence needed for repo root, base branch, worktree policy, and global resource constraints
- approved `decision/<module_id>.json`
- approved `chain/<module_id>.json`
- `sessions/<session_id>/session.json`
- when `session.previous_session_id` is non-null, the truthful predecessor `sessions/<previous_session_id>/modules/<module_id>.json` for historical context only

## Write boundary

You may write only:

- repo-local module worktree topology
- `sessions/<session_id>/worktree-table.json`
- `sessions/<session_id>/modules/<module_id>.json`
- `sessions/<session_id>/session.json`

## Hard rules

- Create one module worktree for `session.json.module_id` at `.worktrees/<module_id>` unless approved planning truth or direct project evidence names a project-specific module-worktree root.
- Do not create any session-scoped per-agent checkout under `.worktrees/`.
- Do not create `.worktrees/<module_id>/integration` or any other nested runtime checkout under a module worktree.
- If an older session-era per-agent or integration worktree exists, do not reuse it as active runtime capacity; the active runtime uses only the module worktree.
- Each module worktree uses that module's runtime branch. If the branch does not exist yet, create it from the approved project base branch discovered from active planning or direct git evidence.
- If active planning or direct git evidence does not expose the base branch name, fail closed instead of hardcoding a branch name.
- Before dispatching a Node, the module worktree must be refreshable from its own module branch.
- Create or refresh worktrees with direct git commands only.
- If you touch `session.json`, preserve its truthful `current_phase`, `continue_reason`, `resume_reason`, and `stop_reason`.
- If `modules/<module_id>.json` already exists for this session and its `chain_revision_id` still matches the approved active chain, preserve live Node progress, packets, current craft/review results, settlements, overrides, parked state, final review state, and prior failure notes. Refresh only worktree topology fields that truthfully changed.
- Initialize `prior_failure_notes = []` only for a newly created module-session file unless a predecessor session contains truthful narrow prior failure notes that are still valid for this fresh successor session.
- Pin `chain_revision_id` and initialize `current_node_id` from `chain_contract.current_head_node_id` only when seeding a new module-session file or a lawful fresh successor session.
- Initialize `node_runtime_state = ready`, `partial_reason = null`, `pending_user_clarification = null`, `active_node_overrides = []`, and `stale_successors = []` only when seeding a new module-session file or a lawful fresh successor session.
- Use `session_format = potato_agent_session/v3`, `worktree_table_format = potato_agent_worktree_table/v2`, and `module_session_format = potato_agent_module_session/v18`.
- Do not allocate acceptance checkouts.
- Do not dispatch subagents.
- Do not edit project files beyond git worktree management and runtime-state writes.

## Consistency gate

- Reread `sessions/<session_id>/worktree-table.json` and confirm it matches `~/.config/opencode/skills/Potato-Agent/potato-agent-common/schemas/worktree-table.schema.json`.
- Reread each written `sessions/<session_id>/modules/<module_id>.json` and confirm it matches `~/.config/opencode/skills/Potato-Agent/potato-agent-common/schemas/module-session.schema.json`.
- Confirm the written worktree table contains exactly one `module_worktree` object whose `module_id` matches `session.json.module_id`.
- Confirm the seeded module session points at the real module worktree and module branch.
