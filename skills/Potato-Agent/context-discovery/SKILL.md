---
name: context-discovery
description: Clarify a vague project or module request and gather minimal direct project evidence before use-case design
compatibility: opencode
---

# Context Discovery (Potato Agent)

Use this Skill at the start of a planner conversation when the user's request still lacks the operator goal, target surface, module boundary, success observation, out-of-scope boundary, or feasibility facts needed for `use-case-design`.

This is an intake and pre-use-case discovery skill. It produces a short `Intake Brief` in the current conversation only. It does not create, refresh, or depend on persistent context artifacts.

Canonical references:

- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/TERMS.md`

## Goal

Move from a fuzzy user goal to the minimum shared facts `use-case-design` needs to define operator-facing use cases without inventing target surface, success observation, or module boundary.

## Read boundary

Read from:

- the user's current goal and scope discussion
- direct project files, docs, git state, and tool outputs needed to avoid fantasy planning
- existing approved planning artifacts only when they are clearly relevant to the new request
- registered read-only knowledge sources when they can locate candidate files, docs, APIs, or constraints faster

Do not read broadly just to summarize the repository.

## Write boundary

Do not write persistent artifacts.

Specifically, do not write:

- context files or context directories
- use-case artifacts
- Decision Handoff
- Chain Handoff
- runtime session artifacts
- project source files

The only output is a concise `Intake Brief` in chat.

## Discovery posture

- Prefer direct evidence over assumptions.
- Keep exploration minimal and goal-shaped.
- Use enough repo awareness to discover platform constraints, existing surfaces, likely entry points, and obvious feasibility risks.
- Do not choose the technical route; leave that to `architecture-confirm` after use cases are approved.
- Do not define chain Nodes, packet scope, review plans, or acceptance gates.
- Do not turn guesses into facts. Mark uncertain items as uncertain.
- If narrow file hunting or symbol lookup would bloat the planner context, dispatch `potato-agent-explore` as a read-only helper and absorb only the relevant findings.
- Treat knowledge-source hits as recall aids. Confirm important hits by direct reads before using them as facts.
- If a missing operator goal, target surface, module boundary, success observation, out-of-scope boundary, or feasibility constraint cannot be discovered safely, ask the user instead of guessing.
- If the decisive uncertainty requires proof rather than ordinary discovery, recommend a focused `spike` and explain the blocked question.

## Clarification focus

Clarify only issues that materially affect use-case design, especially:

- what the operator should be able to do, see, or experience
- whether success means user-visible behavior, reusable project structure, or both
- the likely module boundary and out-of-scope edges
- relevant target surfaces, modes, scenes, screens, commands, or operator flows
- user tolerance for behavior changes, placeholders, manual steps, or temporary debug aids
- hard platform, tool, resource, or environment constraints that shape feasible use cases

Avoid architecture-first questions unless the user's answer changes what the module must achieve.

## Output

Return an `Intake Brief` with:

- `Goal`: concise restatement of the user's desired outcome
- `Candidate Module Boundary`: current best boundary and what is outside it
- `Relevant Anchors`: files, docs, scenes, systems, tools, or commands found by minimal discovery
- `Known Constraints`: factual constraints and their evidence source
- `Uncertainties`: unanswered questions, marked as user-question, repo-discovery, or spike-needed
- `Ready For Use-Case Design`: yes/no, with the reason
- `Suggested Next Step`: usually ask specific questions, run a focused spike, or proceed to `use-case-design`

Keep the brief short. It should help the current planner conversation proceed; it is not durable project memory.

## Consistency gate

- Confirm no persistent files were written.
- Confirm the brief separates observed facts from assumptions.
- Confirm any recommended next step is narrower than broad repo exploration.
