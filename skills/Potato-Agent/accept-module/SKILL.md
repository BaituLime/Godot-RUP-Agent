---
name: accept-module
description: Judge one module dossier and write the authoritative final module acceptance summary. Use only after runtime and final reviews are complete.
compatibility: opencode
---

# Accept Module (Potato Agent)

Use this Skill only during the separate acceptance stage after runtime reaches `ready_for_acceptance`.

## Goal

- judge one module for final acceptance after Human Final Review and Technical Final Review already passed
- write the authoritative module acceptance summary

## Read Boundary

Read only:

- `sessions/<session_id>/acceptance/<module_id>/dossier.json`
- `decision/<module_id>.json`
- approved `review/<module_id>/*.json` review scenario files
- `chain/<module_id>.json`
- Human Final Review record cited by the dossier
- Technical Final Review record cited by the dossier
- relevant craft/review results and settlements cited by the dossier
- cited authoritative evidence summaries

## Write Boundary

Write only:

- `evidence/<module_id>/module-acceptance/summary.json`

## Hard Rules

- Run only after runtime reaches `ready_for_acceptance`.
- Require concrete passed Human Final Review and Technical Final Review records.
- Require every acceptance-relevant review scenario to be closed, not open, missing, blocked, or stale.
- Judge approved use-case claims against the dossier, review scenario closure, review functional evidence, Human Final Review, Technical Final Review, integration state, and cited authoritative evidence.
- Judge architecture through route-conformance findings, Technical Final Review, settlements, and unresolved risks.
- Reject self-report, deterministic fallback, hard-coded output paths, generated/synthetic test paths, route strings, green Verify/Headless output, craft self-check, debug aids, and artifact-exists-only references as support for operator-visible acceptance claims.
- Do not reopen architecture or runtime planning.
- Do not initiate Human Final Review or Technical Final Review inside acceptance.
- Do not promote runtime state here; `potato-agent-acceptor` promotes only after rereading the acceptance summary.
- Do not auto-ship.

## Consistency Gate

- Re-read `evidence/<module_id>/module-acceptance/summary.json`.
- Confirm it uses `evidence-summary.schema.json` with `kind = acceptance`.
- Confirm exact passing acceptance evidence requires `status = pass`, `outcome = done`, and `completion_quality = exact`.
- Confirm scenario coverage, source judgments, Human Final Review refs, Technical Final Review refs, and forbidden simplification breaches are represented.
- If required proof is missing, stale, or contradictory, write failing or blocked acceptance evidence instead of a misleading pass summary.
