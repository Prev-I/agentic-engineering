# Governed Agent Execution

## The Pattern

Define execution modes with explicit permission boundaries. Each mode specifies what the agent can and cannot do, enforced by the agent framework's permission system rather than by instructions alone.

### How it works

Each mode maps to an agent definition (typically a markdown file with frontmatter) that specifies permissions:

#### Exploration mode
- **Purpose**: understand the codebase, investigate problems, clarify requirements
- **Permissions**: read files, search code, read git history
- **Restrictions**: no file edits, no command execution that modifies state
- **May produce**: specification artifacts (proposals, findings documents)

#### Review mode
- **Purpose**: assess implementation against requirements
- **Permissions**: read files, read git state (diff, log, blame)
- **Restrictions**: no file edits, no subagent spawning, no command execution
- **May produce**: review comments, assessment reports

#### Escalation mode
- **Purpose**: provide expert judgment on architectural or security decisions
- **Permissions**: read files, read git state
- **Restrictions**: no file edits, no implementation, capped number of agentic steps
- **May produce**: structured decision packets (recommendation, reasoning, alternatives, confidence)

#### Implementation mode
- **Purpose**: write code, run tests, produce deliverables
- **Permissions**: full file edit access, command execution, subagent spawning
- **Restrictions**: bounded by specification artifacts; must not rewrite specifications without returning to the documentation lane
- **May produce**: code, tests, configuration, documentation updates

### Permission matrix

| Permission | Exploration | Review | Escalation | Implementation |
|---|---|---|---|---|
| Read files | Yes | Yes | Yes | Yes |
| Edit files | No | No | No | Yes |
| Run commands | Read-only | No | No | Yes |
| Spawn subagents | Yes | No | No | Yes |
| Web access | Yes | No | Yes | Yes |
| Max agentic steps | Unlimited | Unlimited | Capped | Unlimited |

### Why the expert advises but does not implement

The escalation agent is deliberately restricted from implementation for two reasons:

1. **Separation of concerns.** The expert provides judgment; the implementing agent translates that judgment into code. This ensures the expert's advice is captured as a reviewable artifact (the decision packet) rather than buried in implementation commits.
2. **Cost control.** Expensive models used for expert judgment should not be consumed by routine implementation work. The capped step count enforces this.

### When to use

When AI agents need different trust levels for different activities. The pattern is especially important when:
- Review must be genuinely independent (not performed by the same agent that wrote the code)
- Expensive models must be used sparingly
- The team needs confidence that exploration does not accidentally modify code
- Different phases of work require different safety properties

### Related decisions

- [ADR-0001: Separate Documentation and Execution Lanes](../decisions/0001-separate-documentation-and-execution-lanes.md) — the lane separation maps to exploration/review vs. implementation modes
- [ADR-0002: Classify Work Before Routing](../decisions/0002-classify-work-before-routing.md) — classification determines which mode the agent enters
