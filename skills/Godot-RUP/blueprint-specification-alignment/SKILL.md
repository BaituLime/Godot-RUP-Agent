---
name: blueprint-specification-alignment
description: Assemble a stabilized landing rehearsal into the blueprint draft and gate package without flattening the route
compatibility: opencode
---

# Blueprint Specification Alignment (Godot-RUP + C#)

Package one stabilized landing rehearsal into the blueprint draft pair and, after explicit approval, the authoritative blueprint pair.

This skill packages route truth.
It does not rediscover route truth.
It does not invent new specification truth.
It does not turn landing rehearsal into a specification list.

Canonical references:

- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/TERMS.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/HANDOFF-CONTRACT.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/HANDOFF-SKILL-SPEC.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/BLUEPRINT-REVIEW-RUBRIC.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/BLUEPRINT-GAP-ANALYSIS-V2.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/BLUEPRINT-REVIEW-GATE-V2.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/schemas/execution-blueprint.schema.json`

## Read boundary

Read only:

- the active handoff root first
- `<handoff_root>/blueprint/<module_id>.landing-rehearsal.md`
- approved `<handoff_root>/use-cases/<module_id>.md`
- current `<handoff_root>/decision/<module_id>.md`
- current `<handoff_root>/decision/<module_id>.json`
- repo surfaces only when needed to resolve an honest alignment ambiguity
- the execution blueprint schema only after the markdown route body is stable

Do not resolve planning artifacts from the repo working tree when the active handoff root already carries them.
Do not let schema fields, realization ids, or coverage bookkeeping dictate the prose order.

## Write boundary

You may write only:

- `blueprint/<module_id>.landing-rehearsal.md` for bounded local repairs that add no new route judgment
- `blueprint/<module_id>.draft.md`
- `blueprint/<module_id>.draft.json`
- `blueprint/<module_id>.md`
- `blueprint/<module_id>.json`
- `blueprint-history/<module_id>/<blueprint_revision_id>.md`
- `blueprint-history/<module_id>/<blueprint_revision_id>.json`

## Source law

`blueprint/<module_id>.landing-rehearsal.md` is the route source.

- keep the draft route body substantially the same as the rehearsal
- preserve the route structure the rehearsal actually needs
- do not force a fixed scaffold onto the draft
- do not turn the route into a design sheet, result summary, coverage-first outline, realization bucket map, or disguised specification list

Allowed bounded local repairs are limited to:

- small naming repairs
- short bridge sentences
- duplicate cleanup
- explicit user-requested fixes
- consistency repairs needed to keep the packaged draft honest

If a repair needs new route reasoning, new action order, new validation logic, new stop-point logic, or forced new structure, stop and return upstream instead.

## Protocol

Do this in order.

### 1. Screen the rehearsal before writing any draft

Check whether the current rehearsal is both:

- real landing preplay rather than a process sheet, design-constraint sheet, result summary, requirements bundle, or disguised specification list
- honestly packageable into the required blueprint sections from current rehearsal truth plus approved upstream truth

If only bounded local repairs are needed, repair `blueprint/<module_id>.landing-rehearsal.md`, reread it, and continue.

If honest packaging would require substantive route-body rewrite, or if any required package section cannot be filled without new route judgment, stop before draft creation and report the exact mismatch.

### 2. Write the draft pair

Write:

- `blueprint/<module_id>.draft.md`
- `blueprint/<module_id>.draft.json`

Markdown order is fixed:

1. `Implementation Route`
2. `Coverage Against Approved Use Cases`
3. `Shared Route Decisions`
4. `Derived Parallelization And Convergence`
5. `Out Of Scope`
6. `Open Questions`

Rules:

- `Implementation Route` stays first and primary
- derived sections stay brief and may not become a second route source
- do not replace the route body with capability buckets, decorative outlines, or forced structure
- json is a thin index only; write it only after the markdown is stable

### 3. Run exactly one strict review pass

Run:

- `/godot-rup-review-blueprint-draft <handoff_root>/blueprint/<module_id>.draft.md`

Do not auto-repair and rerun review inside the same invocation.

### 4. Choose exactly one outcome

Choose the outcome from the strict review result plus `BLUEPRINT-GAP-ANALYSIS-V2.md` and `BLUEPRINT-REVIEW-GATE-V2.md`.

Use exactly one of these outcomes:

- `return_to_blueprint_landing_rehearsal`
  - use when the review or screening shows fake preplay, substantive route rewrite need, or route ambiguity being hidden by forced structure
- `return_to_architecture_review`
  - use when the missing clarity is upstream route uncertainty
- `recommend_spike`
  - use when a bounded technical uncertainty must be validated before blueprint truth can be honest
- `revise_blueprint_locally`
  - use when the route body is stable but the first strict review found only local packaging or alignment issues; report them and stop for user-directed continuation
- `ready_for_blueprint_gate`
  - use only when the draft survives the strict review, blueprint gap analysis, and review-gate law without decisive gaps

Do not patch a collapsing route body just to get through review.

### 5. Report

After the review pass, report in Chinese:

- whether the rehearsal passed entry screening before draft creation
- what was aligned or corrected
- any decisive remaining gaps or local revision items
- the single chosen outcome above

Surface `DG-BLUEPRINT-*` only when the chosen outcome is `ready_for_blueprint_gate`.

### 6. Promote only after explicit approval

Only after `ready_for_blueprint_gate` and explicit user approval:

- archive the prior approved revision if one is being superseded
- promote `blueprint/<module_id>.draft.md` to `blueprint/<module_id>.md`
- promote `blueprint/<module_id>.draft.json` to `blueprint/<module_id>.json`
- remove the current `*.draft.*` pair after promotion

## Hard bans

- do not rewrite the route body around schema fields or realization labels
- do not add specification truth that is not already forced by the stabilized route
- do not silently absorb route-body failure as a local alignment fix
- do not surface `DG-BLUEPRINT-*` after a failed screen or failed review just because the document looks tidy
