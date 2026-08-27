# Specification-Governed Agentic Delivery

## Overview

This composition describes how the patterns and decisions in this repository work together to form a coherent workflow for AI-driven software delivery under human supervision.

The workflow is not a product — it is an approach. Any team can implement it using whatever specification tooling and agent harnesses they prefer. The patterns are composable; the composition below shows one way they fit together.

## The Workflow

### 1. Work arrives

A task is received — a feature request, a bug report, an architectural question.

**Apply**: [Work Classification and Routing](../patterns/work-classification-and-routing.md)

The task is classified before any action is taken. The classification is announced to the human supervisor, who may override it. The classification determines how much process the task warrants.

- **Trivial** → skip to step 3 (implement directly, then verify)
- **Investigation** → enter read-only exploration, produce findings, stop
- **Review** → enter read-only assessment, produce evaluation, stop
- **Bounded** → continue to step 2
- **Architectural** → continue to step 2 (full durable artifacts)

### 2. Design and plan

The process depends on the classification:

**Bounded work** enters a short conversational design phase. The agent reads the codebase, discusses scope and approach with the human, and obtains **human approval** before proceeding to implementation. No durable proposal or specification artifacts are required — the conversation itself is the design record.

**Architectural work** enters the documentation lane ([ADR-0001](../decisions/0001-separate-documentation-and-execution-lanes.md)) using exploration mode ([Governed Agent Execution](../patterns/governed-agent-execution.md)). In this mode, the agent:
- Reads the codebase to understand the current state
- Produces a durable **proposal** that defines scope and rationale
- Writes **specifications** that define what the implementation must satisfy
- Creates an **implementation plan** broken into tasks — individual work units with clear inputs and outputs

These artifacts are durable — they persist on disk and survive session boundaries ([ADR-0003](../decisions/0003-use-durable-specs-as-agent-boundaries.md)). Review criteria are written before any implementation code exists, enabling independent assessment later.

### 3. Implement

The agent switches to implementation mode ([Governed Agent Execution](../patterns/governed-agent-execution.md)). It now has full edit access but is bounded by the approved design (conversational for bounded work, durable artifacts for architectural work).

For architectural work, the agent reads the specification and task breakdown before acting. If it discovers the specification is wrong, it must return to step 2 to update the specification before changing the implementation.

### 4. Verify and review

Verification confirms the implementation meets the design intent. For bounded work, this may be lightweight (tests pass, human spot-check). For architectural work, a different agent — using a different model family ([Model-Family Diversity for Review](../patterns/model-family-diversity-for-review.md)) — enters review mode:

- Reads the specification and review criteria
- Assesses the implementation against those criteria
- Produces a review report
- Cannot modify code (governed read-only mode)

If the review identifies issues, the workflow returns to step 3 (or step 2 if the design needs revision).

### 5. Escalate if needed

For architectural or security-sensitive decisions, an expert agent is invoked in escalation mode:

- Read-only, capped agentic steps
- Returns a structured decision packet (recommendation, reasoning, alternatives, confidence)
- The implementing agent consumes the advice but the expert does not implement

Escalation is triggered by the conditions described in [Model-Family Diversity for Review](../patterns/model-family-diversity-for-review.md).

### 6. Archive

Completed specifications are archived. The archive serves as a project knowledge base — future agents and humans can read it to understand what was decided and why.

## What this composition does NOT prescribe

- **Specific tooling.** Any specification tool, agent framework, or model provider can implement this workflow.
- **Specific file formats.** Proposals, specifications, and tasks can be markdown, YAML, or any structured format.
- **Automation level.** The transitions between steps can be manual (human triggers each step), semi-automated (agent proposes, human approves), or fully automated (with appropriate guardrails).
- **Team size.** The workflow works for a single developer with one agent or a team with multiple agents and harnesses.

## The patterns composed

| Step | Pattern / Decision |
|---|---|
| Classification | [Work Classification and Routing](../patterns/work-classification-and-routing.md), [ADR-0002](../decisions/0002-classify-work-before-routing.md) |
| Lane separation | [ADR-0001](../decisions/0001-separate-documentation-and-execution-lanes.md) |
| Durable artifacts | [ADR-0003](../decisions/0003-use-durable-specs-as-agent-boundaries.md) |
| Permission modes | [Governed Agent Execution](../patterns/governed-agent-execution.md) |
| Cross-family review | [Model-Family Diversity for Review](../patterns/model-family-diversity-for-review.md) |
| Multi-harness support | [Shared Agent Instructions](../patterns/shared-agent-instructions.md), [ADR-0004](../decisions/0004-separate-shared-instructions-from-harness-adapters.md) |
