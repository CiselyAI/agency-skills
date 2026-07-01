# Installing Cisely (Agency Model) for OpenCode

[OpenCode](https://opencode.ai) reads `AGENTS.md`, discovers Agent Skills, and
speaks MCP natively — so it can use this package directly. Two things to wire up:
the **skills** and the **Cisely MCP server**.

## 1. Get the files

Clone the repo somewhere OpenCode can see it (a project you open, or a shared path):

```bash
git clone https://github.com/CiselyAI/agency-skills.git
```

Open it as (or alongside) your project. OpenCode picks up the root
[`AGENTS.md`](../AGENTS.md), which indexes the skills and the modeling discipline.

## 2. Register the Cisely MCP

Add the server to your `opencode.json` (global `~/.config/opencode/opencode.json`
or project-level). The Cisely server is remote HTTP with OAuth:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "cisely": {
      "type": "remote",
      "url": "https://app.cisely.dev/mcp",
      "enabled": true
    }
  }
}
```

Restart OpenCode. On first use it opens the browser OAuth flow — sign in with the
account from your Cisely workspace ([app.cisely.dev](https://app.cisely.dev);
authoring needs tenant-admin rights).

> If your OpenCode build doesn't support remote MCP directly, use the stdio
> bridge instead — the same one the portable [`.mcp.json`](../.mcp.json) uses:
> `{ "type": "local", "command": ["npx", "-y", "mcp-remote", "https://app.cisely.dev/mcp"] }`.

## 3. Use it

Ask in plain language — there are no slash commands in OpenCode, but the intents
from [`AGENTS.md`](../AGENTS.md) all work:

```
Let's model my company in Cisely.
Review my Cisely model for gaps and belief-alignment.
Synthesize a Value Proposition Canvas for our enterprise buyer.
```

Verify the MCP is live by asking OpenCode to "list the cisely tools" — you should
see `CreatePersona`, `SituateStakeholder`, and the rest.

## Troubleshooting

- **No `cisely` tools?** Check the `mcp` block in `opencode.json` and that OpenCode
  finished the OAuth sign-in. Skills can describe the model but cannot author
  without the MCP.
- **Skill not triggering?** Confirm OpenCode loaded `AGENTS.md` / the `skills/`
  directory for this project; ask it to "read the cisely-agency-model skill".
