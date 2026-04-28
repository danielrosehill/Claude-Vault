---
name: setup-workspace
description: Bootstrap a new project's Claude Code configuration by activating a chosen template or set of vault plugins and MCP servers. Use when the user is starting a new project and wants the right tools mounted from the start.
---

# setup-workspace

**Status: stub.** Not yet implemented.

Intended behaviour: ask the user what kind of project this is (or accept a template name), look up the matching bundle in the vault, then dispatch to `activate-bundle` or to `activate-plugin` / `activate-mcp` for each component.

For now, point the user at `activate-plugin` and `activate-mcp` for manual activation, or `activate-bundle` once that's implemented.
