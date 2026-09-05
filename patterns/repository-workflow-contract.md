# Repository Workflow Contract

## The Pattern

A repository declares how work reaches its stable branch in a machine-readable file at its root. Human documentation explains that declaration, agent instructions require it to be respected, and the hosting platform enforces it. One statement of intent, three consumers, no duplication.

### How it works

1. **The declaration is a small version-controlled file.** It names the branching workflow, the stable branch, an integration branch when the model has one, and the mechanism by which work is integrated. It is versioned, so a reader that does not recognise the version refuses it rather than misinterpreting it.

2. **Branching model and integration mechanism are separate fields.** Neither is derived from the other, and neither is derived from a branch name. A stable branch called `main` does not imply reviewed integration and one called `master` does not imply direct commits; those pairings are local conventions, not properties of Git.

3. **Human documentation explains, it does not restate.** `CONTRIBUTING.md` says why the workflow is what it is and what a contributor should do. It points at the declaration for the facts rather than repeating values that will drift.

4. **Agent instructions require inspection, not repetition.** `AGENTS.md` tells agents to read the policy before branching, committing, merging or preparing an integration change. It does not restate the policy, for the same reason human documentation does not.

5. **The hosting platform enforces.** Branch protection rules or rulesets are what actually prevent a violation. The declaration is readable from a clone, which is exactly where enforcement is not.

6. **Explicit policy beats inference, always.** Where no declaration exists, a tool may fall back to heuristics — a `develop` branch hints at Git Flow, branch naming hints at a convention — but these are guesses to confirm with a human, never grounds to override a file that states the answer.

### Layered responsibilities

```
.repository-policy.yaml        declarative source of truth
        |
        +--> CONTRIBUTING.md   human explanation
        |
        +--> AGENTS.md         agent behaviour: inspect before acting
        |
        +--> platform rules    enforcement
```

Each layer has one job. The declaration is the only one that is both authoritative and readable offline; the enforcement layer is the only one that can actually stop a violation. Confusing the two produces either a policy file that tries to become branch protection, or a repository that believes it is protected because a file says so.

### Multi-repository workspaces

Each repository's own declaration governs work inside that repository. A parent workspace's policy does not cascade into the repositories checked out within it.

```
workspace/
├── .repository-policy.yaml
└── checkouts/
    ├── service-a/
    │   └── .repository-policy.yaml
    ├── service-b/
    │   └── .repository-policy.yaml
    └── utility/
        └── .repository-policy.yaml
```

This makes a mixed workspace legitimate rather than inconsistent:

| Repository | Workflow |
|---|---|
| workspace | GitHub Flow |
| service-a | Git Flow |
| service-b | GitHub Flow |
| utility | direct commits to trunk |

The rule follows from repository independence: a child that resolved its workflow from a parent would behave differently cloned alone than it does nested, which is the same failure the Parent Workspace Pattern guards against for skills and specification stores.

### When to use

When repositories are worked on by coding agents, and especially when several repositories with different conventions sit in one working directory. The pattern earns its keep when:

- Agents perform Git operations rather than only proposing them
- Repositories in the same context integrate work differently
- The hosting platform's rules are not visible from a clone, or do not exist yet
- A workflow question would otherwise be answered from habit

It is not worth the file for a single repository worked on by one person who never delegates Git operations. The cost is small but so is the ambiguity being removed.

### Related decisions

- [ADR-0005: Declare Repository Workflow as Machine-Readable Policy](../decisions/0005-declare-repository-workflow-as-machine-readable-policy.md) — the reasoning, the alternatives, and why scope stops short of branch protection
- [Parent Workspace Pattern](parent-workspace-pattern.md) — the independence rule that makes a parent policy non-cascading

### Implementation

A reference format, JSON Schema, examples and validator are in [agentic-dev-toolkit](https://github.com/Prev-I/agentic-dev-toolkit/tree/main/repository-policy).
