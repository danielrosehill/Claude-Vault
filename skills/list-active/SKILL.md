---
name: list-active
description: Show which vault plugins and MCP servers are currently active in the current project. Use when the user asks "what's enabled here", "what's active in this project", or wants to audit the project's tool surface before activating more.
---

# list-active

Inspect the current project's Claude Code configuration and report what's enabled.

## Steps

1. Determine the project root — the current working directory, or walk up to the nearest directory containing `.claude/` or `.git/`.
2. Read these files if they exist (treat missing as empty):
   - `<project>/.claude/settings.json` — look at `enabledPlugins` (object: `"name@marketplace": true`) and `enabledMcpjsonServers` (array of MCP server names from `.mcp.json`).
   - `<project>/.claude/settings.local.json` — same shape, may override.
   - `<project>/.mcp.json` — `mcpServers` object (name → config).
3. Cross-reference with `~/.claude/vault/inventory.json` to label each active item as **vault** (in inventory) or **external** (not in inventory).
4. Render:
   - **Active plugins** — name, marketplace, source (vault/external)
   - **Active MCP servers** — name, source (vault/external), enabled-state (in `.mcp.json` AND in `enabledMcpjsonServers`, or only in `.mcp.json`)
   - If nothing is active, say so explicitly — don't render empty headings.
5. Note any settings.local.json overrides separately so the user sees them.

## Output style

Compact. One line per entry. Mark vault items with `[vault]`, external with `[ext]`. End with totals.
