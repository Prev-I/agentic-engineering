# ADR-0002: Classify Work Before Routing

## Status
Accepted

## Context
A coding agent that receives a task must decide how to handle it: should it plan first, implement directly, explore the codebase, escalate for review, or ask for clarification? Without explicit classification, the agent defaults to its training behavior, which typically means starting implementation immediately.

This creates several failure modes:
- Complex architectural changes are implemented without design review.
- Simple typo fixes go through unnecessary specification workflows.
- Exploration and implementation get tangled — the agent modifies code while still understanding the problem.

## Decision
Every incoming task is classified before any action is taken. The classification determines the routing:

| Classification | Route |
|---|---|
| Trivial mechanical edit | Direct implementation, no specification |
| Bounded change to existing code | Short design in conversation, then implementation |
| New subsystem or architectural change | Full specification → plan → implementation |
| Investigation or diagnosis | Read-only exploration, no file modifications |
| Review request | Independent assessment, no implementation authority |

The classification is announced explicitly so it can be overridden by the human supervisor. The ratchet is one-way: discovering hidden complexity during execution upgrades the classification (e.g., bounded → architectural), but nothing downgrades it mid-task.

## Consequences
- Trivial work proceeds quickly without ceremony.
- Complex work gets appropriate specification depth before implementation begins.
- The human supervisor has a clear intervention point (classification announcement).
- Misclassification is the primary risk; the one-way ratchet mitigates underestimation but not overestimation.
