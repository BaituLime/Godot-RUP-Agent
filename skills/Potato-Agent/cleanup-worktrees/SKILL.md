---
name: cleanup-worktrees
description: Remove shipped module worktrees and record the cleanup state after explicit shipping.
compatibility: opencode
---

Use only these references:

- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/TERMS.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/SESSION-CONTRACT.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/HANDOFF-CONTRACT.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/HANDOFF-SKILL-SPEC.md`

Goal:

- remove the module worktree after successful shipping of one Module
- record the concrete cleanup state without overstating shipping or acceptance authority

Read only:

- `sessions/<session_id>/session.json`
- `sessions/<session_id>/modules/<module_id>.json`
- `sessions/<session_id>/worktree-table.json`

Write only:

- repo-local worktree topology
- `sessions/<session_id>/worktree-table.json`
- `sessions/<session_id>/session.json`
- `sessions/<session_id>/modules/<module_id>.json`

Hard rules:

- operate on one already-shipped Module at a time
- require `sessions/<session_id>/modules/<module_id>.json.status = done` before deleting the module worktree
- remove `.worktrees/<module_id>` after successful ship when it exists
- remove lingering `.worktrees/<module_id>/acceptance` only if it still exists; that checkout is disposable and must not survive just because shipping happened later
- do not touch unrelated module worktrees
- do not edit project files beyond direct git worktree cleanup and runtime-state writes
- do not claim shipping success here; shipping authority remains with `integrate-main`
- if a listed worktree path is already absent, record that truth cleanly instead of failing just to preserve a stale directory
- mark `sessions/<session_id>/worktree-table.json.module_worktree.status = cleaned` after its checkout is removed
- append concise cleanup notes to `sessions/<session_id>/modules/<module_id>.json` and `sessions/<session_id>/session.json`

Consistency gate:

- reread `sessions/<session_id>/worktree-table.json` and confirm it matches `~/.config/opencode/skills/Potato-Agent/potato-agent-common/schemas/worktree-table.schema.json`
- reread `sessions/<session_id>/modules/<module_id>.json` and confirm it matches `~/.config/opencode/skills/Potato-Agent/potato-agent-common/schemas/module-session.schema.json`
- confirm the removed worktree paths are actually gone from the repo-local worktree topology before reporting success

Output:

- report which worktree paths were removed for the shipped Module
- report any already-absent cleanup paths as historical cleanup, not as fresh failure
