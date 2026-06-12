---
name: prepare-human-final-review
description: Prepare the concise Human Final Review prompt after runtime review scenarios close and before Technical Final Review.
compatibility: opencode
---

# Prepare Human Final Review (Potato Agent)

Use this Skill only from the runner/orchestrator after runtime craft/review has settled the planned review scenarios.

Human Final Review is the user's final functional-experience gate. It is not a review Node, not a tool result, and not acceptance.

## Goal

Produce a concise chat prompt that helps the user inspect the real operator/player experience.

Do not write a thick brief artifact. Use approved claims, review scenarios, and settled review results to explain what the user should try and what failure details matter.

## Read Boundary

Read from:

- `decision/<module_id>.json`
- approved `review/<module_id>/*.json` review scenario files
- `chain/<module_id>.json`
- current module runtime state
- relevant settled craft/review results, settlements, integrations, and review evidence

## Write Boundary

Do not write persistent artifacts. The only output is the user-facing prompt in chat.

## Hard Rules

- Keep the prompt short.
- Focus on the normal operator/player path: launch or entry point, user action, visible or interactable result, expected claim meaning, and substitutes the user should not rely on.
- Do not ask the user to trust craft self-check, generated JSON, headless output, backend counters, test scenes, or editor-only paths.
- Do not prepare Technical Final Review here.
- Do not decide the user's verdict.
- Do not edit project files or runtime state.

## Output

Return a concise Human Final Review prompt naming:

- the scenarios or claims the user should try
- the expected visible/operator outcome
- failure details to report: start state, steps taken, expected visible result, actual visible result, environment/build, screenshot/video if useful, and whether the issue is missing UI, wrong behavior, crash, performance, or confusion
- the next flow: pass moves to Technical Final Review; fail enters craft-only paired repair
