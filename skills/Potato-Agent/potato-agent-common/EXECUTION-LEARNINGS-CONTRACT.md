---
title: Potato Agent execution learnings contract
status: active-rebuild
language: general
---

# Potato Agent Execution Learnings Contract

This document defines the active bounded learnings pipeline for unattended execution.

## 1. Core rule

Execution learnings are bounded same-route prior failure notes and background.

They are not replacement planning truth.

They are not gate, witness, or review-brief truth.

They are not acceptance truth.

They are not plugin-owned continuation truth.

## 2. Write path

Module-local prior failure notes live in:

- `sessions/<session_id>/modules/<module_id>.json`

Node state update is the first active writer that may distill narrow prior failure notes from craft/review results for later same-route scopes in that module.

## 3. Read path

Runtime packet preparation reads bounded prior failure notes from the current module session and carries them into the Node packet.

Producer leaves read that handoff field as bounded prior failure background only.

## 4. Content rule

Useful prior failure notes include only narrow same-route guidance such as:

- a concrete failure mode already encountered
- a known trap in the touched surface
- a known non-solution already tried in the current unattended route
- a local environment quirk relevant to the scope
- a repeated review failure signature already represented in the handoff repair gate or review work order
- an implementation hypothesis that a successor review has already falsified

Do not store broad route redesign ideas, user-preference arbitration, or replacement planning decisions as prior failure notes.

When a module is in review-targeted backtracking repair, preserve the still-relevant failure signature and falsified repair hypotheses ahead of generic success notes, but only as background. The actual repair delta, gate, witness, or review obligation must appear in the active craft or review packet.

## 5. Boundedness rule

Prior failure notes should stay:

- module-local
- same-route
- recent
- craft/review-relevant
- short enough to fit later handoffs without crowding out required source ids, scenario ids, route refs, and repair facts

## 6. Hard bans

- do not let prior failure notes outrank planning truth or runtime truth
- do not use prior failure notes as shadow replanning
- do not use prior failure notes as hidden target, gate, witness, or review authority
- do not carry stale or decorative notes forever
