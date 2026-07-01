# Install — Cisely (Agency Model) across coding assistants

This package is one payload (`skills/` + slash `commands/`) with thin per-harness
adapters. **Install it once per tool you use**, then connect the **Cisely MCP**
(`https://app.cisely.dev/mcp`) — the MCP is the source of truth; nothing authors
without it.

> **Before you start:** create/join your workspace at
> [app.cisely.dev](https://app.cisely.dev). Reading the model needs any member;
> **authoring** needs **tenant-admin** rights.

## At a glance

| Harness | Skills | Slash commands | MCP wiring | Adapter file |
|---------|:------:|:--------------:|------------|--------------|
| **Claude Code** | ✅ | ✅ `/cisely:*` | plugin manifest (native `http`) | `.claude-plugin/` |
| **OpenAI Codex** | ✅ | ➖ (ask in prose) | plugin `mcpServers` + `config.toml` | `.codex-plugin/plugin.json` |
| **Google Antigravity** | ✅ | ➖ | `agy plugin install` (auth on install) | `.agents/plugins/marketplace.json` |
| **Cursor** | rule | ➖ | `.cursor/mcp.json` | `.cursor/rules/*.mdc` |
| **OpenCode** | ✅ | ➖ | `opencode.json` `mcp` | `.opencode/INSTALL.md` |
| **Aider** | read-only context | ➖ | ✖ (no MCP — draft only) | `.aider.conf.yml` |
| **Any `AGENTS.md` tool** | read-only context | ➖ | portable `.mcp.json` | `AGENTS.md` + `.mcp.json` |

➖ = no slash-command system; use the plain-language intents in
[`AGENTS.md`](../AGENTS.md). ✖ = the tool can't call MCP; it drafts, you apply
elsewhere.

---

## Claude Code

```
/plugin marketplace add CiselyAI/agency-skills
/plugin install cisely@cisely-marketplace
```

Then connect the MCP: `/mcp` → **cisely** → OAuth sign-in. Use `/cisely:model`,
`/cisely:review`, `/cisely:canvas`.

## OpenAI Codex

**App:** Plugins sidebar → find **Cisely** → install. **CLI:** `/plugins` →
select **cisely** → Install. The plugin ships `skills/` and an `mcpServers` entry.

If your Codex build manages MCP through `~/.codex/config.toml`, add:

```toml
[mcp_servers.cisely]
command = "npx"
args = ["-y", "mcp-remote", "https://app.cisely.dev/mcp"]
```

Then just ask: *"Let's model my company in Cisely."*

## Google Antigravity

```
agy plugin install https://github.com/CiselyAI/agency-skills
```

Authentication is requested on install (`ON_INSTALL`) — complete the Cisely OAuth
flow so the `cisely` MCP tools are available.

## Cursor

Cursor reads the project rule and MCP config directly from this repo (open it, or
copy the files into your project):

- `.cursor/rules/cisely-agency-model.mdc` — triggers the modeling discipline.
- `.cursor/mcp.json` — registers the `cisely` server. Enable it in
  **Settings → MCP** and complete OAuth.

Then ask in Composer/Chat: *"Model our personas and their goals in Cisely."*

## OpenCode

See [`.opencode/INSTALL.md`](../.opencode/INSTALL.md) — add the `cisely` server to
`opencode.json` (`type: remote`, `url: https://app.cisely.dev/mcp`), open the repo
so `AGENTS.md` loads, and ask in prose.

## Aider

```
aider   # picks up .aider.conf.yml automatically
```

`.aider.conf.yml` loads the modeling skill as read-only context. **Aider can't
call the Cisely MCP**, so use it to *draft/think through* the model, then author
via a MCP-capable harness or in the web app.

## Any other AGENTS.md-aware tool (Zed, Jules, Windsurf, …)

Point the tool at this repo. It will read [`AGENTS.md`](../AGENTS.md) for the
skills index and intents, and the portable [`.mcp.json`](../.mcp.json) (stdio via
`mcp-remote`) for the server. If the tool has its own MCP config format, register:

```json
{ "command": "npx", "args": ["-y", "mcp-remote", "https://app.cisely.dev/mcp"] }
```

---

## Verifying the MCP is live

In any harness, ask the agent to **"list the cisely tools."** You should see
`CreatePersona`, `SituateStakeholder`, `CreateStrategy`, and the rest. No tools =
the MCP isn't connected; fix that before modeling. Everything you author appears
in the web app (**Agency Board** for demand, **Purpose Board** for response) at
[app.cisely.dev](https://app.cisely.dev).
