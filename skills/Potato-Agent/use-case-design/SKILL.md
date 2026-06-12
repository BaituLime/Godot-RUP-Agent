---
name: use-case-design
description: Define and record the approved operator-facing use-case contract for one Potato Agent module
compatibility: opencode
---

# Use-Case Design (Potato Agent)

Use this Skill to define what operator-visible scenarios, success conditions, and source use-case claims a module must satisfy before technical route selection begins.

Canonical references:

- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/TERMS.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/HANDOFF-CONTRACT.md`
- `~/.config/opencode/skills/Potato-Agent/potato-agent-common/HANDOFF-SKILL-SPEC.md`

## Goal

Produce a recommendation first, then write the approved use-case design only after the user passes the required `DG-USE-CASE-*` gate.

This phase defines what the module must let the operator do, see, or experience. It is repo-aware, but it does not choose the technical route.

## Read boundary

Read from:

- user goal and scope discussion
- current-conversation `Intake Brief` from `context-discovery` when present
- relevant project docs, UX references, and product intent
- direct repo/project facts needed to avoid impossible or misleading claims: existing user/operator surfaces, current behavior, platform constraints, target modes/scenes/screens/commands, feasibility blockers, and project vocabulary
- registered read-only knowledge sources, such as project RAG or product/API doc indexes, when they can quickly locate relevant docs or repo constraints

## Write boundary

You may write only:

- `use-cases/<module_id>.md`
- `use-cases/<module_id>.json`
- `use-cases-history/<module_id>/<use_case_design_revision_id>.md`
- `use-cases-history/<module_id>/<use_case_design_revision_id>.json`

`use-cases/<module_id>.md` is the authoritative artifact.

`use-cases/<module_id>.json` is a thin companion index only.

Approved use-case truth remains first-class upstream authority for architecture, review-scenario design, chain, packet, and runtime behavior semantics. Later route or review prose may realize these semantics, but it does not replace or downgrade them.

Only write these after explicit user approval.

- If this use-case design supersedes an earlier approved revision, archive the prior markdown and json revision under `use-cases-history/<module_id>/<use_case_design_revision_id>.*` before replacing the latest alias.
- Do not silently discard an earlier approved use-case-design revision.

## Required drafting order

Present recommendations in this order:

1. `Use-Case Claims`
2. `Out Of Scope`
3. `Open Questions`

## Use-Case Claim Standard

`use_case_claims[]` is the only use-case source list. No second use-case source list exists.

Older project artifacts may contain a separate scenario-summary section from a prior format. Treat that section as historical input only. When superseding or drafting current use-case design, fold any useful scenario prose into `use_case_claims[].text` and do not reproduce the old section heading or json fields.

Each claim must stay operator-facing and implementation-neutral. It should make clear, in prose:

- operator intent
- success experience
- relevant edge or failure pressure
- what later acceptance must be able to prove as real behavior

Read direct repo/project facts when needed to avoid claims that the platform, existing UI, current behavior, target mode, or known constraints cannot support. Do not collapse into architecture or implementation route selection here.

Every `use_case_claims[]` row must include:

- `claim_id`
- `text`
- `acceptance_notes[]`

The claim `text` itself must stay in ordinary operator language. It must say what a non-technical operator, player, or maintainer should see, do, understand, or get. Do not let `acceptance_notes[]` become the real claim while `text` collapses into check names, class names, file paths, counters, booleans, or implementation seams.

Good shape: “When the player opens the normal map, the occupied area is visibly distinguishable from neutral terrain and still leaves settlements and rails readable.”

Bad shape: “`OccupationOverlayCheck` passes with `styled_cells > 0`.”

## Hard rules

- This phase owns what the module must achieve, not how the repo should implement it.
- Use-case prose must stay understandable without knowing the codebase. Technical terms may appear only when the user-facing concept itself needs them; pair them with the plain effect.
- Downstream stages must carry this approved use-case truth forward explicitly instead of replacing it with route, review, or implementation prose.
- Do not choose classes, scenes, seams, authority anchors, technical route options, or task structure here.
- Do not define review scenarios, proof packets, or chain Nodes.
- Do not let implementation convenience silently narrow the operator-visible contract.
- If a proposed use case can only be described by assuming one technical route over another, keep the user-facing contract and mark the route pressure as an open question for `architecture-confirm`.
- If the use-case claim set itself is still ambiguous, stop here and resolve it before technical route approval proceeds.
- If use-case scoping depends on a still-unknown repo, platform, or feasibility fact, stop and request a planner-owned `spike` instead of inventing capability truth here.
- Treat RAG/search hits as recall only. Confirm useful hits by directly reading the cited source before they affect the proposed use-case contract.
- Do not make a read-only knowledge source mandatory unless upstream planning truth explicitly depends on that source.

## Consistency gate

After writing the artifacts:

- inspect `use-cases/<module_id>.md` directly and confirm it is the authoritative operator-facing contract
- inspect `use-cases/<module_id>.json` directly and confirm it matches `~/.config/opencode/skills/Potato-Agent/potato-agent-common/schemas/use-case-design.schema.json`
- confirm the json is only a thin companion index for the markdown, not a competing narrative

## Output

1. In chat, present the proposed source `use_case_claims[]` first.
2. Then present the out-of-scope boundary and open questions.
3. Call out the required `DG-USE-CASE-*` gate.
4. After approval, write the authoritative markdown plus thin companion json and archive any superseded revision.
5. Recheck the written artifacts before returning success.
