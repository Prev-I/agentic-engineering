# ADR-0003: Use Durable Specifications as Agent Boundaries

## Status
Accepted

## Context
AI agent sessions are ephemeral: they lose context when the session ends, when the context window fills, or when the agent is restarted. Any decision that exists only in conversation history is effectively lost when the session boundary is crossed.

This is not merely an inconvenience — it means that:
- Requirements discovered during exploration vanish if not captured.
- Design decisions agreed upon in one session are reliably forgotten in the next.
- Different agents working on the same codebase have no shared understanding of what was decided.

## Decision
Use persistent specification artifacts as the boundaries that govern agent behavior:

1. **Proposals** define scope — what is being changed and why.
2. **Specifications** define requirements — what the implementation must satisfy.
3. **Task breakdowns** define work units — what each implementation step must produce.
4. **Review criteria** define acceptance — how to determine if the implementation is correct.

These artifacts are files on disk, committed to version control, and survive any session boundary. An agent entering a task reads the relevant artifacts before acting. An agent completing a task updates the artifacts to reflect what was done.

The specification is not a suggestion — it is a boundary. An agent that discovers the specification is wrong must update the specification (documentation lane) before changing the implementation (execution lane).

## Consequences
- Multi-session work becomes possible because state persists in files.
- Multiple agents can work on the same project by reading the same specifications.
- Specifications add overhead; the classification step (ADR-0002) determines which work warrants this overhead.
- Specification quality directly affects implementation quality — a vague spec produces vague implementations.
- The archive of completed specifications becomes a knowledge base for the project.
