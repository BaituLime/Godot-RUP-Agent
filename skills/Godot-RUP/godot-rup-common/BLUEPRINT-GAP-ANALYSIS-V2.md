# Blueprint Gap Analysis v2

Use this guide while screening a stabilized landing rehearsal for packageability and while aligning or revising a blueprint draft after that rehearsal exists.

Use it after the first user check and after the user-directed strengthening pass.

Do not use it as a hidden early filter on the first-check landing rehearsal.

It may be used as the first internal screen inside `blueprint-specification-alignment` before any draft pair is written.

## 1. Core rule

A blueprint gap is any missing route clarity that would force downstream planning or execution to invent a crucial implementation judgment.

If the downstream implementer still has to invent the route, the blueprint is not ready.

Baseline bar remains: a `mini`-class obedient implementer should not need to invent the decisive implementation route.

Calibration nuance: the user-named important parts and the user-named parts that are easy to get wrong and would cause rework should be preplayed almost to direct-implementation readiness rather than staying abstract.

## 2. Gap classes

Use these classes when analyzing a rehearsal or draft.

### `route_gap`

- the draft states what should exist, but not how a real implementer would actually land the route

### `structure_gap`

- the route is being forced into a decorative scaffold it does not need
- one labeled route unit still contains several different first moves, several different local proofs, several different stop signs, or several different next-safe threads

### `first_step_gap`

- the draft does not make the current real implementation step explicit

### `local_proof_gap`

- the draft does not say what local sign proves the current step is real

### `stop_point_gap`

- the draft does not say what wrong sign means stop here and turn back

### `next_step_gap`

- the draft does not show what becomes safe or possible next once the current step is done

### `authority_gap`

- state ownership, boundary ownership, or conflict ownership is still implicit

### `wrong_route_gap`

- the easiest plausible wrong implementation route is not explicitly named and rejected

### `repo_grounding_gap`

- the route claim is not tied tightly enough to the repo surfaces that forced it

### `phase_boundary_gap`

- the draft leaks into workflow, proof, review, dispatch, or repair policy that should remain in later phases

### `composite_order_gap`

- the draft is still functioning like a decomposed composite order, design-constraint sheet, or result summary rather than route prose

### `handoff_chain_gap`

- the draft names an early move, but does not show how work, ownership, or prepared state is handed forward into the next safe step

### `bootstrap_gap`

- the draft introduces a new host, root, or composition boundary without making bootstrap and first live entry explicit

### `candidate_swap_gap`

- the draft does not make candidate-versus-live construction, swap, and rebind order explicit where that distinction is decisive

### `entry_contract_gap`

- the repo lacks the module's first real content or input contract, but the blueprint has not yet chosen the first honest route for it

### `live_touch_gap`

- the draft does not say when live runtime, live scene objects, or live presentation nodes may first be touched safely

### `derivation_gap`

- a required blueprint package section cannot be filled honestly from the current stabilized rehearsal plus approved upstream truth
- filling it would require new route judgment rather than bounded local repair

## 3. What to analyze

For each route thread, ask:

1. Could a `mini`-class obedient implementer say what they would do first tomorrow in this repo?
2. Could they say what local sign would prove the current move is real?
3. Could they say what wrong sign would make them stop there?
4. Once that move is done, could they say what becomes the next safe move and how work, state, or ownership hand forward?
5. Is the route tied to concrete repo surfaces rather than generic wording?
6. When boundary or live-state discipline matters, does the draft show the first honest entry contract, the first safe live touch, and how prepared work becomes live?
7. On the user-named important parts and the user-named parts that are easy to get wrong and would cause rework, is the route preplayed almost to direct-implementation readiness rather than merely named?
8. Is every labeled route unit really one coherent move, or is any tidy heading still hiding several different moves or proofs?
9. Could this supposed route unit mostly be paraphrased as renamed requirements, or shuffled around without losing much meaning? If so, it is still fake preplay.

Any "no" above is a real blueprint gap.

## 4. Recommended discussion shape

When discussing a draft with the user, describe blueprint gaps in Chinese.

Do not compress that discussion into a terse summary when the user needs visible route detail and concrete missing pieces.

A useful shape is:

1. strongest part of the draft
2. user-named important parts and user-named parts that are easy to get wrong and would cause rework that are already pinned strongly enough, if any
3. decisive gaps that still block `DG-BLUEPRINT-*`
4. whether the fix is local blueprint revision, upstream architecture return, or a spike recommendation

## 5. Gate consequence

If a decisive gap remains, do not surface `DG-BLUEPRINT-*`.

If the decisive gap is discovered before any draft pair is written, do not materialize or refresh the draft pair just to see what review says.

If repairing that gap would require substantive route-body rewrite rather than local draft alignment, return the module to `blueprint-landing-rehearsal` instead of forcing the draft through alignment.

If only bounded local naming, bridge, duplicate-cleanup, user-requested, or consistency repairs are needed to make the rehearsal honestly packageable, `blueprint-specification-alignment` may repair the current landing rehearsal first and continue.

If a required package section still cannot be derived honestly without new route reasoning, new action order, new validation logic, new stop-point logic, or forced new structure, stop before draft creation, report the mismatch explicitly, and return upstream.

Instead choose one of:

- revise blueprint locally
- return upstream to `architecture-review`
- recommend a bounded `spike`

## 6. Anti-pattern

Do not write a gap analysis like a second blueprint.
