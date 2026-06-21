# Cisely — model your company as a living context graph

A Claude Code plugin that helps you **model your company in [Cisely](https://cisely.ai)** —
not as a pile of docs and decks, but as a navigable graph an AI agent (and your whole team) can
traverse to understand the business.

It teaches Claude the **Cisely agency model** and drives the **Cisely MCP** to build it with you:
the *response* side (mission → strategy → initiatives → metrics) and the *demand* side (personas →
goals; contexts → stakeholders → expectations), joined by the beliefs both sides root in.

## What you get

- **Skill `cisely-agency-model`** — the worldview, the modeling discipline, and the playbook for
  wielding the Cisely MCP tools in the right order. Claude uses it automatically when you talk about
  modeling your company, personas, strategy, goals, or value proposition.
- **Skill `cisely-sub-strategy`** — how to define your own strategy *kinds* (a technology strategy, a
  GTM strategy, a people-&-culture strategy): author a company-wide schema, then fill in records.
- **Commands:**
  - `/cisely:model` — start a guided modeling session (interview, or ingest from your own docs).
  - `/cisely:review` — traverse the graph and report coverage, gaps, and belief-alignment.
  - `/cisely:canvas` — synthesize Jobs-to-be-Done and a Value Proposition Canvas from the graph.

## Getting started

### 1. Create your Cisely account

Sign up at **[app.cisely.dev](https://app.cisely.dev)** and create (or join) your company's
workspace. To **author** model content you need **tenant-admin** rights; any member can read. This is
also where you'll view everything you build — the **Agency Board** (demand side) and the **Purpose
Board** (response side).

### 2. Install the plugin

From your Claude Code session:

```
/plugin marketplace add CiselyAI/agency-skills
/plugin install cisely@cisely-marketplace
```

(`/plugin marketplace add` also accepts a full Git URL if you're not using GitHub.)

### 3. Connect the Cisely MCP

The plugin registers the remote MCP server at `https://app.cisely.dev/mcp`. Run `/mcp`, select
**cisely**, and complete the OAuth sign-in in your browser using the account from step 1.

> Prefer to set it up manually? `claude mcp add --transport http cisely https://app.cisely.dev/mcp`

### 4. Start modeling

```
/cisely:model
```

Then `/cisely:review` to check coverage and belief-alignment, and `/cisely:canvas` to synthesize
Jobs-to-be-Done and a Value Proposition Canvas for any persona.

## How it works

Everything you create via the MCP is visible in the Cisely web app at `https://app.cisely.dev`
(the **Agency Board** for the demand side, the **Purpose Board** for the response side). After each
change, the plugin gives you a direct link to view it.

The plugin is **high-rigor** by design: it coaches the model's discipline (every node goes
`Draft → Revise → Activate`), treats a *declined* expectation or persona as a deliberate decision,
and pushes toward the model's core payoff — a business whose strategy roots in the **same beliefs**
its stakeholders hold.

## Learn the model

The agency model is summarized in
[`skills/cisely-agency-model/references/agency-model.md`](skills/cisely-agency-model/references/agency-model.md).
