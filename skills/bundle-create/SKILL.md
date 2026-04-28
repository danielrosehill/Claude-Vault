---
name: bundle-create
description: Save the current project's active plugin and MCP set as a reusable bundle in the Claude Vault inventory. Use when the user has a project configured the way they like and wants to capture that combination for reuse.
---

# bundle-create

**Status: stub.** Not yet implemented.

Intended behaviour: read the current project's `enabledPlugins` and `.mcp.json`, ask the user for a bundle name + description, and append a new entry to `bundles[]` in `~/.claude/vault/inventory.json`. Reject if a bundle with that name already exists unless `--overwrite` is passed.
