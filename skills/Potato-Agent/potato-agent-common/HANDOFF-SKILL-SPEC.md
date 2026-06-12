# Handoff Skill Spec

## Runtime Agent Set

Runtime Node work is performed by Node agents:

- `potato-agent-craft`
- `potato-agent-review`

The scheduler dispatches by active Node kind. It does not build template-generated producer prompts, dispatch command wrappers for craft/review work, or dispatch any role outside `craft` and `review`.

## Subtask Dispatch

- `craft` entrypoint: dispatch `potato-agent-craft` with one craft Node packet and resume the same subagent context for repair/self-check continuation.
- `review` entrypoint: dispatch `potato-agent-review` first with a functional review packet. Dispatch an inspector packet only after functional pass. Review-origin repair never resumes the drained review context.

## Packet Preparation

Runtime packet preparation owns converting chain source wiring into one Node packet. It must fail closed when craft would lack a self-check gate tied to source claim ids and review scenario ids, or when review would lack original review scenario slices, required review path, or inspection target.

## Node Agent Boundary Rules

Runtime Node agents are bounded first-level subagents. They may not dispatch nested agents, become schedulers, broaden Node scope, or rewrite planning truth.

Craft self-check repair resumes the same craft subagent context. Review-origin repair routes repair deltas to the target craft Node and uses `potato-agent-craft`, resuming the target craft context when lawful or starting a fresh craft subagent for that Node when not. Review reruns use a new `potato-agent-review` invocation.

## MCP

MCP remains Node-owned and handoff-authorized. Scheduler never proxies MCP.
