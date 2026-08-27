# OpenCode

## What problem does it solve?

Developers need an AI coding agent that integrates with their terminal workflow, supports multiple AI models from different providers, and allows fine-grained control over which model handles which kind of work. Most agent interfaces are either locked to a single provider or treat model selection as a global setting rather than a per-task routing decision.

OpenCode is an interactive CLI agent for software engineering tasks. It runs in the terminal, supports multiple model providers, and allows configuring which model handles each type of work through its agent system.

## What role can it play?

OpenCode is a **primary development agent** in a multi-harness setup:

- **Primary agents** (interactive, full-capability):
  - `build` — the main interactive controller and implementer.
  - `plan` — design, decomposition, and planning mode with restricted edit access.
- **Built-in subagents** (delegated, task-scoped):
  - `general` — focused delegated worker for bounded implementation and debugging tasks.
  - `explore` — local codebase navigation and discovery.
  - `scout` — external documentation, upstream repositories, and cross-source research.
- **Custom subagents**: defined in `.opencode/agents/` as markdown files with frontmatter that specifies model, permissions, and instructions. Examples include a reviewer agent (read-only, different model family) and an expert agent (escalation-only, capped steps).
- **Skills**: loaded from `.opencode/skills/` directories, providing structured workflows that agents follow.
- **Commands**: slash commands defined in `.opencode/commands/` that invoke skills or execute common workflows.

## Important boundaries

- Model and variant availability depends on the provider and the user's access. A configuration that routes to a specific model will fail if that model is not available.
- OpenCode does not enforce workflow by itself. Discipline comes from instruction files (`AGENTS.md`), skills (Superpowers or custom), and agent permission boundaries — not from the tool's default behavior.
- The `instructions` field in `opencode.json` loads policy files that are prepended to the system prompt alongside `AGENTS.md`. This is a powerful mechanism for injecting constraints but is easy to overlook.

## What did we learn?

- **Agent-to-model routing should be deterministic (config) + semantic (instruction file).** The `opencode.json` agent definitions provide deterministic routing (this agent uses this model). The instruction files and skills provide semantic routing (this type of work goes to this agent). Both are needed; either alone is insufficient.
- **The `instructions` field in `opencode.json` loads policy files alongside `AGENTS.md`.** This allows separating concerns: `AGENTS.md` carries project-wide guidance, while policy files carry agent-specific constraints (e.g., "you are a reviewer; do not modify code").
- **Permission rules (edit/bash/task) are the primary safety mechanism for subagents.** A reviewer agent with `edit: deny` and `bash: deny` physically cannot modify code, regardless of what its instructions say. This is a stronger guarantee than instruction-based constraints alone.
- **Subagent isolation matters for review quality.** A review agent that can spawn its own subagents or modify files is not truly independent. The permission system must enforce read-only access for genuine adversarial assessment.
