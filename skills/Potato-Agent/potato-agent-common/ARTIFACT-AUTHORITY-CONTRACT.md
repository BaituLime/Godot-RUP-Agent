---
title: Potato Agent artifact authority contract
status: active-rebuild
language: general
---

# Potato Agent Artifact Authority Contract

This document prevents current artifacts from inheriting obsolete handoff shapes.

## 1. Fact/Form Split

- Artifact form means field layout, required sections, gate wording, lifecycle wording, process template, and success/failure shape.
- Artifact form may come only from the current active schema, the current owning skill contract, and any current skeleton explicitly named by that owning skill.
- Historical handoff packets, attempts, settlements, session records, and predecessor artifacts may provide facts, provenance, failure signatures, paths, ids, observations, and lineage context only.
- Historical artifacts may not provide field layout, gate wording, section order, process template, or implied lifecycle state even when they look similar to the current artifact being produced.

## 2. Active Authority Bootstrap

- Artifact-producing work must read the current owning skill instructions and current artifact contract before reading old session artifacts for facts.
- When a machine artifact has a schema, artifact-producing work must read or otherwise use the current schema before using historical artifacts as factual input.
- Old session artifacts are never the first authority for how to write a new Node packet, Node result, final review record, state update, dossier, evidence summary, scheduler return, session state file, or planning handoff.

## 3. No-Template-Copy Rule

- Copying an old packet, attempt, settlement, final review record, dossier, evidence summary, scheduler return, or planning handoff as the example shape for a new artifact is invalid.
- Passing schema validation after copying an old shape does not make the artifact valid; the producer must be able to show that the current schema, current owning skill contract, or current skeleton supplied the form.
- If current form authority and historical artifact shape conflict, current form authority wins and the historical artifact may be cited only for facts.

## 4. Source Ledger

Artifact-producing steps should record in the artifact when the active shape allows it, or be able to state in their raw handback when it does not:

- `shape_source`: current schema, current owning skill contract, or current skeleton used for field layout and required sections
- `contract_source`: current skill or common contract that supplied lifecycle and gate semantics
- `fact_sources`: historical artifacts, current runtime/planning artifacts, review scenarios, code files, docs, commands, or evidence used only for facts/provenance
- `forbidden_template_sources_not_used`: old packets, attempts, settlements, dossiers, or handoff artifacts that were consulted only for facts or intentionally not consulted for form

## 5. Semantic Lint For Successor Reviews

- A successor review packet after predecessor repair must carry the predecessor failure signature and the semantic delta being checked.
- A successor review packet must not reduce the recheck to rerunning proof, confirming status, or replaying the old pass condition without target-local falsification checks.
- If the handoff does not visibly preserve the predecessor failure signature and semantic delta, it is insufficient even if the artifact validates structurally.
