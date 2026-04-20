# Blueprint Review Gate v2

This document defines how `blueprint-specification-alignment` should decide whether a draft is ready for `DG-BLUEPRINT-*`.

This document applies only after the stabilized landing rehearsal has already survived blueprint-specification-alignment entry screening, after the user-directed strengthening pass, and after one fresh strict draft review pass.

It does not govern the first-check landing rehearsal.

## 1. Core rule

`DG-BLUEPRINT-*` is not a style gate.

It is the gate for whether the implementation route has been pre-rehearsed strongly enough that downstream planning does not have to invent the decisive route.

Each decisive part of the route should now survive two tests at once:

- the route structure is honest and is not being forced into a decorative scaffold
- each real route unit makes the current move, the local sign, the wrong sign, and the next safe move explicit

## 2. Gate verdicts

Use exactly one of these verdict classes when finishing a blueprint draft.

### `ready_for_blueprint_gate`

Use only when:

- the draft survives the blueprint rubric
- no decisive blueprint gaps remain
- the route is clear enough that a `mini`-class obedient implementer could inherit it without inventing the decisive implementation route
- the route structure is honest and no tidy heading is still hiding several distinct moves or proof threads
- each real route unit already makes clear what to do now, what local sign proves it, what wrong sign means stop there, and what becomes safe next
- the user-named important parts and the user-named parts that are easy to get wrong and would cause rework are preplayed almost to direct-implementation readiness rather than stopping at abstract route labels
- the latest fresh `godot-rup-review-blueprint-draft` pass on the current draft also returns `ready_for_blueprint_gate`

### `revise_blueprint_locally`

Use when:

- the route body itself remains stable
- the missing clarity is locally recoverable inside specification alignment
- the missing clarity is still blueprint-level rather than upstream architectural uncertainty
- the current invocation has already completed its one allowed review pass and must now stop for user-directed continuation

### `return_to_blueprint_landing_rehearsal`

Use when:

- the route body itself still needs substantive rewrite
- the aligned draft is collapsing back into a design sheet, result summary, or other fake-preplay shape
- the draft is still hiding route ambiguity behind a forced or overly neat structure
- the problem cannot be repaired honestly without reopening landing rehearsal writing

### `return_to_architecture_review`

Use when:

- the missing clarity is upstream route uncertainty
- blueprint cannot honestly decide the route without re-opening technical route approval

### `recommend_spike`

Use when:

- a bounded technical uncertainty must be validated before blueprint truth can be honest

## 3. Ready-for-gate bar

Do not use `ready_for_blueprint_gate` unless all are true:

- each key use case has a real technical operator preplay
- the route structure is honest and does not depend on a forced scaffold
- the current real move is visible
- the local sign is visible
- the wrong sign and stop point are visible
- the next safe move is visible
- the first files or surfaces to open are visible when repo entry would otherwise stay vague
- the way work, state, or ownership hand forward is visible
- the tempting wrong route is explicit
- ownership boundaries are clear enough to survive execution
- repo grounding supports the decisive route claims
- if the module establishes a root boundary, bootstrap, candidate, swap, rebind, and first safe live-runtime touch timing are explicit enough to inherit
- if the repo lacks the module's primary entry contract, the blueprint has already chosen the first honest repo-local contract route
- the user-named important parts and the user-named parts that are easy to get wrong and would cause rework are detailed enough that downstream planning is mostly transcription of the route there rather than fresh route invention
- the blueprint is not leaking into workflow, proof, review, dispatch, or repair policy decisions

Open questions are allowed only when they do not change the first practical step, root ownership, primary entry contract, candidate-vs-live boundary, or first safe live-runtime touch point.

## 4. User-facing discussion rule

When discussing gate readiness with the user in chat:

- describe the route and verdict in Chinese
- keep the artifact itself in English
- do not paste a half-English artifact draft into the discussion unless the user asks for it

## 5. Suggested discussion shape

1. what was strengthened or corrected after the user's first-check feedback
2. the decisive remaining gaps, if any
3. whether the draft is now waiting for blueprint approval, still needs another local alignment repair round, or must go back to `blueprint-landing-rehearsal`
4. the current verdict class

## 6. Hard bans

- do not surface `DG-BLUEPRINT-*` just because the draft looks tidy
- do not hide decisive route gaps behind a long blueprint
- do not call the draft ready if weaker downstream execution still has to invent the route
- do not repair route-body collapse here by rewriting the route into a specification list; send it back to `blueprint-landing-rehearsal`
- do not auto-rerun blueprint draft review inside the same alignment invocation; stop after the first strict pass and report the blocking issues instead
