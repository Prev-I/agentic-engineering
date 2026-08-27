# Parent Workspace Pattern

## The Pattern

A tracked container repository holds cross-cutting documentation and configuration. Child repositories are checked out inside it but not tracked by it. Dependencies run strictly one way: the parent may reference children, but children must work when cloned alone.

### How it works

1. **The parent repository is a tracked Git repository** that contains documentation, configuration, and cross-cutting guides. It does not contain application code.

2. **Child repositories are checked out inside the parent** (typically under a `repos/` directory) but are listed in the parent's `.gitignore`. Each child is its own independent Git repository.

3. **Dependencies run one way.** The parent may reference children (e.g., in documentation tables, inventory files). No child may reference the parent by path, rely on a file the parent provides, or resolve any resource (store, schema, skill) that lives above its own root.

4. **Each child carries its own complete agent configuration.** This means its own `AGENTS.md`, its own MCP config files (`.mcp.json`, `.codex/config.toml`, `opencode.json`), and its own skills directories. A child cloned alone must have everything it needs to work with any supported harness.

5. **Runnable invariant checks enforce the boundaries.** Because the one-way dependency rule is easy to violate silently (everything works while the child sits inside the parent), automated checks verify:
   - No child references the parent by name or path
   - Every child has its own skills and resolves to its own root
   - Store registries (if applicable) point to valid locations

### Directory layout

```
parent-workspace/
├── AGENTS.md                    # Parent-level guidance
├── .gitignore                   # Includes: repos/
├── docs/                        # Cross-cutting documentation
├── repos/
│   ├── child-project-a/         # Independent Git repository
│   │   ├── AGENTS.md
│   │   ├── .mcp.json
│   │   ├── .codex/config.toml
│   │   ├── opencode.json
│   │   └── src/
│   └── child-project-b/         # Independent Git repository
│       ├── AGENTS.md
│       └── ...
└── README.md
```

### Common violations

Three common ways the one-way rule can break:

- **A child with no skills of its own.** It resolved skills from the parent, which works only while sitting inside the parent. Cloned alone, it had no workflow. Fix: ensure every child has its own skill directories.
- **Store resolution walking upward.** A specification tool that resolves the nearest root walks upward from the child. If the child loses its own root, it silently adopts the parent's. Proximity enforces the rule, not an explicit boundary.
- **Relative paths crossing boundaries.** A link written relative to one repository resolves somewhere else once the file is delivered into another repository.

### When to use

When managing multiple related but independently-deliverable repositories. The pattern is appropriate when:
- The repositories share conventions but not code
- Cross-cutting documentation needs a home
- The team wants a single working directory that contains all projects
- Each project must be independently cloneable and functional

### Notes

The invariant checks are essential — without them, violations accumulate silently because everything continues to work as long as the child sits inside the parent.
