# Superpowers

## What problem does it solve?

AI agents without process guardrails skip steps, produce inconsistent quality, and forget verification. An agent asked to "build feature X" will typically start writing code immediately, skipping design exploration, and declare success without running tests or requesting review.

Superpowers is a skill-dispatch plugin that provides structured workflows: brainstorming, systematic debugging, test-driven development, code review, plan execution, and others. Each skill is a set of instructions that the agent loads and follows, injecting discipline into what would otherwise be an ad-hoc process.

## What role can it play?

Superpowers provides **process skills** that sit above domain-specific work:

- **Brainstorming**: explores intent, requirements, and design alternatives before implementation begins.
- **Systematic debugging**: builds hypotheses and tests them methodically instead of applying random fixes.
- **Test-driven development**: writes tests before implementation code.
- **Code review**: assesses work against requirements before declaring completion.
- **Plan execution**: follows a written plan with review checkpoints.
- **Verification before completion**: requires running verification commands and confirming output before claiming success.

Skills are invoked explicitly and the agent follows their instructions for the duration of the task.

## Important boundaries

- Superpowers provides process, not domain knowledge. It tells the agent *how* to approach a problem (brainstorm first, then implement) but not *what* to build.
- It dispatches skills but does not own business logic. The skills are instructions, not code.
- Superpowers is a multi-harness project. Claude Code, Codex, and OpenCode are supported upstream, though installation and integration differ by harness. Each harness has its own skill mechanism, and Superpowers provides compatible skill definitions for each.
- Skill content evolves. An agent that memorizes a skill's instructions from a previous session may follow outdated guidance. The correct behavior is to load the current version every time.

## What did we learn?

- **The brainstorming skill reduces premature implementation.** In practice, structured brainstorming before implementation has reduced the frequency of premature starts and scope drift in our workflow.
- **Cross-family review is a useful design heuristic.** When implementing and reviewing agents use the same model family, they may share analytical blind spots. Assigning review to a different model family is intended to reduce the risk of correlated failures. This is a design heuristic for reducing shared bias, not a guarantee of higher review quality.
- **Skills should be invoked, not memorized.** An agent that says "I know what that skill does" and skips loading it will miss updates and skip steps. The discipline of loading the skill every time is the point.
- **Process skills compose.** Brainstorming feeds into plan writing, which feeds into plan execution with review checkpoints. The skills are designed to chain, not to operate in isolation.
