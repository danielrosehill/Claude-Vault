---
name: vault-search
description: Find a dormant skill or plugin in the vault by keyword. Use when the user describes a task ("trim silence", "look up a medication") and the matching tool may live in a dormant plugin.
---

# vault-search

Progressive discovery for the vault. Searches a flat skill-index across every dormant plugin and surfaces matches with their parent plugin so the user can activate just what they need.

## When to use

Whenever the user's request *might* be served by a dormant plugin and you don't already see a matching active skill. Cheap to call — single grep against a flat file.

## Inputs

A query string (one or more keywords). If the user didn't give one, infer from their stated task.

## Steps

1. Resolve `$VAULT_DIR/skill-index.txt` per `skills/_shared/skill-index.md`. If missing, tell the user to run `update-vault` to generate it, then stop.
2. Run a case-insensitive grep for each keyword against the index. Score lines by number of keyword matches; take top 5–10.
3. For each hit, render: `<skill-name>` — `[<plugin>]` — `<description>`.
4. If exactly one strong match, offer to activate the parent plugin via `activate-plugin`. If multiple plugins are involved, list them grouped by plugin and ask which to activate.
5. If no matches, say so plainly. Do not invent skills.

## Output style

Compact list. No preamble. End with one line: "Activate `<plugin>`?" when there's a clear winner, or "Which plugin do you want?" when ambiguous.

## Notes

- This skill never activates anything itself — it only surfaces candidates and hands off to `activate-plugin` / `activate-bundle`.
- Skill names in the index are the *dormant* skill name. The user cannot invoke them until the parent plugin is activated and Claude Code is restarted in the project.
