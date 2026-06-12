---
name: record-human-final-review
description: Record the explicit Human Final Review result and route failures to craft-only paired repair.
compatibility: opencode
---

# Record Human Final Review (Potato Agent)

Use this Skill only from the runner/orchestrator when the user has given an explicit Human Final Review result.

Human Final Review is the final user functional-experience gate before Technical Final Review. It is not acceptance and not a review Node.

## Read Boundary

Read from:

- `decision/<module_id>.json`
- approved `review/<module_id>/*.json` review scenario files
- `chain/<module_id>.json`
- current module runtime state
- relevant settled craft/review results and settlements needed to map user feedback to scenarios, claims, and candidate craft Nodes
- the user's explicit Human Final Review verdict and notes from the current runner conversation

## Write Boundary

Write only:

- `sessions/<session_id>/human-final-review/<module_id>/human-final-review-record.json`

## Hard Rules

- Preserve the user's explicit verdict exactly as `pass`, `fail`, `more_checks`, or `hold`.
- Do not invent checked items, notes, scenario ids, or approval.
- Map user feedback to `review_scenario_ids[]`, `source_use_case_claim_ids[]`, and candidate target craft Nodes when the user's words and runtime truth support it.
- Each checked item must record operator action, expected visible outcome, observed visible outcome, environment/build reference, result, review scenario ids, and source use-case claim ids.
- A `pass` verdict is legal only when every checked item result is `pass`, at least one review scenario and source claim are covered, and no repair delta remains.
- If the user says review missed the real problem, record `review_scenarios_need_rework = true`; the runner may explain options, but the user decides whether to return to review-scenario-design or chain-plan.
- On fail, do not route back to a review Node. Record repair deltas for craft-only paired repair when attributable.
- Runner/orchestrator must not edit code during paired repair; repair goes through `potato-agent-craft` only.
- Do not promote module state, write acceptance evidence, perform Technical Final Review, or ship.

## Consistency Gate

- Re-read the written record.
- Confirm it validates against `human-final-review-record.schema.json`.
- Confirm failed results do not imply acceptance or review rerun authority.
