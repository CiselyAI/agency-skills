# Contributing to the Cisely Agency-Model plugin

Thanks for your interest in improving this plugin! It helps people model their company in
[Cisely](https://cisely.ai) over the Cisely MCP. Contributions of all sizes are welcome — from fixing
a typo in a skill to sharpening the modeling playbook.

## Reporting issues & ideas

- **Bugs / problems** → open an [issue](https://github.com/CiselyAI/agency-skills/issues). Please
  include: what you ran (the command or what you asked), what you expected, what happened, and your
  Claude Code version (`claude --version`). If it involves the MCP, note whether `/mcp` shows **cisely**
  connected.
- **Questions, ideas, show-and-tell** → start a
  [discussion](https://github.com/CiselyAI/agency-skills/discussions).
- **Security issues** → please do **not** open a public issue. Email security@cisely.ai.

> Note: this repo is about the *plugin* (the skills and commands). Issues with the Cisely product
> itself are best filed in-app at [app.cisely.dev](https://app.cisely.dev).

## What's in here

```
skills/                the portable payload (Markdown + YAML frontmatter)
  cisely-agency-model/ the worldview, modeling discipline, tool playbook (+ references/)
  cisely-sub-strategy/ authoring custom strategy kinds (runtime business objects)
commands/              Claude Code slash commands → /cisely:model, /cisely:review, /cisely:canvas
AGENTS.md              cross-harness entrypoint (Codex, Cursor, OpenCode, generic AGENTS.md tools)
.mcp.json              portable MCP manifest (cisely server via the mcp-remote stdio bridge)

.claude-plugin/        Claude Code:  plugin.json + marketplace.json (self-hosted marketplace)
.codex-plugin/         OpenAI Codex: plugin.json (skills + mcpServers + interface)
.agents/plugins/       Google Antigravity: marketplace.json
.cursor/               Cursor: rules/*.mdc + mcp.json
.opencode/             OpenCode: INSTALL.md
.aider.conf.yml        Aider: read-only skill context (no MCP — draft only)
docs/                  install.md (per-harness install matrix)
```

Skills and commands are **Markdown with YAML frontmatter** — no build step. The deeper material lives
in `skills/cisely-agency-model/references/*.md`, loaded on demand. The **portable payload** is
`skills/`; every per-harness directory is a thin adapter that points back at it — keep the skills
harness-agnostic and let the adapters carry tool-specific wiring.

## Develop & test locally

1. Clone your fork.
2. In a Claude Code session, add the local checkout as a marketplace and install it:
   ```
   /plugin marketplace add /absolute/path/to/agency-skills
   /plugin install cisely@cisely-marketplace
   ```
3. Connect the MCP (`/mcp` → **cisely** → OAuth) and exercise the change — run `/cisely:model`,
   `/cisely:review`, or `/cisely:canvas` and confirm the skill triggers and behaves as intended.
4. Re-add the marketplace after edits to pick up changes.

Validate the manifests parse before committing:
```
python3 -c "import json; json.load(open('.claude-plugin/plugin.json')); json.load(open('.claude-plugin/marketplace.json'))"
```

## Style for skills & commands

- Write for the **reading model**: clear, specific, and oriented to *when* to use a thing.
- Match the existing **voice** — warm, precise, grounded in the agency model. Don't restate generic
  LLM advice.
- Keep `SKILL.md` focused; push depth into `references/` and link to it (progressive disclosure).
- A skill's `description` is its trigger — make it concretely match the user phrasings it should fire on.
- Refer to MCP tools by name (e.g. `SituateStakeholder`); the tools' own descriptions carry the
  detailed mechanics, so don't duplicate them.

## Pull requests

1. Branch from `main` (e.g. `feat/clearer-canvas`, `fix/model-command-typo`).
2. Use **[Conventional Commits](https://www.conventionalcommits.org/)** for messages —
   `feat(...)`, `fix(...)`, `docs(...)`, `chore(...)`.
3. Keep PRs focused; describe the change and how you tested it.
4. `main` is protected — changes land via PR.

## License

By contributing, you agree that your contributions are licensed under the
[Apache License 2.0](LICENSE), consistent with the rest of this project.
