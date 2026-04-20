---
title: Godot-RUP blueprint draft review
status: active-rebuild
language: C#
---

# Godot-RUP Blueprint Draft Review

Treat this skill as the strict findings-first review pass for one blueprint draft.

This skill does not revise the draft.
This is not a happy-path check.
Use a fresh child context for each ordinary draft review.

Run it only after `blueprint-specification-alignment` has already written the current draft pair under the active handoff root.
Do not use it on the first-check landing rehearsal.

## Canonical references

- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/TERMS.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/HANDOFF-SKILL-SPEC.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/BLUEPRINT-REVIEW-RUBRIC.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/BLUEPRINT-GAP-ANALYSIS-V2.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/BLUEPRINT-REVIEW-GATE-V2.md`

Judge only against current common law plus the current module's contract artifacts.
Do not use other modules' blueprints, historical handoff artifacts, or prior blueprint revisions as the review baseline.

## Read boundary

Resolve the active handoff root first.

Then resolve exactly one draft markdown target from the explicit request:

- `blueprint/<module_id>.draft.md`
- `<handoff_root>/blueprint/<module_id>.draft.md`

Do not resolve the draft from the repo working tree.
Do not guess by searching the repo for a similarly named planning artifact.

From that same handoff root, read:

- the matching `blueprint/<module_id>.draft.json`
- `blueprint/<module_id>.landing-rehearsal.md`
- approved `use-cases/<module_id>.md`
- approved `decision/<module_id>.md`
- approved `decision/<module_id>.json`

Read repo surfaces only to verify decisive route claims after the planning artifacts are already resolved.

If the target draft path does not resolve inside the active handoff root, treat that as a finding and still write the temporary review file.
If a required peer artifact is missing, treat that as a finding and still write the temporary review file.

## Write boundary

You may write exactly one non-authoritative temporary review file:

- `/tmp/godot-rup-blueprint-review/<module_id>.md`

You may overwrite the prior file at that same path.
Do not edit handoff artifacts.
Do not edit the draft.

## Review law

Findings come first.
Prefer decisive route gaps over broad style commentary.

Review especially for:

- fake-preplay shapes such as process sheet, simulated player journey, requirements bundle, or design-constraint summary wearing route prose
- route structure honesty rather than decorative template neatness
- whether any labeled route unit is still hiding several current moves, proofs, stop points, or next-safe threads
- current move, local proof sign, wrong-sign stop point, and next-safe-move visibility
- entry contract, ownership boundary, candidate-versus-live route, swap/rebind order, and first safe live-runtime touch timing when decisive
- object or seam handoff clarity
- whether the draft has been flattened or distorted relative to `blueprint/<module_id>.landing-rehearsal.md`

## Protocol

Do this in order.

### 1. Review exactly one draft set

Review only the resolved current-module draft set.
Do not broaden scope.

### 2. Choose exactly one verdict

Choose the verdict from the current draft plus `BLUEPRINT-REVIEW-RUBRIC.md`, `BLUEPRINT-GAP-ANALYSIS-V2.md`, and `BLUEPRINT-REVIEW-GATE-V2.md`.

Use exactly one:

- `ready_for_blueprint_gate`
- `revise_blueprint_locally`
- `return_to_blueprint_landing_rehearsal`
- `return_to_architecture_review`
- `recommend_spike`

### 3. Write the temporary review file

Write the file in English in this order:

1. `Target`
2. `Findings`
3. `Gate Judgment`
4. `Residual Risk`

Each finding should be concrete, cite file and line references when possible, and focus on route gaps, route-structure failure, ownership gaps, fake-preplay shape, or gate-blocking ambiguity.

If there are no findings, say so explicitly.

### 4. Return only the review result

Return only:

- `review_file: <path>`
- `findings: <count>`
- `verdict: <verdict>`

## Hard bans

- do not revise the draft
- do not broaden the review to alternate artifacts or other modules
- do not guess missing planning truth
- do not bury decisive findings under style commentary
- do not return anything except the review file path, findings count, and verdict
