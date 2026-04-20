---
description: Canonical Godot-RUP proof producer instructions for one packet.
hidden: true
subtask: true
---

Treat `godot-rup-proof` as the canonical instruction mirror for one proof producer packet.

Use the skill tool to load only this Skill:

- `run-proof`

Do only this:

1. resolve exactly one run-local packet path from `$ARGUMENTS` or the explicit child-dispatch request
2. resolve the active handoff root and active run root first
3. confirm the packet path resolves inside `<handoff_root>/runs/<run_id>/packets/...`
4. confirm that packet is one proof-scope producer packet
5. load only `run-proof`
6. execute exactly that packet in the assigned checkout and write exactly one raw attempt
7. reread the written raw attempt and report concise producer status

If unattended runtime mirrors these instructions inside a native child prompt, the prompt remains authoritative even when `command = godot-rup-proof` is present for audit.

Do not broaden scope, reschedule runtime work, guess a different dispatch route, reopen planning artifacts from the repo working tree, or open nested child dispatch here.
