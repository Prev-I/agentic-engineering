# Model-Family Diversity for Review

## The Pattern

When AI-generated code is reviewed by another AI agent, the reviewing agent must use a different model family than the implementing agent. Cross-family review is a design heuristic intended to reduce correlated failure modes by introducing an independent analytical perspective. It does not guarantee better review quality, but it reduces the risk that implementation and review share the same blind spots.

### How it works

1. **Assign implementation agents to one model family.** The agents that write code, generate tests, and produce deliverables all use models from the same family (e.g., Claude).

2. **Assign the review agent to a different model family.** The agent that assesses implementation quality uses a model from a different provider (e.g., GPT, Gemini). This is intended to introduce a more independent perspective on code quality, security, and correctness.

3. **Isolate scarce or expensive models behind escalation-only agents.** If a powerful model is too expensive for routine use, define an escalation agent that:
   - Is read-only (no file edits, no command execution, no subagent spawning)
   - Has a capped number of agentic steps
   - Is invoked only when specific conditions are met (architectural decisions, security-sensitive changes, unresolved disagreements between other agents)
   - Returns structured advice (a decision packet) rather than implementing changes

### The escalation gate

An escalation to the expert agent is warranted when:
- The change affects system architecture (new subsystems, changed boundaries)
- The change involves security-sensitive logic (authentication, authorization, cryptography)
- Two agents disagree on an approach and the disagreement cannot be resolved by the specification
- The classification was upgraded from bounded to architectural during implementation

The escalation produces a **decision packet**: a structured response containing the recommendation, the reasoning, the alternatives considered, and the confidence level. The implementing agent consumes this packet but the expert does not implement anything directly.

### When to use

Any project where AI-generated code is reviewed by another AI agent. The pattern is unnecessary when all review is done by humans, but becomes important as AI review supplements or gates human review.

### Related decisions

- [ADR-0002: Classify Work Before Routing](../decisions/0002-classify-work-before-routing.md) — classification determines when escalation is warranted
