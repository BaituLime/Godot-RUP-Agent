# Execution Role Map

## Active Runtime Nodes

| Node Kind | Runtime Agent | Owns | Cannot Own |
| --- | --- | --- | --- |
| `craft` | `potato-agent-craft` | implementation, repair, and craft self-check readiness gates inside a resumable craft context | final evidence, review pass, Human Final Review, Technical Final Review, acceptance |
| `review` | `potato-agent-review` | independent functional/spec review and code-quality inspection inside one drained review invocation | implementation edits, implementation repair, reuse of drained review context for repair |

## Dispatch Rule

The scheduler dispatches by active Node kind. It does not dispatch any packet role outside `craft` and `review`, does not build template-generated producer prompts, and does not use model-tier names as dispatch authority.

## Evidence Rule

Craft checks are required readiness records. They do not prove user-visible behavior. Review-owned attacks, Human Final Review, and Technical Final Review are the sources that may support acceptance evidence.

## Repair Context Rule

Craft self-check failures resume the same craft context. Review-origin failures end the review context, become repair deltas, route to the target craft Node, and rerun review later in a new review invocation.
