---
name: deactivate-plugin
description: Disable a plugin in the current project by removing its entry from .claude/settings.json enabledPlugins. Symmetric counterpart to activate-plugin.
---

# deactivate-plugin

Remove a plugin's activation from the current project's `.claude/settings.json`. The plugin stays installed — only the project-level enable flag is removed.

## Inputs

The user names a plugin. If ambiguous, run `list-active` first.

## Steps

1. Determine project root (cwd or nearest `.git`/`.claude` ancestor).
2. Read `<project>/.claude/settings.json`. If missing or `enabledPlugins` is empty, tell the user nothing's active and stop.
3. Find the matching key in `enabledPlugins`. The user may give just the plugin name (e.g. `podcast-production`) or the full activation key (`podcast-production@danielrosehill`). Match on either.
4. If the plugin is set to `false` (explicitly disabled rather than enabled), surface that and ask whether to remove the entry entirely or leave the disable in place.
5. Delete the key. Write the file back with 2-space indentation, preserving other keys.
6. Confirm: plugin name, file edited, plus the **restart Claude Code** reminder.

## Safety

- Project-scoped only. Never touch user-global config.
- If `enabledPlugins` becomes empty after removal, leave it as `{}` rather than deleting the key — keeps the schema stable.
- Check `settings.local.json` too: warn if the plugin is enabled there independently (deactivation in `settings.json` won't override that).

## Output style

One-line confirmation. Restart reminder.
