# Cisely — write down your company's taste, so your AI can build to it

*Taste is the judgment you can recognize but can't easily put into words. Cisely writes down its
roots — the beliefs your company holds, and the choices they justify — so your people and your AI can
apply it without you in the room.*

AI has turned intelligence into a utility — limitless, and the same for everyone. When anyone can
generate anything, generation stops being the scarce half; what's scarce is the judgment to recognize
which output is *right* — what the industry has started calling **taste**. Taste is easy to recognize
and hard to state, and at the level of a company it is rooted in **shared belief**: what a business
holds true is what decides what "good" even looks like.

But taste you can't express is taste your team and your AI can't apply — it stays locked in a founder's
head. **[Cisely](https://cisely.ai) exists to make it concrete.** It doesn't try to bottle the
ineffable judgment itself; it writes down its *roots* — your beliefs, and the choices they justify
(whom to serve, which expectations to meet, which to deliberately decline) — as a shared,
machine-legible **context graph**. An AI dropped into that context stops being a tool waiting for
instructions and becomes a teammate that has inherited what the company is trying to do, for whom, and
why — able to apply your judgment without you in the loop. Cisely keeps a living, continuously-revised
approximation of that judgment; it amplifies the vision you already hold — it doesn't author it for you.

This package is how you build that context with your coding assistant. It teaches the **Cisely agency
model** and drives the **Cisely MCP** to author it with you: the *response* side (mission → strategy →
initiatives → metrics) and the *demand* side (personas → goals; contexts → stakeholders →
expectations), joined by the beliefs both sides root in — the business's shared "mind," written down
once and navigable by every human and agent that works in it.

> Works with **Claude Code, OpenAI Codex, Google Antigravity, Cursor, OpenCode, Aider**, and other
> `AGENTS.md`-aware tools — one payload, thin per-harness adapters. See [`docs/install.md`](docs/install.md).

## The model in brief: demand meets response

Cisely models a business as two sides that meet where value actually changes hands.

- **Demand — what your stakeholders want.** Anything that acts is an *agent*. Its invariant identity
  is a **persona**, which holds latent **goals** (emotional deliverables — to feel secure, in control,
  proud). Viewed through an **operating context** (onboarding, security, billing…), a persona becomes
  a **stakeholder** carrying concrete **expectations** — the only directly *measurable* proxy for the
  goal underneath.
- **Response — what the business does about it.** A **belief** (its mission) roots a **strategy**,
  which chooses *whom to serve* and *which expectations to meet — and which to deliberately decline*.
  Strategy spawns **initiatives** that build **touchpoints**; **metrics** check whether the
  expectations were actually met.

The two sides are one equation — joined at the touchpoint, audited by the metric:

```
  RESPONSE   belief → strategy → initiative → touchpoint
                                                 ↓ MEETS
  DEMAND     belief → goal (latent) → expectation
  AUDIT      metric measures the meeting → evidence reshapes the strategy
```

A business is strongest when the belief rooting its strategy and the belief rooting its customers'
goals are the **same belief** — then serving them stops being a guess about strangers and becomes an
expression of shared conviction. (Author agency once, and Jobs-to-be-Done and a Value Proposition
Canvas fall out as *views* of the same graph — derived, never maintained by hand.)

## Why your AI becomes a 10× teammate with it

An AI agent is itself an agent in this model — a **delegated** one, acting for a principal on
*inherited* goals. **Alignment is precisely the gap between the goals it inherited and the actions it
takes** — and agents drift because that context was never written down. They were handed a task, not
the business.

Lend the agent this context graph and it **discovers the business by traversal** — from a touchpoint
to the expectation it meets, to the stakeholder that holds it, to the persona and its goal, to the
belief, to the strategy that answers. It stops guessing what "good" means and inherits your company's
actual judgment: **alignment implanted by construction, not hoped for.**

That is what turns a capable model into a **10× teammate** — not more raw intelligence, but the
substrate it stands on. A 10× engineer is usually an ordinary engineer on a 10× platform; move them
off it and the multiplier evaporates. The Cisely context graph is that platform for your AI — the
difference between an assistant that executes prompts and a teammate that shares the mission.

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

### 2. Install for your coding assistant

This package works across coding assistants. **In Claude Code:**

```
/plugin marketplace add CiselyAI/agency-skills
/plugin install cisely@cisely-marketplace
```

(`/plugin marketplace add` also accepts a full Git URL if you're not using GitHub.)

Using **Codex, Antigravity, Cursor, OpenCode, Aider,** or another `AGENTS.md`-aware
tool? See [`docs/install.md`](docs/install.md) for the one-per-tool setup. Every
harness reads the same skills; each just needs the **Cisely MCP** wired up (step 3).

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

## Updating

New versions ship as pushes to this repo. How you pull them depends on your harness — there are two
models. (The Cisely MCP is a hosted server, so it updates on its own; you only ever refresh the
skills and adapters.)

**Plugin-managed harnesses** — the tool's plugin manager fetches the new version:

- **Claude Code:**
  ```
  /plugin marketplace update cisely-marketplace
  ```
  This refreshes the catalog; Claude Code picks up the new release. To force it, reinstall with
  `/plugin uninstall cisely` then `/plugin install cisely@cisely-marketplace`. Use `/reload-plugins`
  to apply without restarting.
- **OpenAI Codex:** reinstall **cisely** from the Plugins list (app) or `/plugins` (CLI).
- **Google Antigravity:** re-run `agy plugin install https://github.com/CiselyAI/agency-skills`.

**File-based harnesses** — the tool reads the skills straight from your checkout, so a pull *is* the
update:

```bash
git -C /path/to/agency-skills pull
```

Applies to **Cursor** (`.cursor/` rules), **OpenCode** (cloned repo), **Aider**
(`.aider.conf.yml`), and any other `AGENTS.md`-aware tool. Restart the tool (or start a new session)
so it re-reads the files. See [`docs/install.md`](docs/install.md) for per-tool specifics.

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

## License

Licensed under the [Apache License 2.0](LICENSE). Copyright 2026 Bhumika AI Pte Ltd.
