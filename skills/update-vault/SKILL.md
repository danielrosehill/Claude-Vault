---
name: update-vault
description: Refresh the vault — pull plugin updates and regenerate the skill-index used by vault-search. Use when the user asks to "update the vault" or after adding new plugins to the inventory.
---

# update-vault

Two jobs: pull updates for vault-tracked plugin sources, and regenerate `skill-index.txt` so `vault-search` reflects current state.

## Steps

1. Resolve the inventory and vault dir per `skills/_shared/inventory-path.md`. Read `inventory.json`. If missing, tell the user to create it (per README) and stop.

2. **Plugin updates** (best-effort):
   - For each plugin entry, look for the marketplace cache directory (typically `~/.claude/plugins/cache/<marketplace>/<plugin-name>` or similar). If it's a git checkout, run `git -C <path> pull --ff-only` and capture the result (updated / already up to date / failed).
   - If the path isn't a git repo or doesn't exist, mark as `skipped (not git)` — don't fail the whole run.
   - Collect results into a per-plugin status line.

3. **MCP servers** — surface available updates without applying:
   - For npm-style entries (`command: npx`, args containing `-y <pkg>`), note the package name. Don't auto-update; just report.
   - Skip everything else.

4. **Skill-index regeneration** (per `skills/_shared/skill-index.md`):
   - Open `$VAULT_DIR/skill-index.txt` for writing (truncate).
   - For each plugin in the inventory:
     - If the entry has a `skills` array (`[{name, description}]`), emit one line per skill: `<name> | <plugin> | <description>` (strip newlines and pipes from description, trim to ~100 chars).
     - Else, attempt best-effort discovery: scan the plugin's installed directory for `skills/*/SKILL.md` files; parse the frontmatter `name` and `description`; emit those lines. (Use `head -20` per file plus a simple awk/grep for the frontmatter — don't load full bodies.)
     - If neither yields anything, emit a single placeholder: `<plugin-name> | <plugin-name> | (no skill metadata — see plugin README)`.
   - Sort the output for stable diffs.

5. Report a summary: N plugins pulled (X updated, Y already current, Z skipped), M skill-index rows written.

## Safety

- Never run `git pull` outside the marketplace cache directory.
- Never modify the inventory file itself — this skill is read-only against `inventory.json`.
- If a plugin directory has uncommitted local changes, skip the pull and surface the warning.

## Output style

One status line per plugin during the pull phase, then a totals line, then the index regeneration totals. Don't dump file contents.
