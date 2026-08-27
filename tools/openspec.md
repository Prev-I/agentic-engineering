# OpenSpec

## What problem does it solve?

AI agents need structured context to make good decisions. When a project has no written specification, agents infer requirements from code, conversation history, and guesswork. The results are unpredictable: the agent may solve a different problem than the one intended, or solve the right problem in a way that conflicts with unstated constraints.

Specifications provide durable boundaries. They persist on disk, survive session boundaries, and give every agent — and every human — the same source of truth about what is being built.

OpenSpec is a specification management CLI that structures how changes flow through a project. It manages a change lifecycle with an explicit artifact dependency graph: propose → spec → task → plan → apply → verify → finalize → archive.

## What role can it play?

OpenSpec manages the **documentation lane** of a spec-driven workflow. It scaffolds and maintains the artifacts that govern implementation:

- **Proposals** define what is changing and why.
- **Specifications** define requirements the implementation must satisfy.
- **Task breakdowns** define individual work units.
- **Plans** define the ordering and approach for implementation.

It also generates harness-specific skills so that the same workflow is available in Claude Code, Codex, and OpenCode.

## Important boundaries

- OpenSpec is a specification tool, not an implementation tool. It does not write application code, run tests, or deploy anything.
- It manages artifact files on disk and their lifecycle transitions. The content of those artifacts is written by agents or humans; OpenSpec provides structure, not substance.
- Its store resolution walks upward from the current directory to find the nearest `openspec/` root. This is convenient but means a misplaced working directory can resolve to the wrong store silently.

## What did we learn?

- **Specifications prevent scope drift in agent-driven work.** Without them, agents progressively expand the scope of a change as they discover adjacent concerns. With a specification boundary, the agent can note the adjacent concern and defer it.
- **The artifact DAG enforces dependency ordering.** You cannot create tasks without a specification, and you cannot apply changes without tasks. This prevents the common failure mode of jumping straight to implementation.
- **Generated skills must be regenerated together, not hand-edited individually.** OpenSpec generates skills for each harness (`.claude/skills/`, `.agents/skills/`, `.opencode/skills/`). Editing one without regenerating the others causes drift. The correct approach is to modify the generation source and regenerate all three.
- **Schema selection is a manual step.** After adoption, `openspec/config.yaml` must be edited to select the appropriate schema. Tooling can verify this was done but cannot do it automatically because the choice requires project-specific judgment.
