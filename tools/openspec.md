# OpenSpec

## What problem does it solve?

AI agents need structured context to make good decisions. When a project has no written specification, agents infer requirements from code, conversation history, and guesswork. The results are unpredictable: the agent may solve a different problem than the one intended, or solve the right problem in a way that conflicts with unstated constraints.

Specifications provide durable boundaries. They persist on disk, survive session boundaries, and give every agent — and every human — the same source of truth about what is being built.

OpenSpec is a schema-driven specification management CLI that structures how changes flow through a project. It defines artifact types, their dependency relationships, and their lifecycle transitions through configurable schemas.

## What role can it play?

OpenSpec manages the **documentation lane** of a spec-driven workflow. Under its default `spec-driven` schema, the artifact flow is:

**proposal** → **specs** → **design** → **tasks** → **apply** → **archive**

Other schemas can define different artifact DAGs with different stages. For example, a more governed schema might add brainstorming, planning, verification, and finalization stages. The artifact types and their dependencies are schema decisions, not universal OpenSpec constraints.

Within any schema, OpenSpec scaffolds and maintains the artifacts that govern implementation:

- **Proposals** define what is changing and why.
- **Specifications** define requirements the implementation must satisfy.
- **Designs** record technical decisions and approach.
- **Task breakdowns** define individual work units.

It also generates harness-specific skills so that the same workflow is available in Claude Code, Codex, and OpenCode.

## Important boundaries

- OpenSpec is a specification tool, not an implementation tool. It does not write application code, run tests, or deploy anything.
- It manages artifact files on disk and their lifecycle transitions. The content of those artifacts is written by agents or humans; OpenSpec provides structure, not substance.
- Its store resolution walks upward from the current directory to find the nearest `openspec/` root. This is convenient but means a misplaced working directory can resolve to the wrong store silently.
- This repository chooses to use OpenSpec primarily for the documentation/specification lane. This is a composition decision (see [Specification-Governed Agentic Delivery](../compositions/spec-governed-agentic-delivery.md)), not a limitation of OpenSpec itself. OpenSpec can be used for other purposes.

## What did we learn?

- **Specifications prevent scope drift in agent-driven work.** Without them, agents progressively expand the scope of a change as they discover adjacent concerns. With a specification boundary, the agent can note the adjacent concern and defer it.
- **The artifact DAG enforces dependency ordering.** The schema defines which artifacts must exist before others can be created. This prevents the common failure mode of jumping straight to implementation.
- **Generated skills must be regenerated together, not hand-edited individually.** OpenSpec generates skills for each harness (`.claude/skills/`, `.agents/skills/`, `.opencode/skills/`). Editing one without regenerating the others causes drift. The correct approach is to modify the generation source and regenerate all three.
- **Schema selection is a manual step.** After adoption, `openspec/config.yaml` must be edited to select the appropriate schema. Tooling can verify this was done but cannot do it automatically because the choice requires project-specific judgment.
