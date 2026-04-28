---
name: activate-mcp
description: Mount an MCP server from the Claude Vault inventory in the current project by writing to .mcp.json and enabling it in .claude/settings.json. Use when the user wants to "activate", "mount", or "add" a vault MCP server to the current repo only.
---

# activate-mcp

Mount a vault MCP server in the current project.

## Inputs

The user names an MCP server (e.g. "mount the greeninvoice MCP"). If ambiguous, run `list-vault` first or ask.

## Steps

1. Read `~/.claude/vault/inventory.json`. Find the MCP entry under `mcpServers` by name. If not found, tell the user and stop.
2. The inventory entry's `config` object is the canonical server spec (`command`, `args`, `env`, etc.).
3. Determine project root (cwd or nearest `.git`/`.claude` ancestor).
4. Read `<project>/.mcp.json`. If it doesn't exist, start with `{ "mcpServers": {} }`.
5. Set `mcpServers["<name>"] = <inventory.config>`. If the server is already present with a different config, ask the user before overwriting.
6. Write `.mcp.json` back with 2-space indentation.
7. Read `<project>/.claude/settings.json` (create if missing). Ensure `enabledMcpjsonServers` exists as an array. Add `<name>` if not already present.
8. Write settings back.
9. Confirm: server name, files edited, and the reminder that **Claude Code must be restarted for the MCP to mount**. If the config references env vars (e.g. `${FOO}`), list them so the user knows to ensure they're set.

## Safety

- Project-scoped only. Never touch user-global config.
- Do not paste secrets inline. If the inventory uses `${VAR}` placeholders, preserve them — do not resolve.
- If the server requires auth and no env var is present, warn but still write the config.

## Output style

One-line confirmation, list of edited files, and the env-vars-needed line if applicable. Single restart reminder.
