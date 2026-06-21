---
description: Start a guided session to model your company in Cisely — interview-style, or by ingesting your own docs. Builds the agency graph over the Cisely MCP.
argument-hint: "[focus area, or a path/URL to ingest]"
---

Begin a **Cisely modeling session**. Follow the `cisely-agency-model` skill throughout — its worldview,
build order, and the high-rigor operating principles are binding here.

User input (optional focus or source to ingest): **$ARGUMENTS**

Work through these steps:

1. **Verify the connection.** Confirm the Cisely MCP tools (`mcp__cisely__*`) are available. If they
   are not, stop and tell the user to run `/mcp` → select **cisely** → complete the OAuth sign-in (or
   `claude mcp add --transport http cisely https://app.cisely.dev/mcp`). Do not pretend to write data.

2. **Take stock of what exists.** Before proposing anything, read the current model: `GetMission`,
   `ListBeliefs`, `ListPersonas`, `ListOperatingContexts`, `ListStakeholders`, `ListGoals`,
   `ListExpectations`, `ListStrategies`, `ListInitiatives`, `ListMetrics`. Give the user a short,
   honest summary of where the model stands today and the biggest gaps (e.g. "no mission yet",
   "personas exist but none have goals", "strategies don't root in any belief").

3. **Choose a mode** (unless `$ARGUMENTS` already implies one — a file/URL ⇒ ingest; a topic ⇒ focus):
   - **Interview** — elicit the model Socratically (see the skill's playbook reference). Start where it
     pays most: usually the mission, or the single most important persona and what it *really* wants.
   - **Ingest** — read the material the user pointed at, extract candidate personas / goals / contexts /
     expectations / strategy, and **present the proposed graph** for approval before writing.

4. **Model, with discipline.** Respect the build order (mission → beliefs → demand side → response
   side). For each concept: **propose it, confirm with the user, then** `Create → Revise → Activate`.
   Wire the edges (born-linked ones happen automatically; author `ROOTED_IN`, `PROXIES_FOR`, and
   `MEASURES` yourself). Actively look for goals and strategies that should root in the **same belief**
   — that convergence is the alignment payoff.

5. **Show the work.** After each change, give the user the **View in Cisely** deep link the tool
   returns, and periodically point at the **Agency Board** (`https://app.cisely.dev/concise/agency`)
   and **Purpose Board** (`https://app.cisely.dev/concise/purpose`).

6. **Close the loop.** When a coherent slice exists, offer to run `/cisely:review` (coverage, gaps,
   alignment) or `/cisely:canvas` (Jobs-to-be-Done + Value Proposition Canvas).

Keep it conversational and incremental. Don't bulk-create without the user seeing the shape, and treat
every "we don't serve that" as a deliberate decision worth recording, not an omission.
