---
name: architecture-confirm
description: Propose and record the approved technical route for one Potato Agent module through route choice and route consequence rounds.
compatibility: opencode
---

# Architecture Confirm (Potato Agent)

Use this Skill after approved use-case design and before `review-scenario-design`.

Architecture turns approved `use_case_claims[]` plus direct project evidence into route authority. It does not write review scenarios, review targets, packets, chain Nodes, runtime evidence, acceptance verdicts, or handoff/project decision artifacts outside the Decision Handoff.

This Skill has two dialogue rounds in one context:

1. Round 1: Route Choice.
2. Round 2: Route Consequence.

Round 1 writes nothing. Round 2 writes the Decision Handoff only after explicit user approval.

## Read Boundary

Read from:

- approved Use-Case Design, including canonical `use_case_claims[]`
- direct repo/project facts needed to judge route feasibility: seams, platform constraints, data ownership, integration points, lifecycle/resource limits, and blockers
- relevant project docs and architecture references
- registered read-only knowledge sources when they can locate route-relevant facts faster, confirmed by direct reads before use

## Write Boundary

Write only in Round 2 after explicit user approval:

- `decision/<module_id>.json`

The file must use `decision_handoff_format = potato_agent_decision_handoff/v10`.

Do not paste the full JSON in chat. Write the file, re-read it, validate it, and report the path plus validation result.

## Round 1: Route Choice

Round 1 answers whether the user has a real technical choice. It is chat-only and concise. Do not write Round 1 content to the main Decision Handoff JSON.

Present compact in chat:

- `Credible Routes`: only routes the user could actually approve, written as short route cards
- `Rejected Non-Routes`: only ideas that were proposed by the user or naturally appeared during repo evidence review and failed as architecture; do not invent silly examples
- `Recommendation`: selected route id or no selection when user choice is required
- `Open Questions`: only missing facts that block choosing a route, or facts that require a bounded `spike`
- `Decision Gate`: ask the user to choose among credible routes or confirm the only credible route with `DG-ARCH-ROUTE-CHOICE-*`

Keep Round 1 short. Do not write a proof essay. Do not restate the use-case design. Do not pad the answer with obvious facts, defensive explanations, or contrived bad alternatives.

A credible route card must say:

- what approach the route uses
- the short sequence of production changes it would make
- the real tradeoff when more than one credible route exists

Mention extra technical constraints only when they are the route difference or a route blocker. Do not give every route the same checklist.

Do not create fake alternatives. If only one route survives, say so and show the reason. Do not package rejected non-routes as options.

A rejected non-route must name the concrete failing constraint in one sentence.

Skip `Rejected Non-Routes` when there are no real rejected ideas. Do not manufacture rejection bullets to make the chosen route look stronger.

If no credible route can be identified, stop for use-case repair or a bounded `spike`.

## Round 2: Route Consequence

Round 2 happens only after the user chooses or confirms a route in Round 1.

Present in chat only:

- selected route id
- implementation detail ids, titles, `purpose_summary`, and `content[]` rows that need user review; do not show `source_use_case_claim_ids[]` in chat
- cross-cutting contract ids and `content[]` rows that need user review; do not show `applies_to_implementation_detail_ids[]` in chat
- open risks as one plain description per risk; do not show `risk_id`, `applies_to_implementation_detail_ids[]`, or `required_action` labels in chat
- open questions that remain before writing the Decision Handoff
- `Decision Gate`: ask the user to approve route consequences with `DG-ARCH-ROUTE-CONSEQUENCE-*`

Keep chat readable, but do not hide implementation details that need user review. Do not paste the full JSON in chat.

After Round 2 approval, write `decision/<module_id>.json` as a v10 Decision Handoff. The main JSON contains only:

- `decision_handoff_format`
- `module_id`
- `use_case_design_path`
- `source_use_case_design_revision_id`
- `approved_route_id`
- `selected_route_implementation_details`
- `cross_cutting_contracts`
- `open_risks`
- `decision_gates`
- `approved_at`

Do not write route choice notes, route consequence wrappers, goals, scope lists, or acceptance strategy into the Decision Handoff.

Each `selected_route_implementation_details[]` item contains:

- `implementation_detail_id`
- `title`
- `purpose_summary`
- `source_use_case_claim_ids[]`
- `content[]` as route rows with `id` and `text`
- `forbidden_simplifications[]` as route rows with `id` and `text`
- `technical_final_review_focus[]`

Use `content[]` for the selected route authority rows that describe fact/resource ownership, movement of facts between systems, setup and cleanup behavior, seams, callable boundaries, implementation obligations, and preservation obligations. Keep each row direct and reviewable.

Each `cross_cutting_contracts[]` item contains:

- `contract_id`
- `applies_to_implementation_detail_ids[]`
- `content[]` as route rows with `id` and `text`

Write allowed behavior, forbidden shortcuts, and final technical review concerns directly in `content[]`. Do not create separate cross-cutting forbidden-simplification or Technical Final Review lists.

Each `open_risks[]` item contains:

- `risk_id`
- `applies_to_implementation_detail_ids[]`
- `risk`
- `required_action`

`decision_gates[]` must include approved gates for both:

- `architecture-route-choice`
- `architecture-route-consequence`

## Route Implementation Detail Standard

Every route implementation detail should be reviewable without reading a long prose blob. A reviewer should be able to pick one `implementation_detail_id` and see:

- what technical change the selected route requires
- why this implementation detail exists
- what code is allowed to do
- what code must not do
- how Technical Final Review will inspect it

Use ordinary language, but do not replace technical structure with use-case prose. Use-case says what the operator wants. Architecture says where the system will own state, data flow, lifecycle, seams, and forbidden shortcuts so the use case cannot be faked.

## Decision Posture

- The user owns the final technical route.
- Recommend and challenge, but do not record approval before the user passes the relevant gate.
- If Round 1 has multiple credible routes, ask the user to choose.
- If Round 1 has one credible route, ask the user to confirm it.
- If the approved use-case design is ambiguous or route-incompatible, stop and return upstream instead of turning ambiguity into architecture text.
- If route feasibility depends on a repo/platform fact that is still unknown, stop and request a planner-owned `spike`.

## Output

1. Round 1: present route choice in chat only and ask for `DG-ARCH-ROUTE-CHOICE-*` approval.
2. Round 2: after user choice or confirmation, present concise route consequences and ask for `DG-ARCH-ROUTE-CONSEQUENCE-*` approval.
3. After Round 2 approval, write `decision/<module_id>.json` in v10 shape.
4. Re-read the written decision JSON and confirm it validates against `decision-handoff.schema.json` before returning success.
