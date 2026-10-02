# ADR-0006: Require Evidence Before Changing Model Routing

## Status
Accepted

## Context
Model availability, price, and vendor claims do not establish fitness for a software-engineering role. A model may complete a small coding task while ignoring workflow constraints, or appear efficient because an invalid or incomplete run performed less work.

Routing changes made from isolated successes create several failure modes:
- Candidate and incumbent receive different prompts, tools, or execution contracts.
- Timing and cost conceal regressions in correctness, scope, or instruction adherence.
- Results are interpreted after observation without a prior adoption rule.
- Capability checks are mistaken for evidence that a model fits a role.

## Decision
Every model-routing change requires a comparative evaluation before activation:

1. **Freeze the protocol.** Record candidates, workloads, order, repetitions, limits, failure handling, and the adoption rule before dispatch.
2. **Separate capability from fitness.** Confirm that each pinned provider, model, and reasoning setting responds, then evaluate coding quality and workflow adherence as distinct role claims.
3. **Hold the execution contract constant.** Use identical prompts, tools, permissions, fresh workspaces, and candidate-independent oracles while preserving each candidate's pinned definition.
4. **Prioritize quality.** Correctness, regression preservation, scope, instruction adherence, and required human correction precede latency and cost.
5. **Retain and review evidence.** Preserve raw responses, work products, oracle outcomes, and resource observations, then require an independent adjudicator to verify exclusions and recompute the result.
6. **Require promotion evidence.** Only a Promote outcome changes durable routing policy. Keep Incumbent rejects the challenger, Contender authorizes further evaluation without a mandatory failure, and Inconclusive requires setup repair.
7. **Handle forced replacement provisionally.** When the incumbent is unavailable, an emergency assignment is an explicit operational exception rather than a promotion. Freeze absolute acceptance criteria or compare available alternatives, and convert the provisional assignment into durable policy only after a Promote outcome.

## Consequences
- Routing decisions become auditable and resistant to selective interpretation.
- Capability, coding quality, workflow adherence, and operational efficiency remain distinct claims.
- The evidence supports four explicit outcomes: Promote, Keep Incumbent, Contender, or Inconclusive.
- Evaluations consume time and model budget, and small fixture sets remain screening evidence rather than broad reliability proof.
- The process cannot eliminate benchmark contamination, provider opacity, or differences between controlled fixtures and sustained production use.
