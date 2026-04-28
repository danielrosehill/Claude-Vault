---
name: activate-bundle
description: Apply a named bundle from the Claude Vault inventory — activates all plugins and MCP servers the bundle declares in the current project. Use when the user names a bundle ("activate the podcast bundle").
---

# activate-bundle

**Status: stub.** Not yet implemented.

Intended behaviour: read `bundles[name]` from the inventory (path resolved per `skills/_shared/inventory-path.md`), then loop over its `plugins` and `mcpServers` arrays, calling the same logic as `activate-plugin` and `activate-mcp` for each. Report the aggregate result and a single restart reminder.

For now, run `activate-plugin` / `activate-mcp` individually.
