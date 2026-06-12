---
title: Potato Agent execution conductor contract
status: active-rebuild
language: general
---

# Potato Agent Execution Conductor Contract

This document defines the active conductor-side contract for unattended execution.

## 1. Conductor family

Execution conductors are the coordination surfaces that move a run forward without becoming craft or review producers.

The active conductor family is:

- `potato-agent-runner` as the top-level root execution shell
- `orchestrate-runtime` as the internal scheduler conductor
- same-context scheduler-local handoff preparation, Node state update, and integration phases

These surfaces coordinate execution.

They do not replace bounded producer leaves.

## 2. Core rule

Conductors coordinate.

Runtime Node agents execute one bounded Node scope and hand back one truth artifact.

Do not collapse those jobs together.

## 3. Root runner shell authority

`potato-agent-runner` owns:

- top-level unattended execution entry and same-session continuation
- resolving or creating the single-module session for each requested module iteration
- continuation registration and stop integration
- runner-owned reread after every returned scheduler slice
- user-facing surfacing under `RUNNER-RULES.md`
- user-facing surfacing when visible replanning is required after runtime repair cannot proceed inside existing Node contracts
- runner-owned decision about whether a stopped session should later be cleaned or preserved

`potato-agent-runner` may not:

- become the normal producer of craft or review work
- branch on scheduler prose instead of reread runtime truth
- host the scheduler inside a child agent
- let plugin-local continuation state replace session truth
- create a new session merely because the runner was invoked again or resumed after a lawful stop
- reuse a superseded or shipped session directory as the live session for a new module chain iteration
- create one shared session for multiple modules

## 4. Scheduler conductor authority

`orchestrate-runtime` owns:

- reading current runtime truth
- selecting ready work under the current head Node for `session.json.module_id`
- running handoff preparation, Node state update, and integration in the same scheduler context
- dispatching first-level producer children only
- keeping MCP work inside producer children and never creating scheduler-owned MCP proxy paths
- waiting only on already-dispatched producer children
- dispatching review-targeted craft repair when `repair_deltas[]` are attributable to existing Node contracts
- stopping at `replan_needed` when the defect is a route/chain-boundary failure or cannot be attributed to an existing Node contract

`orchestrate-runtime` may not:

- surface to the user directly
- become a planning surface
- author chain or planning repair
- silently alter route, scope, use-case truth, acceptance bar, proof rigor, or final acceptance boundary; non-major uncertainty continues only as a warning with explicit directive refs
- treat producer children as new schedulers
- use a returned summary sentence as stop authority instead of reread runtime truth

## 5. Same-context rule

Packet preparation, Node state update, and integration are conductor-local phases inside the scheduler context.

They are not producer children.

They are not new user-facing execution surfaces.

## 6. Node-agent boundary rule

Runtime Node agents are bounded first-level subagents by default.

Conductors must treat them as:

- bounded
- handoff-driven
- Node-kind-specific
- truthful-result-producing
- unable to spawn nested producer subagents

Conductors must not treat them as:

- nested schedulers
- replacement planners
- hidden acceptance authorities
- top-level continuation owners

## 7. Conductor verification rule

Conductors should not trust child self-report by itself.

Conductor action must branch from authoritative reread inputs such as:

- `session.json`
- `modules/<module_id>.json`
- handoff truth
- craft/review result artifacts
- Node state results

Producer prose is explanatory only.

## 8. Role-mismatch handback rule

If a producer child truthfully hands back that:

- the handoff cannot name the source claim ids, review scenario ids, production seam, read boundary, required review path, or repair target needed for bounded craft or review work
- the assigned Node kind is wrong for the actual work
- the scope now needs broader route change or planning change
- the scope needs MCP through a formal request
- the scope needs a detail-only override proposal
- the scope needs user clarification for a local detail

then conductor surfaces must treat that as handback truth to be processed inside runtime law.

They must not instruct the child to silently widen its own authority.

## 9. Continuation boundary

Conductors participate in continuation law, but they do not all own it equally.

- `potato-agent-runner` owns root continuation posture and user-facing stop surfacing
- `orchestrate-runtime` owns legal internal yields and scheduler-side runtime progression
- runtime producer leaves do not own top-level continuation sovereignty

## 10. Failure-repair boundary

The conductor family must preserve the current repair law.

- scheduler repairs only through craft repair, detail override, user clarification, or review-targeted backtracking inside existing Node contracts
- scheduler stops at real `replan_needed` when a defect changes route/chain boundaries or cannot be attributed to an existing Node contract
- runner may not invoke delegated delta planning; visible replanning belongs to ordinary planning surfaces after the runtime stop is surfaced
- after visible replanning updates approved planning truth, the runner starts a fresh successor session instead of mutating stale live session truth in place; ordinary stop/resume in the same chain iteration uses the same session

## 12. Final Review Boundary

- after runtime review scenario closure, the runner owns Human Final Review coordination
- failed Human Final Review routes to craft-only paired repair; the scheduler does not dispatch review Nodes for issues the user already observed
- Technical Final Review runs only after Human Final Review passes
- Technical Final Review repair goes through craft and may make Human Final Review stale by user decision

## 11. Hard bans

- do not let the runner shell quietly become a generic implementation subagent
- do not let the scheduler quietly become a planning surface
- do not let Node agents quietly become conductors
- do not let child self-report override runtime reread truth
- do not let conductor convenience bypass the Node-contract repair boundary
