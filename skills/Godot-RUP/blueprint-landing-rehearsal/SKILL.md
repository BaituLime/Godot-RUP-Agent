---
name: blueprint-landing-rehearsal
description: Write and stabilize the route-only landing rehearsal for one approved module
compatibility: opencode
---

# Blueprint Landing Rehearsal (Godot-RUP + C#)

Use this Skill to write the route-only landing rehearsal for one module after use-case design and architecture route approval, before blueprint specification alignment, workflow design, and DAG planning.

Optional references:

- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/HANDOFF-CONTRACT.md`
- `/home/bunny/.config/opencode/skills/Godot-RUP/godot-rup-common/HANDOFF-SKILL-SPEC.md`

## Goal

Discover and stabilize route truth.

Do **not** write a technical spec, architecture summary, contract doc, or responsibility matrix.

Write a landing rehearsal: a first-person implementation route from the perspective of an engineer who will start tomorrow.

Your job is not to define what the system is.
Your job is to simulate how you would land it without going wrong.

## Prose-first rule

Write the route as landing prose first.
Use structure only after the route is already real.

The prose should answer:
- what I touch first
- why I start there instead of at the more visible thing
- what small honest truth I make real now
- what tempting wrong route would fake progress
- what local sign proves the step is alive
- what wrong sign means stop here
- what becomes safe next

If headings, contract names, or step labels begin to dictate the content, stop and rewrite.
The prose should generate the structure, never the reverse.
Don't worry about the length, the key is to anticipate the details of the landing process, but avoid using unnecessary descriptive phrases.

## Route writing rule

Required style:
- write in first person: "If I started tomorrow, I would ..."
- follow actual landing order, not concept taxonomy
- keep spec language subordinate to landing logic
- write the route in prose first; use explicit signpost phrases only where they help the route stay clear

Useful cues include:
- "I would not start with ..."
- "The first honest cut is ..."
- "The tempting wrong route is ..."
- "The local sign that this step is real is ..."
- "If I see that, I stop here ..."
- "Once this stands, the next safe step is ..."

These are cues, not a fixed paragraph template.
Use them when they clarify the landing route.
Do not force every step to contain the same phrases in the same order.

Do **not**:
- make ownership summaries the main mode
- write "the approved route is ...", "the contract requires ...", or "the system must ..." as the skeleton
- fill a Block / Section / Step scaffold first and then backfill prose
- summarize the final design and disguise it as steps
- normalize every step to the same size, rhythm, or sentence pattern
- force every step to spell out the same cues in the same order

## Litmus test

The prose should generate the structure, never the reverse.

If you can delete most first-person landing language and the piece still mostly works, you wrote a spec. Rewrite it.

If the step order could be shuffled around without changing much meaning, you wrote a list, not a rehearsal. Rewrite it.

## Read boundary

Read only what is needed to rehearse the route honestly:
- resolve the active handoff root first
- approved `Use-Case Design`
- current `Decision Handoff`
- module-specific architecture references and approved constraints
- concrete repo surfaces needed for the route
- latest user direction for this module or ring iteration

Do not read blueprint schema while drafting this route.
Do not let realization ids, coverage tables, or index shape become the prose skeleton.
Do not resolve planning artifacts from the repo working tree when the active handoff root already carries them.

## Write boundary

You may write only:
- `blueprint/<module_id>.landing-rehearsal.md`

Do not write:
- `blueprint/<module_id>.draft.md`
- `blueprint/<module_id>.draft.json`
- `blueprint/<module_id>.md`
- `blueprint/<module_id>.json`
- `blueprint-history/<module_id>/...`

## Structure rule

Use only the structure the route actually needs.
Do not force multi-level hierarchy just because the module is large.

Use headings, numbered route units, or plain prose only when they actually clarify the route.
If a passage is clearer as plain prose under `Implementation Route`, keep it plain.

A real route unit should still make clear:
- the current move
- the local sign
- the wrong sign / stop point
- what becomes safe next

If one labeled route unit is still hiding several different first moves, several proofs, or several next-safe threads, split or rewrite it.

## Artifact shape

Write the artifact in English.
Keep the body under `Implementation Route`.

Do not add blueprint-packaging sections here:
- `Coverage Against Approved Use Cases`
- `Shared Route Decisions`
- `Derived Parallelization And Convergence`
- `Out Of Scope`
- `Open Questions`

Do not add json notes, realization labels, coverage matrices, or schema-shaped bullet lists.

## First user check

Before any self-check or child review:
1. write `blueprint/<module_id>.landing-rehearsal.md`
2. keep every part at the baseline bar
3. show the user one Chinese route breakdown
4. let the user point out:
   - which parts are important and easy to get wrong and would cause rework
   - obvious problems
   - missing details
   - where a step is still too big or too vague

## Strengthening pass

After the user points those things out:
- Work through only those user-named parts in much finer detail so that even a mini model agent can read it and know exactly how to implement it, without having to invent anything on its own.
- fix only the user-called-out problems or missing details
- split route units only where the feedback shows the route is still too big or too vague
- make only the consistency edits needed to keep the route coherent
- do not expand this into a full blueprint package

Stop when the route is stable enough for packaging.
Then hand off to `blueprint-specification-alignment`.

## Hard bans

- do not write a requirements bundle in narrative form
- do not write a process sheet in narrative form
- do not turn the route into a disguised checklist
- do not let decision-handoff constraints become the paragraph skeleton
- do not predefine DAG tasks, proof scopes, packet context, or dispatch choices
- do not let tidy structure substitute for real route clarity
- do not surface `DG-BLUEPRINT-*` here

## Output

1. Write `blueprint/<module_id>.landing-rehearsal.md`.
2. In chat, show the Chinese route explanation for the first user check.
3. After user feedback, strengthen only the user-named parts and the user-called-out issues.
4. When stable, return:
   - `landing_rehearsal_file: <path>`
   - `status: ready_for_specification_alignment`
5. If the route is still not stable after that round, say so explicitly and return:
   - `landing_rehearsal_file: <path>`
   - `status: needs_more_user_feedback`
