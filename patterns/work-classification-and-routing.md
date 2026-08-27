# Work Classification and Routing

## The Pattern

Every incoming task is classified before any action is taken. The classification determines how much process the task warrants and routes it to the appropriate workflow depth.

### How it works

1. **Classify the task** into one of the following categories:

   | Classification | Description | Route |
   |---|---|---|
   | Trivial mechanical edit | Typo fix, import reorder, comment update | Direct implementation, no specification |
   | Bounded change | Modification to existing code with clear scope | Short design in conversation, then implementation |
   | Architectural change | New subsystem, cross-cutting concern, API change | Full specification → plan → implementation |
   | Investigation | Bug diagnosis, performance analysis, codebase exploration | Read-only exploration, no file modifications |
   | Review | Assessment of existing code or proposed changes | Independent evaluation, no implementation authority |

2. **Announce the classification** so the human supervisor can override it. The announcement is explicit: "This is a bounded change. I will design briefly, then implement."

3. **Upgrade when hidden complexity is discovered.** If a bounded change reveals architectural implications during implementation, upgrade to the architectural classification. This ratchet is one-way — complexity can upgrade a classification, but nothing downgrades it mid-task.

4. **Never downgrade mid-task.** Once a task has been classified at a given level, it stays there or goes up. This prevents the failure mode where an agent reclassifies a complex task as simple to avoid specification overhead.

### Examples

- **Trivial**: Fix a typo in a log message → implement directly.
- **Bounded**: Add a new field to an existing API response → brief design note, then implement.
- **Architectural**: Replace the authentication system → full specification, task breakdown, plan, then implement.
- **Investigation**: "Why is this endpoint slow?" → read-only exploration, produce findings, no code changes.
- **Review**: "Is this PR ready to merge?" → read code, assess against requirements, no modifications.

### When to use

Any project using AI agents for implementation where tasks vary in complexity. The pattern prevents two failure modes: over-engineering trivial changes with unnecessary specification ceremony, and under-engineering complex changes by skipping design.

### Related decisions

- [ADR-0002: Classify Work Before Routing](../decisions/0002-classify-work-before-routing.md)
