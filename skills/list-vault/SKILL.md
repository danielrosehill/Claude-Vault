---
name: list-vault
description: List plugins, MCP servers, and bundles available in the user's Claude Vault inventory. Use when the user asks what's in their vault, what they could activate, or "show me my vault".
---

# list-vault

Read the vault inventory and present its contents grouped by category.

## Steps

1. Read `~/.claude/vault/inventory.json`. If it doesn't exist, tell the user to create it (point at the README format) and stop.
2. Parse the JSON. Expected top-level keys: `plugins` (array), `mcpServers` (array), `bundles` (array). Any may be missing — treat as empty.
3. Render a compact summary:
   - **Plugins** — name, marketplace, description, tags
   - **MCP Servers** — name, description, tags
   - **Bundles** — name, description, what it activates
4. If the user asked for a filter (by tag, by name pattern, by category), apply it before rendering.

## Output style

Group by category with a heading per group. One line per entry. Skip empty groups. End with totals: `N plugins, M MCP servers, K bundles`.

Do not echo the raw JSON unless the user asks.
