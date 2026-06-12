# Scheduler Rules

## Dispatch

- Dispatch only active Node kinds `craft` and `review`.
- Dispatch `craft` Nodes to `potato-agent-craft`.
- Dispatch `review` Nodes to `potato-agent-review`.
- Craft Nodes implement, self-check, repair, and re-check in one resumable craft context controlled by scheduler continuation.
- Review Nodes run functional review first. Inspector review runs only after functional pass, and the context drains after each review stage result returns.
- Do not dispatch any role outside the active `craft` and `review` Node model.
- Do not compile replacement producer prompts from templates.
- Do not dispatch command wrappers for craft or review work.
- Runtime children are terminal leaves.

## Packet Requirements

- Runtime packets must carry approved use-case, architecture, chain, source ids, and review target refs.
- Runtime handoffs must bound reads/searches and forbid broad roots `/`, `/home/bunny`, and `~`.
- Craft handoffs must carry craft self-check gates.
- Review packets must carry review targets, inspection targets, and debug aid policy.

## Repair

- Review functional failures backtrack by `review_target_id` and source ids.
- Review inspection failures backtrack by `inspection_finding_id`.
- Craft self-check failures resume the same craft subagent context with updated structured failures.
- Review-origin failures do not resume the review subagent for repair. They become repair deltas, route to the target craft Node, and run through `potato-agent-craft`.
- For review-origin repair, resume the target craft Node's existing craft context when lawful; otherwise start a fresh craft subagent for that target craft Node.
- After craft repair readiness, rerun stale review targets or inspection focus in a new `potato-agent-review` invocation.
- A passing phase settlement must be integrated as a module worktree commit before scheduling a successor phase.
- Backtracked craft repair must include the failed review target or inspection gate before craft may report ready.

## MCP

MCP is Node-agent-owned inside the dispatched craft or review context. The scheduler never calls MCP, never bootstraps runtime for evidence, and never collects proof.
