---
description: Synthesize Jobs-to-be-Done and a Value Proposition Canvas for a persona, read straight off your Cisely graph.
argument-hint: "<persona or stakeholder> (e.g. 'enterprise buyer')"
---

Synthesize **Jobs-to-be-Done** and a **Value Proposition Canvas** for a persona/stakeholder, as a
read-only **view** derived from the Cisely graph. Do not author or change anything. Follow the
`cisely-agency-model` skill and its [`references/synthesis.md`](../skills/cisely-agency-model/references/synthesis.md).
Target: **$ARGUMENTS**.

1. **Confirm the MCP is connected** (`mcp__cisely__*`); if not, tell the user to run `/mcp` → **cisely**.

2. **Resolve the target.** If `$ARGUMENTS` is empty or ambiguous, `ListPersonas` and ask the user which
   persona (and optionally which operating context / stakeholder) to chart.

3. **Gather the demand side** — `GetPersona`; its goals (`ListGoals`); its stakeholders
   (`ListStakeholders`) and their expectations (`ListExpectations`); follow `PROXIES_FOR` from each
   expectation to its goal; note each expectation's valence (PAIN/GAIN) and articulation.

4. **Gather the response side** — the strategies serving this persona/context, their initiatives
   (`ListInitiatives`), and the metrics measuring those expectations/initiatives (`ListMetrics`).

5. **Render Jobs-to-be-Done** — one job story per defined expectation:
   **"When** `<operating context>`**, I want to** `<expectation>`**, so I can** `<goal>`**."** Flag
   latent/emerging expectations as not-yet-crisp jobs (a discovery opportunity).

6. **Render the Value Proposition Canvas:**
   - **Customer profile** — Jobs (goals), Pains (PAIN-valence expectations), Gains (GAIN-valence).
   - **Value map** — Products/services & pain relievers/gain creators (the initiatives), and the
     metrics that track them.
   - **Fit** — for each committed expectation: is there an initiative addressing it and a metric showing
     it's met? Mark good fit, gaps (committed but unaddressed/unmeasured), and **deliberate non-fit**
     (expectations the strategy declined — a decision, not a hole).

7. **Cite provenance.** For each fragment, name the source node and give its
   `https://app.cisely.dev/concise/...` link, so the user can click through and nothing in the view can
   silently disagree with the graph.

Note that this is a living view: it re-renders as the model changes. Offer `/cisely:review` for the
whole-model health check, or `/cisely:model` to fill a gap the canvas exposed.
