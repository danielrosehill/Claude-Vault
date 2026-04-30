---
name: deactivate-mcp
description: Remove an MCP server from the current project's .mcp.json and disable it in .claude/settings.json. Symmetric counterpart to activate-mcp.
---

# deactivate-mcp

Unmount an MCP server from the current project. Removes it from `.mcp.json` and from `enabledMcpjsonServers` in `.claude/settings.json`.

## Inputs

The user names an MCP server. If ambiguous, run `list-active`.

## Steps

1. Determine project root (cwd or nearest `.git`/`.claude` ancestor).
2. Read `<project>/.mcp.json`. If missing or the server isn't present, say so and stop.
3. Delete the entry under `mcpServers["<name>"]`. Write the file back with 2-space indentation. If `mcpServers` becomes empty, leave it as `{}`.
4. Read `<project>/.claude/settings.json`. If `enabledMcpjsonServers` contains the name, remove it. Write back.
5. Confirm: server name, files edited, restart reminder.

## Safety

- Project-scoped only.
- Do not delete the user's `.mcp.json` file even if it ends up empty — leave the empty `{ "mcpServers": {} }` shell.
- Surface any `settings.local.json` overrides separately.

## Output style

One-line confirmation. Restart reminder.
