# ADR-0005: Declare Repository Workflow as Machine-Readable Policy

## Status
Accepted

## Context
Coding agents perform Git operations directly: creating branches, committing, choosing an integration target, preparing pull requests, and moving between a workspace repository and the independent repositories checked out inside it. Each of those operations needs an answer to a question the repository has never been asked to state — where does work go, and how does it get there.

Different repositories legitimately answer differently. A workspace repository must not impose its branching model on the repositories it contains; each one owns its integration lifecycle independently. So the answer has to come from the repository being worked in, not from the agent's habits or from whatever repository it looked at last.

Git provides no portable, repository-level way to declare this. The signals an agent can actually observe are unreliable:

- `main` does not inherently mean pull-request-only, and repositories with a single maintainer commonly take direct commits on it.
- `master` does not inherently mean direct commits; it is often a repository that simply predates the rename.
- A `develop` branch strongly suggests Git Flow but says nothing about whether integration is reviewed.
- Hosting-platform protection rules do enforce the intended policy, but they are vendor-specific and may be invisible from a plain clone.

The result is that an agent either guesses from branch names, or asks a human the same question in every repository, or silently applies the wrong workflow — committing to a stable branch that expected a pull request, or opening a pull request in a repository that never uses them.

## Decision
Four options were considered:

| Option | Why it was not chosen |
|---|---|
| 1. Infer the workflow from branch names | Encodes a naming habit as if it were a rule of Git. Wrong for every repository that does not share the habit, and wrong silently. |
| 2. Document it only in `CONTRIBUTING.md` and `AGENTS.md` | Prose is written for a reader, not a parser. Two documents drift, and neither yields a value a tool can branch on. |
| 3. Use Git-local configuration such as `.git/config` | Not version-controlled and not distributed by clone. Every contributor and every fresh checkout would have to set it independently. |
| 4. Add a small versioned machine-readable policy file | Chosen. |

Adopt option 4: a version-controlled `.repository-policy.yaml` at the repository root.

1. **The policy is versioned.** A reader that does not recognise the version refuses the file rather than applying current meanings to a document written under different ones.
2. **Branching workflow and integration mechanism are independent fields.** Neither is inferred from the other, and neither is inferred from a branch name. All four combinations of model and mechanism are expressible because all four occur in practice.
3. **The declaration does not enforce.** Hosting-platform rules enforce; this file states what they are meant to enforce, and is readable when they are not.
4. **Scope stops at the workflow.** Required reviewers, code owners, signed commits, merge queues, merge-strategy rules and required status checks are enforcement configuration and are excluded.

Point 4 is a constraint, not an omission. The obvious failure mode for this format is growing into a second, weaker branch-protection system — one that describes more and more of what a hosting platform already enforces, while being unable to stop anything. A declaration that cannot enforce must stay small enough that nobody mistakes it for a control.

## Consequences
- An agent reads the workflow instead of guessing it, and behaves the same way across harnesses and hosting vendors.
- The declaration travels with the clone, so it works for a standalone repository and for one nested in a workspace, with no dependency on platform APIs or network access.
- Human documentation and agent instructions can both point at one source of truth rather than restating the policy and drifting from it.
- It is a custom convention, and no tool outside the ones that adopt it will read the file.
- The declared policy and the hosting platform's enforcement can disagree, and nothing detects that; a policy claiming reviewed integration in a repository with no protection rules is aspirational rather than false, and only a human reading both will notice.
- Resisting scope growth is ongoing work — each individual field that gets proposed will look reasonable on its own.
