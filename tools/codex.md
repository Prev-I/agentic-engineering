# Codex

## What problem does it solve?

Developers using OpenAI models need a terminal agent that can work alongside other AI coding harnesses in the same repository. Codex provides this as a CLI agent that reads project instructions, connects to MCP servers, and supports a skill system for structured workflows.

## What role can it play?

Codex is a **development agent** in a multi-harness setup:

- Loads `AGENTS.md` natively — no redirect file needed.
- Uses `.codex/config.toml` (TOML format) for MCP server configuration.
- Skills live under `.agents/skills/`, with each skill as a markdown file.
- Walks from the repository root down to the current working directory, concatenating each `AGENTS.md` it encounters along the way.

## Important boundaries

- No equivalent of Claude Code's `@`-import syntax. Codex reads `AGENTS.md` directly, which means no file indirection is needed but also no file inclusion is available.
- Configuration format is TOML (`.codex/config.toml`), different from Claude Code's JSON (`.mcp.json`) and OpenCode's JSON (`opencode.json`). The same MCP servers must be defined in all three formats.
- Skills live under `.agents/skills/`, a different path from Claude Code's `.claude/skills/` and OpenCode's `.opencode/skills/`.

## What did we learn?

- **Codex walks from root down to cwd for `AGENTS.md`, concatenating each one it meets.** This means nested `AGENTS.md` files (e.g., in subdirectories or monorepo packages) are automatically included. This is a different loading model from Claude Code (single file with imports) and OpenCode (single file at root).
- **Skill parity across harnesses requires maintaining three copies (or generating them).** The `.agents/skills/` directory is Codex-specific. When the same workflow must be available in all three harnesses, the skill content must exist in three locations. Generation from a single source (e.g., OpenSpec's skill generation) is the sustainable approach; manual maintenance of three copies drifts.
- **TOML configuration is the odd format out.** Both Claude Code and OpenCode use JSON for MCP configuration. Codex uses TOML. A parity verification script that checks all three formats are equivalent is essential to prevent silent configuration drift.
