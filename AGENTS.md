# AGENTS.md — model your company in Cisely

This repository is a **portable skill package** that teaches an AI coding agent to
**model a company as a living context graph in [Cisely](https://cisely.ai)** by
driving the **Cisely MCP** server. It ships as a Claude Code plugin *and* as
plain, harness-agnostic `SKILL.md` skills that Codex, Antigravity, Cursor,
OpenCode, Aider, and any other `AGENTS.md`-aware agent can read.

If you are an agent and the user asks to model, populate, or review their company,
personas, strategy, goals, or value proposition in Cisely — **read the relevant
skill below before acting**, and make sure the **Cisely MCP is connected** first.

## The one hard dependency: the Cisely MCP

Everything here works by calling tools on the **`cisely`** MCP server at
`https://app.cisely.dev/mcp` (authenticated over OAuth). Authoring is impossible
without it. Confirm it is connected before you start:

- **Claude Code** — the plugin registers it; run `/mcp`, pick **cisely**, sign in.
- **Every other harness** — see [`docs/install.md`](docs/install.md) for the
  one-line MCP config for your tool. The portable [`.mcp.json`](.mcp.json) at the
  repo root works for any tool that reads it (via the `mcp-remote` stdio bridge).

If no `cisely` tools are available in your session, **stop and tell the user to
connect the MCP** — do not invent business data or write to files as a substitute.

## The skills (read the one that matches the task)

| Skill | When to read it |
|-------|-----------------|
| [`cisely-agency-model`](skills/cisely-agency-model/SKILL.md) | The core. Modeling a company's context graph — the *response* side (mission, beliefs, strategy, initiatives, metrics) and the *demand* side (personas, operating contexts, stakeholders, goals, expectations). The worldview, the build order, and the MCP tool playbook. Read this whenever the user wants to model, build, populate, or refine their company, personas, strategy, goals, or value proposition. |
| [`cisely-sub-strategy`](skills/cisely-sub-strategy/SKILL.md) | Authoring your own **strategy kinds** (a technology strategy, a GTM strategy, a people-&-culture strategy): define a company-wide schema, publish it, then fill in records nested under a core strategy. |

Deeper material lives under
[`skills/cisely-agency-model/references/`](skills/cisely-agency-model/references/)
(the worldview in `agency-model.md`, the tool playbook, the synthesis method, and
the MCP tool map) — load it on demand, as the `SKILL.md` points you to it.

## Commands → natural-language intents

Claude Code exposes three slash commands. On harnesses without slash commands,
**the user just asks for the same thing in plain language** and you follow the
`cisely-agency-model` skill:

| Claude command | Plain-language intent (any harness) |
|----------------|-------------------------------------|
| `/cisely:model` | "Start a guided session to model my company in Cisely" (interview, or ingest a doc/URL). |
| `/cisely:review` | "Review my Cisely model — coverage, gaps, lifecycle drafts, belief-alignment." Read-only. |
| `/cisely:canvas` | "Synthesize Jobs-to-be-Done and a Value Proposition Canvas for `<persona>`." Read-only view. |

## Operating principles (they are binding — from the skill)

1. **The MCP is the source of truth**, not files. You author *through* the tools;
   everything you create is visible in the web app at `https://app.cisely.dev`.
2. **High rigor.** Every node moves `Draft → Revise → Activate`. A *declined*
   expectation or persona is a deliberate decision, not an omission.
3. **Refer to MCP tools by name** (e.g. `SituateStakeholder`, `CreatePersona`);
   the tools' own descriptions carry the mechanics — don't duplicate them.
4. **The payoff to steer toward:** a business whose strategy roots in the *same
   beliefs* its stakeholders hold. Coverage without alignment is not done.

## Install & platform notes

See [`docs/install.md`](docs/install.md) for per-harness setup (Claude Code,
Codex, Antigravity, Cursor, OpenCode, Aider, and the generic path). To report
issues, open one at https://github.com/CiselyAI/agency-skills/issues; product
issues are best filed in-app at https://app.cisely.dev.
