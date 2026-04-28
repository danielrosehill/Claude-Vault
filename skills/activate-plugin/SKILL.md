---
name: activate-plugin
description: Enable a plugin from the Claude Vault inventory in the current project by editing the project's .claude/settings.json. Use when the user wants to "activate", "turn on", or "enable" a vault plugin for the current repo only.
---

# activate-plugin

Enable a vault plugin in the current project's `.claude/settings.json`.

## Inputs

The user names a plugin (e.g. "activate the podcast-production plugin"). If ambiguous, run `list-vault` first or ask.

## Steps

1. Resolve the inventory path per `skills/_shared/inventory-path.md` and read it. Find the plugin entry by name. If not found, tell the user and stop — suggest `list-vault`.
2. Resolve the activation key: `<plugin-name>@<marketplace-name>` from the inventory entry.
3. Determine project root (cwd or nearest `.git`/`.claude` ancestor).
4. Read `<project>/.claude/settings.json`. If it doesn't exist, create the directory and start with `{}`.
5. Ensure `enabledPlugins` exists as an object. Set `enabledPlugins["<name>@<marketplace>"] = true`.
6. Write the file back with 2-space indentation, preserving any other keys.
7. Confirm to the user: plugin name, marketplace, settings file path edited, and the reminder that **Claude Code must be restarted in this project for the activation to take effect**.

## Safety

- Never edit `~/.claude/settings.json` (user-global). This skill is project-scoped only.
- If `.claude/settings.json` already has the plugin enabled, say so and do not rewrite the file.
- If the project has a `settings.local.json` that disables the same plugin, surface the conflict — don't silently override.

## Output style

One-line confirmation. Then a single reminder line about restarting Claude Code.
