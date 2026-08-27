# Shared Agent Instructions

## The Pattern

When multiple AI coding harnesses (Claude Code, Codex, OpenCode) work in the same repository, project guidance must be available to all of them without duplication.

### How it works

1. **Write all project guidance in `AGENTS.md`** at the repository root. Use harness-neutral language: describe what the project needs, not which tool command to run.

2. **Create `CLAUDE.md` with one line:**
   ```
   @AGENTS.md
   ```
   Claude Code reads `CLAUDE.md` natively and the `@` syntax imports the referenced file. Codex and OpenCode read `AGENTS.md` directly.

3. **Define MCP servers in three parallel config files** with identical server names and commands:

   | Harness | Config file | Format |
   |---|---|---|
   | Claude Code | `.mcp.json` | JSON |
   | Codex | `.codex/config.toml` | TOML |
   | OpenCode | `opencode.json` | JSON (different schema from `.mcp.json`) |

4. **Run a parity verification script** to catch drift between the three configurations. The script extracts server names from each file and reports any that exist in one format but not the others.

### File layout

```
project/
├── AGENTS.md              # Canonical instructions (all harnesses)
├── CLAUDE.md              # Contains: @AGENTS.md
├── .mcp.json              # MCP servers for Claude Code
├── .codex/config.toml     # MCP servers for Codex
├── opencode.json          # MCP servers for OpenCode
├── .claude/
│   ├── settings.json      # Claude Code project settings
│   └── skills/            # Claude Code skills
├── .agents/skills/        # Codex skills
└── .opencode/
    ├── skills/            # OpenCode skills
    ├── agents/            # OpenCode custom agents
    └── commands/          # OpenCode custom commands
```

### When to use

Any project where more than one AI coding harness may be used. Even if the team currently uses only one harness, this pattern has low overhead and makes future adoption of additional harnesses trivial.

### Related decisions

- [ADR-0004: Separate Shared Instructions from Harness Adapters](../decisions/0004-separate-shared-instructions-from-harness-adapters.md)
