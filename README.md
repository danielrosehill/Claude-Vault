# Claude-Vault

Meta-plugin for **per-project activation of dormant plugins and MCP servers** from a personal vault.

Mitigates the user-level eager skill-description loading problem (see [Claude-Vault-Idea](https://github.com/danielrosehill/Claude-Vault-Idea) for the full write-up): plugins enabled at user level inject all their skill descriptions into every session's system prompt. With many plugins installed, that adds up. The vault pattern keeps everything installed-but-dormant and activates only what each project needs, by editing project-level `.claude/settings.json` and `.mcp.json`.

This is a workaround for a current Claude Code limitation, not a permanent design.

## Status

Early. `list-vault`, `list-active`, `activate-plugin`, `activate-mcp` are functional. `setup-workspace`, `activate-bundle`, `update-vault`, `bundle-create` are stubs.

## Install

```bash
claude plugin install Claude-Vault
```

(Or clone and add via your local marketplace.)

Then create the vault inventory at `~/.claude/vault/inventory.json` — see [Vault inventory format](#vault-inventory-format) below.

## Skills

| Skill | Status | What it does |
|-------|--------|--------------|
| `list-vault` | ✅ | Enumerate plugins, MCP servers, and bundles in the inventory |
| `list-active` | ✅ | Show what's currently enabled in the current project |
| `activate-plugin` | ✅ | Enable a vault plugin in the current project's `.claude/settings.json` |
| `activate-mcp` | ✅ | Add a vault MCP server to the current project's `.mcp.json` |
| `setup-workspace` | 🚧 | Bootstrap a new project with a chosen template + tools |
| `activate-bundle` | 🚧 | Apply a named bundle (e.g. `podcast-production`) |
| `update-vault` | 🚧 | Pull updates across vault plugins and MCP servers |
| `bundle-create` | 🚧 | Save the current project's tool set as a reusable bundle |

## Vault inventory format

`~/.claude/vault/inventory.json`:

```json
{
  "plugins": [
    {
      "name": "podcast-production",
      "marketplace": "danielrosehill",
      "description": "Tools for podcast editing and publishing",
      "tags": ["podcast", "audio"]
    }
  ],
  "mcpServers": [
    {
      "name": "greeninvoice",
      "description": "Israeli invoicing API",
      "config": {
        "command": "npx",
        "args": ["-y", "@greeninvoice/mcp-server"],
        "env": { "GREENINVOICE_API_KEY": "${GREENINVOICE_API_KEY}" }
      },
      "tags": ["finance", "israel"]
    }
  ],
  "bundles": [
    {
      "name": "podcast",
      "description": "Full podcast production stack",
      "plugins": ["podcast-production"],
      "mcpServers": []
    }
  ]
}
```

Plugins must already be installed on your system (cloned from a marketplace). The vault is an *activation* layer, not an installer — it does not fetch plugins.

## License

MIT
