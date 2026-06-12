---
name: run-technical-final-review
description: Run the Technical Final Review after Human Final Review passes and before acceptance dossier assembly.
compatibility: opencode
---

# Run Technical Final Review (Potato Agent)

Use this Skill after Human Final Review has passed and before acceptance dossier assembly.

Technical Final Review is execution-stage code and route-quality review. It is not Review Inspector, not Human Final Review, and not acceptance.

## Goal

Judge whether the final integrated module implementation still follows selected route implementation details and cross-cutting contracts, and avoids concrete blocking defects: hard-coded proof paths, route bypasses, production debug residue, duplicate wrapper layers without responsibility, conflicting ownership for the same fact or resource, unbounded polling, unowned cleanup, and test/debug-only dependencies in production paths.

## Read Boundary

Read from:

- approved use-case design
- `decision/<module_id>.json`, including `selected_route_implementation_details[].technical_final_review_focus[]` and cross-cutting contract `content[]`
- approved `review/<module_id>/*.json` review scenario files
- `chain/<module_id>.json`
- module session state, settlements, integrations, and final changed code surfaces
- Human Final Review record

## Write Boundary

Write only:

- `sessions/<session_id>/technical-final-review/<module_id>/technical-final-review-record.json`

## Hard Rules

- Require a passed Human Final Review record before starting.
- Inspect changed files against selected route implementation details, implementation-detail-local `content[]`, implementation-detail-local `forbidden_simplifications[]`, implementation-detail-local `technical_final_review_focus[]`, cross-cutting contract `content[]`, resource ownership, test/debug residue, and performance-sensitive loops. Record file/route anchors for every pass or finding.
- Do not use Technical Final Review as a functional pass substitute.
- Do not rerun Review Functional or Review Inspector as a review Node.
- Do not edit code.
- If blocking findings require repair, route them to craft repair and mark whether previous Human Final Review may be stale.
- If repair occurs after Technical Final Review, runner/orchestrator must tell the user what changed and let the user decide whether to redo Human Final Review.
- Every focus result must record implementation detail or contract refs, inspected anchors, changed files or surfaces, the risk checked, and why it passes or fails.
- A `pass` verdict is legal only when route conformance is `aligned`, every focus result is `pass`, no blocking finding remains, no repair delta remains, and Human Final Review staleness risk is `none`.

## Consistency Gate

- Re-read the written record.
- Confirm it validates against `technical-final-review-record.schema.json`.
- Confirm the record does not claim functional acceptance.
