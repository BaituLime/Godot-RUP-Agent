---
name: assemble-acceptance-dossier
description: Assemble one module acceptance dossier from claims, route authority, review scenarios, final reviews, integration, and evidence.
compatibility: opencode
---

# Assemble Acceptance Dossier (Potato Agent)

Use this Skill after runtime reaches `ready_for_acceptance` and before final module acceptance.

The dossier is a collected readback bundle, not a verdict.

## Read Boundary

Read only:

- `decision/<module_id>.json`
- approved `review/<module_id>/*.json` review scenario files
- `chain/<module_id>.json`
- `sessions/<session_id>/modules/<module_id>.json`
- relevant settlements, craft/review results, integrations, and evidence summaries
- `sessions/<session_id>/human-final-review/<module_id>/human-final-review-record.json`
- `sessions/<session_id>/technical-final-review/<module_id>/technical-final-review-record.json`
- registered read-only knowledge sources only to locate already-cited artifacts for direct reread

## Write Boundary

Write only:

- `sessions/<session_id>/acceptance/<module_id>/dossier.json`

## Hard Rules

- Collect exact evidence references; do not judge pass or fail here.
- Copy the exact current source id set from `use_case_claims[]` into `planned_source_ids[]`.
- Emit exactly one `source_judgment_inputs[]` row per source id.
- Emit `scenario_coverage[]` for every relevant review scenario.
- Mark any open, missing, blocked, or stale review scenario visibly in `scenario_coverage[]` and `pending_risks[]`.
- Require concrete passed Human Final Review and Technical Final Review records before a complete dossier.
- Write `human_final_review_verdict = pass` and `technical_final_review_verdict = pass` only after rereading the concrete records.
- Surface downgraded evidence explicitly, including weaker-than-planned rigor, substituted proof type, deterministic fallback, hard-coded output path, generated/synthetic artifact path, route-string proof, self-report, or artifact-exists-only support.
- Do not promote craft self-check, debug aids, generated JSON, headless bool, test scenes, or backend counters into final support for operator-visible claims.
- If final-review repair made earlier review stale, record that staleness instead of hiding it.
- Do not edit project files.

## Consistency Gate

- Re-read the dossier and confirm it remains a bundle rather than a verdict.
- Confirm it validates against `acceptance-dossier.schema.json`.
- Confirm `planned_source_ids[]` matches current `use_case_claims[]`.
- Confirm scenario coverage is complete and visible.
