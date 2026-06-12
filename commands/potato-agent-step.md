---
description: Run exactly one rebuilt Potato Agent internal slice directly for manual operator control. Use when the user explicitly wants one named runtime or acceptance slice instead of the full top-level surface.
---
Reply in the user's language.

Treat `potato-agent-step` as a thin manual entrypoint for exactly one rebuilt slice.

Requested slice: `$ARGUMENTS`

Use the skill tool to load only the one explicitly requested slice from this list:

- `allocate-worktrees`
- `orchestrate-runtime`
- `assemble-acceptance-dossier`
- `accept-module`
- `run-technical-final-review`

Do only this:

1. resolve exactly one requested slice from `$ARGUMENTS` or the user's explicit request
2. if the request is missing or ambiguous, stop and ask for exactly one slice name from the allowed list
3. load only that one slice skill
4. execute only that slice and preserve its own contract, authority boundaries, and stop policy
5. stop as soon as that slice finishes or reaches its own gate
