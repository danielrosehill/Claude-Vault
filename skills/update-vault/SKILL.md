---
name: update-vault
description: Pull updates across all plugins and MCP servers tracked in the Claude Vault inventory. Use when the user asks to "update the vault" or "refresh my plugins".
---

# update-vault

**Status: stub.** Not yet implemented.

Intended behaviour: enumerate plugin sources from configured marketplaces and run `git pull` (or the marketplace's update mechanism) on each; for MCP servers installed from package registries, surface available updates without applying them automatically.
