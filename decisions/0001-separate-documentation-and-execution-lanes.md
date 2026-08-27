# ADR-0001: Separate Documentation and Execution Lanes

## Status
Accepted

## Context
When AI agents drive software delivery, two fundamentally different activities compete for the same session: understanding what to build (documentation, specification, design) and building it (code, tests, configuration). Mixing them causes problems:

- Agents that start coding before the specification is stable produce work that must be discarded when requirements change.
- Review of implementation becomes impossible when the specification itself was never written down.
- Progress tracking collapses because there is no durable artifact that records what was decided vs. what was attempted.

## Decision
Separate the workflow into two cooperating lanes:

1. **Documentation lane**: produces specifications, designs, task breakdowns, and review criteria. These artifacts are durable — they persist across agent sessions and survive context window boundaries.
2. **Execution lane**: implements code, runs tests, and produces deliverables. It consumes the documentation lane's artifacts as input and is bounded by them.

The two lanes may be driven by the same agent, but the mode switch must be explicit. An agent in the documentation lane must not write application code. An agent in the execution lane must not rewrite specifications without returning to the documentation lane.

## Consequences
- Specifications become reviewable artifacts before implementation begins.
- Agents can be restarted mid-task because the specification survives the session.
- The documentation lane creates overhead for trivial changes; a classification step is needed to determine which changes warrant full specification and which can proceed directly.
- Review criteria can be written before implementation, enabling independent assessment.
