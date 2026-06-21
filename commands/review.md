---
description: Traverse your Cisely model and report its health — coverage, gaps, lifecycle drafts, and belief-alignment between what you believe and what your stakeholders want.
argument-hint: "[optional focus: a persona, strategy, or concept]"
---

Run a **health review** of the company's Cisely model. Read-only — do not create or change anything.
Follow the `cisely-agency-model` skill's worldview. Optional focus: **$ARGUMENTS**.

1. **Confirm the MCP is connected** (`mcp__cisely__*`); if not, tell the user to run `/mcp` → **cisely**.

2. **Read the whole graph** (or the focus area): `GetMission`, `ListBeliefs`, `ListPersonas`,
   `ListOperatingContexts`, `ListStakeholders`, `ListGoals`, `ListExpectations`, `ListStrategies`,
   `ListInitiatives`, `ListMetrics`. Use `Get*` to inspect declarations and follow edges where needed.

3. **Report on these dimensions** (be specific — name the nodes, link them with their
   `https://app.cisely.dev/concise/...` URLs):

   - **Lifecycle hygiene** — what's still a DRAFT (hollow, not yet activated), and what content gate is
     blocking it (e.g. a goal with no statement, a strategy missing its core choices, an initiative with
     no falsifiable hypothesis).
   - **Demand coverage** — personas with no goals; goals not rooted in any belief; stakeholders with no
     expectations; expectations not proxying any goal (orphan proxies); latent/emerging expectations
     that are candidates for sharpening.
   - **Response coverage** — does a mission exist and is it active? strategies not rooted in a belief;
     strategies with no initiatives; initiatives with no metric; metrics measuring nothing.
   - **The loop** — expectations with **no metric measuring them** (the demand↔response loop is open
     there); metrics with no recent observations.
   - **Alignment (the headline)** — for each belief, list the goals and the strategies that root in it.
     **Call out the wins**: a belief that *both* a stakeholder goal and a business strategy root in is
     alignment realized. **Call out the risks**: strategies rooted in beliefs no stakeholder shares, or
     central stakeholder goals rooting in beliefs no strategy responds to.

4. **End with a short, prioritized list** of the highest-leverage next moves (e.g. "activate these 3
   draft personas", "this goal has no strategy serving it", "no expectation is measured — close the loop
   on `<expectation>`"). Frame declined/absent items as decisions to confirm, not just gaps.

Offer to act on any item via `/cisely:model`, or to chart a `/cisely:canvas` for a chosen persona.
