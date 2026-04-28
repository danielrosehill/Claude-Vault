# Resolving the vault inventory path

All Claude-Vault skills use this same resolution. Never write to or read from `~/.claude/` for plugin data — that's Claude Code's config surface and is clobbered on plugin updates.

## Resolution order

1. If `CLAUDE_USER_DATA` is set → `$CLAUDE_USER_DATA/claude-vault/inventory.json`
2. Else if `XDG_DATA_HOME` is set → `$XDG_DATA_HOME/claude-plugins/claude-vault/inventory.json`
3. Else → `$HOME/.local/share/claude-plugins/claude-vault/inventory.json`

## Shell snippet

```bash
VAULT_DIR="${CLAUDE_USER_DATA:-${XDG_DATA_HOME:-$HOME/.local/share}/claude-plugins}/claude-vault"
INVENTORY="$VAULT_DIR/inventory.json"
mkdir -p "$VAULT_DIR"
```

## When the file doesn't exist

Skills should tell the user where it would go (computed via the rules above) and point at the README format — they should **not** silently create it with placeholder data.
