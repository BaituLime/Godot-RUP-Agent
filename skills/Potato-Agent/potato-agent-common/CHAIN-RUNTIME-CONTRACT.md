# Chain Runtime Contract

## Active Runtime Roles

- `craft`
- `review`

## Chain-To-Runtime Flow

Chain carries Node kind, source use-case ids, review scenario ids, review targets, craft self-check gates, review inspection plan, resources, and MCP posture. Runtime packet preparation compiles one packet for the ready Node.

Chain order should preserve local feedback loops. Chain planning first groups work by phase for operator explanation and planning clarity, then cuts each phase into MVP craft Nodes. Phase settlement is anchored by the review Node and covers `review_plan.reviewed_craft_node_ids[]`. Runtime should run the smallest craft group whose review Node can validate at least one named review scenario or unblock a named dependent chain Node, run functional review, run inspector only after functional pass, settle the phase, and integrate the passed phase before dependent phase work continues. Do not default to one review per craft Node, and do not batch all craft Nodes ahead of all review Nodes unless the chain names the indivisible dependency that prevents earlier review.

## MCP Pairing

Runtime-visible craft self-check gates and review scenario attacks for the same seam must both name MCP/resource needs, allowed actions, unavailable resources, fallback legality, and whether the same runtime-visible seam can be exercised without substitution. This does not create scheduler-owned MCP; the Node agents own bounded MCP work inside their craft or review context.

## Repair Flow

Craft self-check failures stay inside the same `potato-agent-craft` subagent context for that craft Node. The scheduler updates the packet with structured failures and resumes that craft context rather than cold-starting repair.

Review functional and inspection findings produce repair deltas. If functional fails or blocks, inspector is not run. The drained `potato-agent-review` context ends and is never resumed for implementation repair. Scheduler backtracks each delta to the target craft Node, where it becomes a craft self-check gate. Repair runs through `potato-agent-craft`: resume the target craft Node's existing craft context when lawful, otherwise start a fresh craft context for that target craft Node. After repair readiness, rerun stale review targets or inspection focus in a new `potato-agent-review` invocation.

## Dispatch

Scheduler derives the runtime agent from `node.kind`: `craft` dispatches to `potato-agent-craft`, and `review` dispatches to `potato-agent-review`. Chain truth must not carry tier names or command-wrapper names as live dispatch authority.
