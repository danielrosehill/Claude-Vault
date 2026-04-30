---
name: activate-bundle
description: Apply a named bundle from the Claude Vault inventory — activates all plugins and MCP servers the bundle declares in the current project. Use when the user names a bundle ("activate the podcast bundle").
---

# activate-bundle

Apply a bundle: a named set of plugins + MCP servers defined in the inventory. Equivalent to running `activate-plugin` and `activate-mcp` for each member, but in one shot.

## Inputs

The user names a bundle (e.g. "activate the podcast bundle"). If ambiguous, run `list-vault` filtered to bundles.

## Steps

1. Resolve and read the inventory per `skills/_shared/inventory-path.md`. Find `bundles[]` entry by name. If not found, tell the user and stop — suggest `list-vault`.
2. Determine project root (cwd or nearest `.git`/`.claude` ancestor).
3. **Validate before mutating**: for each entry in the bundle's `plugins` and `mcpServers`, confirm the referenced item still exists in the inventory's top-level `plugins` / `mcpServers` arrays. Collect missing references.
4. If any references are missing, surface them and ask whether to proceed with the rest or abort. Do not silently skip.
5. For each plugin in the bundle: apply the same logic as `activate-plugin` (set `enabledPlugins["<name>@<marketplace>"] = true` in `.claude/settings.json`). Skip ones already active, count them.
6. For each MCP server in the bundle: apply the same logic as `activate-mcp` (write to `.mcp.json`, add to `enabledMcpjsonServers`). Skip ones already mounted with matching config; ask before overwriting on config mismatch.
7. Aggregate any env vars referenced by activated MCP configs (`${VAR}` placeholders) and list them once at the end.
8. Single restart reminder for the whole batch.

## Safety

- Project-scoped only.
- Atomic-ish: if step 3 finds missing references and the user aborts, write nothing.
- Never apply a bundle to user-global settings.

## Output style

Per-member status line (`activated` / `already active` / `skipped`). Totals line. Env-vars-needed line if applicable. One restart reminder.
