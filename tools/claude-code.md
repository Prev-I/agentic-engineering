# Claude Code

## What problem does it solve?

Developers using Anthropic's Claude models need a native terminal integration that can read project context, modify files, run commands, and connect to external services through MCP servers. Claude Code provides this as a CLI agent that operates directly in the developer's working directory.

## What role can it play?

Claude Code is a **development agent** in a multi-harness setup:

- Loads project instructions from `CLAUDE.md` at the repository root.
- Supports `@`-import syntax for file inclusion (e.g., `@AGENTS.md` pulls in the referenced file's content).
- Has its own skill system under `.claude/skills/`, where each skill is a markdown file with structured instructions.
- Has a command system under `.claude/commands/` for custom slash commands.
- Reads MCP server configuration from `.mcp.json`.
- Uses `.claude/settings.json` for project-level settings (server enablement, permission defaults) and `.claude/settings.local.json` for machine-specific overrides.

## Important boundaries

- Claude Code reads `CLAUDE.md`, not `AGENTS.md`, as its native instruction file. Compatibility with the shared-instruction pattern requires the `@AGENTS.md` redirect.
- MCP server configuration lives in `.mcp.json`, which is a different format from what Codex (`.codex/config.toml`) and OpenCode (`opencode.json`) use.
- Skills live under `.claude/skills/` and are not interchangeable with OpenCode's `.opencode/skills/` or Codex's `.agents/skills/` without adaptation.
- `.claude/settings.json` controls which MCP servers are enabled and must be updated when adding new servers.

## What did we learn?

- **The `@AGENTS.md` import makes Claude Code compatible with the shared-instruction pattern.** A single canonical instruction file (`AGENTS.md`) serves all three harnesses. Claude Code's `@`-import is the mechanism that makes this work for Claude specifically.
- **`.claude/settings.json` controls server enablement.** Adding an MCP server to `.mcp.json` is not sufficient; it must also be enabled in the settings file. This is a common source of "the server is configured but not available" issues.
- **`.claude/settings.local.json` provides machine-specific overrides and should be gitignored.** It allows developers to customize behavior (e.g., different model preferences, local-only servers) without affecting the shared configuration.
- **Skill parity across harnesses requires maintaining three parallel skill sets.** When skills are generated (e.g., by OpenSpec), all three harness directories must be regenerated together. Hand-editing one harness's skills without updating the others causes behavioral drift.
