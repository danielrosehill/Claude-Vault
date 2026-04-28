---
name: list-vault
description: List plugins, MCP servers, and bundles available in the user's Claude Vault inventory. Use when the user asks what's in their vault, what they could activate, or "show me my vault".
---

# list-vault

Read the vault inventory and present its contents grouped by category.

## Steps

1. Resolve the inventory path per `skills/_shared/inventory-path.md` (the canonical resolver: `$CLAUDE_USER_DATA` → `$XDG_DATA_HOME/claude-plugins` → `~/.local/share/claude-plugins`, then `/claude-vault/inventory.json`). Read it. If it doesn't exist, tell the user the resolved path and point at the README format, then stop. Do **not** fall back to `~/.claude/`.
2. Parse the JSON. Expected top-level keys: `plugins` (array), `mcpServers` (array), `bundles` (array). Any may be missing — treat as empty.
3. Render a compact summary:
   - **Plugins** — name, marketplace, description, tags
   - **MCP Servers** — name, description, tags
   - **Bundles** — name, description, what it activates
4. If the user asked for a filter (by tag, by name pattern, by category), apply it before rendering.

## Output style

Group by category with a heading per group. One line per entry. Skip empty groups. End with totals: `N plugins, M MCP servers, K bundles`.

Do not echo the raw JSON unless the user asks.
