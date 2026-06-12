# Runtime Artifact Contract

## Current Live Forms

- Chain handoff: `potato_agent_chain_handoff/v18`
- Review scenario approved per-gate file: `potato_agent_review_scenario_gate/v1`
- Node packet/result records from the active Node-agent runtime
- Module session: `potato_agent_module_session/v18`
- Human Final Review record: `potato_agent_human_final_review_record/v2`
- Technical Final Review record: `potato_agent_technical_final_review_record/v2`
- Active Node kinds: `craft`, `review`

## Node Authority

The current runtime contract is the active Node packet plus its shared context id. It carries:

- Node kind: `craft` or `review`
- derived runtime agent for that Node kind
- packet `context_id` for the active craft or review context
- source truth refs
- read/search boundaries
- source use-case ids, review scenario ids, review scenario slices, and review targets
- active repair deltas
- exactly one Node payload

## Node-Agent Authority

- Craft Nodes dispatch to `potato-agent-craft`, which implements, self-checks, and repairs in one resumable craft context.
- Review Nodes dispatch to `potato-agent-review` in stages. Functional review runs first. Inspector review runs only after functional pass and drains after returning its result.
- The scheduler derives the agent from `node.kind`; model tiers and command wrappers are not live dispatch authority.

Craft self-check output is readiness/support data inside the craft Node result. It does not prove user-visible behavior and does not require a persistent separate attempt file. Review functional output and Human Final Review can support functional evidence. Review inspection and Technical Final Review can support quality gates but not functional pass.

Phase settlement is anchored by one review Node and covers that review Node plus `review_plan.reviewed_craft_node_ids[]`. A phase may pass only when functional review passed and inspector review passed. If functional review fails or blocks, inspector is not run and the phase settles as rework or blocked. Passed phase settlement must be integrated as a module worktree commit before any successor phase is scheduled.

Craft self-check repair resumes the same craft context. Review-origin repair does not resume the drained review context; it backtracks to the target craft Node and runs through `potato-agent-craft`, resuming the target craft context when lawful or starting a fresh craft context for that Node when not. Review reruns happen in new `potato-agent-review` invocations.

## Debug Aid Boundary

- Debug aids are scratch tools for repair or failure triage.
- Debug aids must live in independent, test, or tooling files; a production-path debug aid is a failure.
- Debug aids may exist only during the craft loop. `kept_for_check` is temporary and must be resolved by the next craft execution step.
- Before review, every debug aid must be `deleted` or `promoted_to_test_asset`; otherwise the Node blocks.
- Test assets help checks and regressions, but they do not prove acceptance by themselves.
- Acceptance evidence comes from review-owned attack, Human Final Review, Technical Final Review, settlement audit, or final acceptance, never from debug aids or craft self-check output.

## Historical Artifacts

Older runtime artifacts, packet kinds, attempts, summaries, and settlements are provenance only. They may contribute facts and failure signatures after translation into current repair deltas, but they cannot provide current form, field names, pass logic, dispatch authority, or template authority.
