# agentic-engineering

Patterns, decisions and minimal examples for governed, spec-driven, multi-harness agentic software engineering.

## Purpose

This repository captures what we learned while building software with AI coding agents under human supervision. It is a technical knowledge base, not a tutorial or a product.

The organizing model is:

**Tool** → **Decision** → **Pattern** → **Composition** → **Example**

- **Tools** document external building blocks and their relevant properties.
- **Decisions** record architectural choices using the MADR format, with context, alternatives, and consequences.
- **Patterns** describe reusable approaches: how they work and when to apply them.
- **Compositions** show how multiple patterns work together to form a coherent workflow.
- **Examples** are minimal, neutral artifacts that demonstrate a concept without requiring project-specific context.

## Repository Map

```text
agentic-engineering/
├── tools/               # Tool assessments (OpenSpec, Superpowers, OpenCode, Claude Code, Codex)
├── decisions/           # MADR-style architectural decision records
├── patterns/            # Reusable patterns for multi-agent engineering
├── compositions/        # How patterns combine into workflows
├── examples/            # Minimal neutral examples
├── LICENSE
└── README.md
```

## What This Is Not

- **Not a toolkit.** Installable configurations, scripts, and harness adapters live in [agentic-dev-toolkit](https://github.com/Prev-I/agentic-dev-toolkit).
- **Not SpecRivet documentation.** When the SpecRivet product repository is published, it will reference patterns and decisions from here, but it will be independently understandable, installable, and usable. There is no runtime dependency from SpecRivet to this repository.
- **Not a personal diary or chronological journey.** Documents are organized by concept, not by date of discovery.
- **Not a dump of research notes.** Every document was rewritten from scratch to extract only the generic engineering principle.

## Relationship to agentic-dev-toolkit

| This repository | agentic-dev-toolkit |
|---|---|
| What have I learned/designed? | What can I reuse/install? |
| Conceptual: patterns, decisions, examples | Operational: scripts, configs, adapters |
| Docs-first, minimal executable content | Executable-first, documentation supports usage |

The two repositories are complementary. This one explains *why* a certain approach works; agentic-dev-toolkit provides the *assets* that implement it.

## Philosophy

- **Docs-first.** Code and configuration exist only when they help prove or explain a pattern.
- **Rewrite, don't copy.** Every document was rewritten to extract the generic principle from its original context.
- **Evidence-aware.** Documents distinguish observed behavior, design rationale, and proposed reusable patterns.
- **Minimal examples.** Examples are small, neutral, and understandable without project-specific context.

## License

MIT
