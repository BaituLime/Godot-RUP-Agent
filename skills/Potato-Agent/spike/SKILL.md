---
name: spike
description: Validate one planner-selected uncertainty with appropriately bounded evidence for one Potato Agent module
compatibility: opencode
---

# Spike (Potato Agent)

Use this Skill to validate exactly one unresolved planning uncertainty or route/proof/process risk without pretending the result itself is approved planning truth or final acceptance evidence.

Canonical references:

- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/TERMS.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/HANDOFF-CONTRACT.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/HANDOFF-SKILL-SPEC.md`

## Goal

Produce focused evidence for one planning uncertainty that blocks use-case, architecture, review-scenario, or chain fields from being written without invented facts, and write that evidence into one `Spike Record`.

This Skill exists to reduce uncertainty inside planning. It is planner-owned validation with an explicit question boundary, not a fixed planning stage, not a user gate, and not a production execution substitute.

## Read boundary

Read from:

- any current planning artifacts that are already authoritative for the blocked question
- the explicit spike target or question
- registered read-only knowledge sources when they can quickly locate candidate files, docs, APIs, or evidence relevant to the one spike question

## Write boundary

You may write only:

- `spikes/<module_id>/<spike_id>.json`

Do not write:

- `Decision Handoff`
- `Chain Handoff`
- runtime packet artifacts under `sessions/<session_id>/packets/`
- runtime session artifacts under `sessions/<session_id>/`

## Spike rules

- Validate one question at a time.
- Keep the question boundary narrow and explicit, but size the evidence work to the uncertainty. Most spikes should stay small; a larger proof or complete throwaway implementation is allowed when it is the shortest way to resolve the exact blocked field.
- Prefer the smallest evidence that actually resolves the uncertainty; do not stop at a tiny probe if it leaves the decisive planning question unanswered.
- Record what was tried, what was observed, and which planning claim it does or does not support.
- Use `spike` whenever current planning would otherwise rely on guessed repo facts, guessed route viability, guessed proof feasibility, process/resource assumptions, or guessed chain feasibility.
- A spike may be invoked before, between, or inside planning skills; after it finishes, the blocked planning skill still owns any approved truth change and any required user gate.
- Treat spike output as planner-owned validation evidence only; it does not by itself approve route, review scenarios, chain, or final module acceptance.
- If the spike reveals a need to change approved decisions or planning artifacts, return that as evidence for the owning planning skill and gate; do not rewrite those artifacts here.
- If the spike touches code or scenes, keep the work clearly identified as spike material unless the user later promotes it. Spike code may be exploratory, partial, or complete, but it is not runtime-integrated production work until an owning planning/runtime path adopts it explicitly.
- If this spike is running in the top-level planner context, prefer dispatching `potato-agent-explore` for read-only file, symbol, or artifact lookup when that would avoid broad context loading before writing the spike record.
- If this spike is already running inside any child context, do not dispatch `potato-agent-explore` or any other nested child; OpenCode does not provide a second built-in subtask-dispatch layer there.
- Use registered read-only knowledge sources only as recall aids for the spike target. Confirm useful hits by direct reads or observed checks before recording them as spike observations.
- Do not rebuild, reindex, install, or mutate RAG or other knowledge sources as part of an ordinary spike.

## Good spike targets

- repo or platform facts that block a named use-case, architecture, review-scenario, or chain field
- viability of a technical route
- runtime/editor behavior that blocks route choice or landing truth
- interface or data-shape feasibility
- proof or test-probe feasibility for an acceptance claim
- process or runtime-resource questions that affect planning
- Node or chain feasibility when scope would otherwise be guessed
- performance or stability unknowns that affect planning

## Do not

- expand into unrelated feature implementation beyond the explicit spike question
- turn a spike into hidden production work or silently ship/adopt spike code
- treat `spike` as a fixed stage or approval shortcut
- silently settle architecture, review-scenario, or chain truth without the owning user gate
- rewrite planning artifacts in place from inside the spike
- rewrite the approved chain

## Output

1. Write concise spike proof into `spikes/<module_id>/<spike_id>.json`.
2. Return a short result with:
   - spike target
   - what planning assumption or blocked question it tested
   - what was tested
   - what was observed
   - which planning stage should resume next
   - whether it supports the current planning truth, requires upstream rework, or requires visible regating/replanning
