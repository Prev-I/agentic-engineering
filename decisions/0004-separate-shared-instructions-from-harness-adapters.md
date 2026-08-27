# ADR-0004: Separate Shared Instructions from Harness Adapters

## Status
Accepted

## Context
AI coding agents (Claude Code, Codex, OpenCode) each load project instructions from different files in different formats. Without coordination, teams either:

- Write instructions in one harness's format and leave others without guidance.
- Duplicate instructions across multiple files, which inevitably drift apart.
- Avoid instructions entirely, losing the ability to encode project conventions.

The same problem affects MCP server configuration: each harness uses a different config format (.mcp.json, .codex/config.toml, opencode.json), and defining servers in only one format leaves other harnesses without access to those tools.

## Decision
Maintain a single canonical instruction file (`AGENTS.md`) and thin harness-specific adapters:

- **AGENTS.md** at the repository root contains all project guidance in harness-neutral language.
- **CLAUDE.md** contains exactly `@AGENTS.md` — Claude Code's import syntax pulls in the canonical file.
- Codex and OpenCode load `AGENTS.md` natively; no adapter needed for instructions.
- MCP servers are defined in parallel across all three config formats with identical server names and commands.

The canonical file uses harness-neutral language: instead of referencing harness-specific tool names, it describes capabilities ("the MCP server connects to the issue tracker") and lets each harness discover available tools at runtime.

## Consequences
- Editing `AGENTS.md` once updates guidance for all three harnesses.
- MCP configuration must still be maintained in three files; a verification script checks parity.
- The `CLAUDE.md` redirect is a leaky abstraction — it works only because Claude Code supports the `@` import syntax. A new harness that reads `CLAUDE.md` but not `AGENTS.md` and does not support imports would need its own solution.
- Skills and commands must also be maintained per-harness (`.claude/skills/`, `.agents/skills/`, `.opencode/skills/`). Generated skill sets (e.g., from OpenSpec) should be regenerated together.
