# Evidence-Based Model Routing

## The Pattern

Model routing changes follow a pre-registered incumbent-versus-challenger evaluation. The incumbent is the assigned model and the challenger is the proposed replacement; representative work, candidate-independent checks, retained evidence, and a frozen adoption rule produce one of four defined outcomes.

### How it works

1. **Define the role claim.** State the role being evaluated and the behaviors that make a model fit for it. Build fitness, review quality, exploration accuracy, and instruction adherence are separate claims.

2. **Establish capability first.** Pin each provider, model, and reasoning or effort setting. A catalogue listing indicates that a target exists; a successful trivial call proves usability; neither proves role fitness.

3. **Freeze the protocol before dispatch.** Record candidates, fixtures, prompts, order, repetitions, timeouts, retry policy, budget ceiling, invalid-run handling, and promotion criteria. Hash or otherwise anchor these artifacts so later edits are visible, and apply exclusions and replacements symmetrically.

4. **Use matched representative workloads.** Give incumbent and challenger identical prompts, permissions, tools, and fresh workspaces. Alternate execution order where cache state, service load, or warm-up effects may bias observations.

5. **Score with candidate-independent evidence.** Prefer behavioral oracles, regression suites, protected-scope checks, and known-broken mutations over self-reported success. Verify that an oracle fails on the broken state and passes on a known-good state before candidate calls.

6. **Separate coding from workflow adherence.** A task blocked by a required approval gate does not measure coding quality, while bypassing that gate is an adherence failure regardless of code quality. Use explicitly pre-authorized implementation workloads and independent gate-adherence probes.

7. **Test candidate-authored tests.** Use mutation testing against the same pre-existing broken work product for every candidate. Submitted tests must pass on known-good work and fail at the intended behavioral assertion on broken work; a crash, missing helper, or unrelated environment failure does not establish regression sensitivity.

8. **Retain the evidence chain.** Preserve raw event streams, prompts, resolved targets, work products, diffs, oracle logs, resource observations, and classifications. Keep invalid runs visible, apply frozen invalid-run rules equally, and require the adjudicator to verify every exclusion.

9. **Apply decision precedence.** Evaluate correctness, regression preservation, scope, instruction adherence, and the amount of human correction required before latency or cost. Operational savings never compensate for a quality regression.

10. **Adjudicate independently.** A reviewer who neither proposed the routing change nor participated as an evaluated model recomputes results, inspects retained work, challenges protocol compliance, and distinguishes observed facts from interpretation. A different model family provides additional perspective where AI performs the review.

11. **Report the evidence level.** A few bounded fixtures support screening or Contender status, not general superiority. Screening evidence cannot be reclassified later as promotion evidence; promotion requires a protocol frozen for that decision.

### Decision outcomes

- **Promote.** A promotion-grade evaluation is complete and the challenger satisfies every mandatory criterion and the frozen adoption rule. This is the only outcome that changes durable routing policy.
- **Keep Incumbent.** Any valid completed evaluation rejects the challenger for this role under the frozen rule. The incumbent remains assigned.
- **Contender.** A completed screening protocol finds no mandatory quality or adherence failure and shows useful role fitness or operational advantages, but was not authorized or designed to promote. The incumbent remains assigned and a new promotion-grade evaluation is required.
- **Inconclusive.** Environment failures, invalid routing, fixture defects, or budget stops prevent the protocol from reaching a valid verdict. The setup is repaired before a new evaluation, and the incumbent remains assigned.

### When to use

This pattern applies when a team changes the model assigned to a durable agent role, especially when providers, model families, prices, or reasoning settings differ. It is unnecessary for an ad hoc human-selected conversation that creates no persistent routing policy. When the incumbent is unavailable, an emergency assignment is an explicit operational exception: freeze absolute acceptance criteria or compare available alternatives, and keep the assignment provisional until a Promote outcome makes it durable.

### Related decisions

- [ADR-0006: Require Evidence Before Changing Model Routing](../decisions/0006-require-evidence-before-changing-model-routing.md)
