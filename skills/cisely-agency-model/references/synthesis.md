# Synthesis — Jobs-to-be-Done & the Value Proposition Canvas

> These are **views derived from the graph**, never authored or maintained by hand. You read them off
> by traversing what's already modeled. If a view looks thin, the fix is to model more (or move an
> expectation up the articulation gradient), not to write the view by hand.

## Jobs-to-be-Done

A JTBD **job story** maps one-to-one onto the agency primitives:

| Job story fragment | Agency primitive | Read it from |
|---|---|---|
| *(the implicit "I")* | **Persona** | the job's executor |
| **When** [situation] | **Operating Context** | the lens the stakeholder is situated in |
| **I want to** [motivation] | **Expectation** | the contextual proxy |
| **so I can** [outcome] | **Goal** | the emotional deliverable it proxies |

**To synthesize jobs for a persona or stakeholder:**
1. `GetPersona` / `ListStakeholders` for the persona — find each stakeholder (its operating contexts).
2. For each stakeholder, list its **expectations** (`ListExpectations`, filter to the stakeholder).
3. For each expectation, follow `PROXIES_FOR` to its **goal**.
4. Render: **"When** `<operating context>`**, I want to** `<expectation statement>`**, so I can**
   `<goal statement>`**."** (executed by the persona).
5. **Quality note:** a *latent* or *emerging* expectation can't yet be stated as a crisp job outcome —
   say so, and treat sharpening it as the next discovery. A *defined* expectation yields a clean job.

## Value Proposition Canvas

The canvas is the two sides of the model laid side by side, for a chosen **persona / stakeholder**.

**Customer profile (demand side):**
- **Jobs** ← the persona's **goals** (and the jobs synthesized above).
- **Pains** ← its **expectations with PAIN valence** (fears of going unmet).
- **Gains** ← its **expectations with GAIN valence** (hopes).

**Value map (response side):**
- **Products & services** ← the **touchpoints** the business offers (in this model, surfaced via the
  **initiatives** and the strategy serving this persona/context).
- **Pain relievers / gain creators** ← the **initiatives** (and the metrics tracking the expectations
  they target).

**Fit** ← the **`MEASURES` / meeting relationship evaluated**: for each committed expectation, is there
an initiative addressing it and a metric showing it's being met? Walk:
`Expectation ← MEASURES ← Metric` and `Expectation`'s strategy `← EXECUTES ← Initiative`.

**To chart a canvas:**
1. Pick a persona (and optionally a specific operating context / stakeholder).
2. Project its goals + valence-split expectations as the **profile**.
3. Project the initiatives and metrics tied (through strategy / measurement) to those expectations as
   the **value map**.
4. Evaluate **fit**: which expectations have a reliever + a measure (good fit), which are committed but
   unaddressed (a gap), and which are **declined by strategy** (a deliberate non-fit — note it as a
   decision, not a hole).

## Reporting both

Present synthesized jobs and canvases as **read-only artifacts** with their provenance — name the
nodes each fragment came from, so the user can click through (use the **View in Cisely** links) and so
nothing in the view can silently disagree with the graph. One graph yields as many canvases as there
are segments; they re-render whenever the model changes.
