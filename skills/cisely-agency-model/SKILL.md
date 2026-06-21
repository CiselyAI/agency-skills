---
name: cisely-agency-model
description: >-
  Model a company's context in Cisely as a living graph — the response side (mission, beliefs,
  strategy, initiatives, metrics) and the demand side (personas, operating contexts, stakeholders,
  goals, expectations) — by wielding the Cisely MCP tools with the right discipline and order. Use
  whenever the user wants to model, build, populate, or refine their company, agency model, personas,
  operating contexts, stakeholders, goals, expectations, strategy, initiatives, metrics, beliefs,
  Jobs-to-be-Done, or value proposition in Cisely, or when the Cisely MCP is connected and they want
  to capture business context.
---

# Modeling a company in Cisely

You help the user turn their business into a **navigable agency graph** in Cisely, using the **Cisely
MCP** (tools prefixed `mcp__cisely__*` such as `CreatePersona`, `SituateStakeholder`,
`LinkExpectationProxiesForGoal`). This skill is the worldview, the discipline, and the playbook.

## The worldview in one breath

A business is an **exchange of value between agents**. You cannot control what your stakeholders
expect of you — you can only **choose whom to serve** and **design how you meet them**. Cisely makes
that explicit as two sides joined by belief:

- **Demand (what is wanted):** a **Persona** (invariant WHO) *holds* a **Goal** (a latent emotional
  deliverable). A persona seen through an **Operating Context** (a lens) becomes a **Stakeholder**,
  which *holds* **Expectations** (the only directly-measurable atoms of demand). An expectation
  *proxies for* a goal.
- **Response (what the business does):** a **Mission** (the WHY) is *pursued by* **Strategy** (the
  HOW), which is *executed by* **Initiatives** (the WHAT). **Metrics** measure strategies,
  initiatives, and — crucially — expectations.
- **Keystone:** **Belief.** Goals and strategies both *root in* beliefs. When a stakeholder's goal and
  the business's strategy root in the **same** belief, that is alignment — the thing that makes a
  business feel inevitable.

Full primer: [`references/agency-model.md`](references/agency-model.md). Read it before a first
session so you can converse fluently about the model.

## Before you start

1. **Check the MCP is connected.** Confirm `mcp__cisely__*` tools are available. If not, tell the
   user to run `/mcp`, select **cisely**, and complete the OAuth sign-in (or
   `claude mcp add --transport http cisely https://app.cisely.dev/mcp`). Don't fabricate results.
2. **Read what's already there** before adding. Call the `List*` tools (`ListPersonas`,
   `ListStrategies`, `GetMission`, …) so you build on the existing model instead of duplicating it.
   Modeling is **discovery**, continuous and never finished — expect to revise.

## Two ways to drive a session

Ask the user which they prefer (or infer from what they give you):

- **Guided interview** — elicit the model Socratically. Good when starting fresh. Ask about the
  agents they serve and what those agents are *really* reaching for beneath any feature request; what
  the business believes; how it chooses to win. Propose nodes and edges as you go.
- **Ingest from material** — they point you at decks, docs, transcripts, a website, or a codebase;
  you extract candidate personas / goals / strategies / expectations and **propose** the graph for
  their review before writing anything.

In both modes: **propose, confirm, then write.** Never bulk-create without the user seeing the shape.
See [`references/playbook.md`](references/playbook.md) for the interview prompts and ingest recipe.

## The build order (respect the dependencies)

Cisely enforces preconditions; follow this order so creates don't 409/404. Full tool-by-tool map,
including every born-linked and discovered-later edge, is in
[`references/tool-map.md`](references/tool-map.md).

1. **Mission first.** `EstablishMission` → `ReviseMission` (purpose, just cause, vision, values) →
   `ActivateMission`. Nothing on the response side can exist without it (`CreateStrategy` 409s
   `MISSION_REQUIRED` otherwise).
2. **Beliefs early.** `CreateBelief` → revise → activate. They're free-standing and both sides root
   into them later — capturing them early lets you wire alignment as you go.
3. **Demand side:**
   - `CreatePersona` (pick `nature`: HUMAN / ORGANIZATION / GROUP / SYSTEM / AI_AGENT) → revise →
     activate. Then `CreateGoal` (born held by that persona) → revise → activate.
   - `CreateOperatingContext` → revise → activate.
   - `SituateStakeholder` (needs an existing persona **and** context; must declare `principal_hood`).
   - `CreateExpectation` (born held by that stakeholder) → revise → activate.
   - `LinkExpectationProxiesForGoal` — only valid when the goal's persona **equals** the expectation's
     stakeholder's persona (else 409 `EXPECTATION_GOAL_HOLDER_MISMATCH`).
   - `LinkGoalRootedInBelief`.
4. **Response side:**
   - `CreateStrategy` (auto-pursues the mission) → revise → activate. `LinkStrategyRootedInBelief` —
     aim to root it in a belief a **goal also roots in** (that convergence is the alignment signal).
   - `CreateInitiative` (executes a strategy; activation requires a falsifiable hypothesis — a
     statement *and* a falsification criterion) → revise → activate.
   - `CreateMetric` → revise → activate. `LinkMetricMeasures{Strategy,Initiative,Expectation}` — favor
     measuring **expectations**, which closes the demand↔response loop. `RecordObservation` over time.
5. **Synthesize** Jobs-to-be-Done and the Value Proposition Canvas as *views* — see
   [`references/synthesis.md`](references/synthesis.md) and `/cisely:canvas`.

## Operating principles (high rigor)

- **Discovery, not invention.** A goal is *latent* — you infer it from proxies, you don't author it
  for the stakeholder. Help the user move expectations up the articulation gradient
  (latent → emerging → defined); that *is* the work.
- **The lifecycle is real.** Create makes a DRAFT; you must `Revise` to give it substance and
  `Activate` to commit (Cisely refuses hollow drafts with precise `*_CONTENT_REQUIRED` 409s). Only
  build edges onto things you intend to keep.
- **Declining is a decision.** Choosing *not* to serve a persona, or *not* to meet an expectation, is
  strategic — record it (archive with a rationale), don't silently omit it.
- **Push for alignment.** Whenever a goal and a strategy could root in the same belief, surface it.
  The strongest businesses share a conviction with the people they serve.
- **Confirm before writing; revise freely after.** Show the user the node/edge you're about to
  create. Wholesale revision is the norm — pass the full declaration each time, not a delta.
- **Echo the UI link after every change.** On success, give the user the deep link the tool returns
  (e.g. `https://app.cisely.dev/concise/personas/<id>`), and point at the **Agency Board**
  (`/concise/agency`) or **Purpose Board** (`/concise/purpose`) for the big picture.
- **Slugs are immutable handles.** Lowercase, kebab-case, stable. Display names are human-friendly.

## When you're modeling strategy *kinds*

If the user wants their own structured strategy types (technology, GTM, people-&-culture, …), that's
the **sub-strategy** surface — defer to the `cisely-sub-strategy` skill.
