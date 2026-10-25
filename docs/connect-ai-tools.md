# Connecting clients from any AI tool

The data side never changes: one **remote MCP server** in front of the governed
metrics views, with OAuth so each user signs in with their own analytics login.
What changes per tool is only **where the MCP URL goes** and **where the skill goes**.

| Tool | Add the MCP server | Load the skill |
|---|---|---|
| **Claude** | Settings → Connectors → add custom connector (remote MCP URL) | Upload the skill folder as a skill (or paste `SKILL.md` into a Project's instructions) |
| **ChatGPT** | Settings → Apps / Connectors → add a custom MCP connector (developer mode, plan-dependent) | Project instructions or a custom GPT's instructions, with `references/` as files |
| **Gemini** | Gemini CLI: add the server under `mcpServers` in `settings.json` | Put `SKILL.md` in the `GEMINI.md` context file |
| **Grok** | xAI API: attach the server as a remote MCP tool | Send `SKILL.md` as the system prompt |
| **Any MCP client** | point it at the server URL | system prompt / instructions + reference files |

> Menus and plan requirements change often; check each vendor's current docs.

## What the client sees
1. Open their usual AI assistant.
2. Sign in once to the analytics connector (OAuth).
3. Ask: *"Net sales by branch last week vs the week before."*
4. Get the same governed numbers, in the same format, in every tool.

## Why this is simpler for clients
- No new dashboard tool to learn: they stay in the assistant they already use.
- No exports or spreadsheets to request from the data team.
- One login, one set of permissions, enforced on every query.
- The same skill file works everywhere, so answers are consistent across tools.

## Onboarding checklist (data team)
- [ ] Client's data flows into the standard models and metrics views
- [ ] User accounts created with the right row-level security attributes
- [ ] MCP server URL + OAuth shared with the client
- [ ] Skill files shared (or pre-installed for their workspace)
- [ ] Three smoke-test questions answered and reconciled with the POS
