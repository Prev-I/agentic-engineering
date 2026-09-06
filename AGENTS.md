# Agentic Engineering

Instructions for all AI coding agents (Claude Code, Codex, OpenCode).

## Project Overview

A documentation repository. It records what was learned building software with AI
coding agents: tool assessments, architectural decisions, reusable patterns, and
the compositions and examples that show them working together.

There is **no code, no build, no test suite and nothing executable**. The
deliverable is prose. Changes are edits to Markdown, and the quality bar is
whether a reader who has never seen this repository can apply the idea somewhere
else.

Documents are **rewritten, not copied**. Anything drawn from a real project is
restated as a generic principle with the original context removed. No employer
name, no product name, no internal system, no path from the machine it came from.

## Repository Structure

```
tools/          Assessments of external building blocks and their properties
decisions/      Architectural decision records
patterns/       Reusable approaches: how they work and when to apply them
compositions/   How several patterns combine into a coherent workflow
examples/       Minimal neutral artifacts demonstrating a concept
docs/images/    Repository imagery only
```

## Writing a decision record

File name: `NNNN-kebab-slug.md`, four digits, zero-padded, next number in
sequence. The slug compresses the title and does not have to reproduce it word
for word.

The heading is `# ADR-NNNN: Title In Title Case`. The `ADR-` prefix appears in the
heading and never in the file name.

Four sections, in this order, with **no blank line between a heading and the text
under it**:

```markdown
# ADR-NNNN: Title In Title Case

## Status
Accepted

## Context
Prose describing the forces at work, optionally followed by a bullet list of the
failure modes that motivate the decision.

## Decision
A lead-in sentence ending in a colon, then a numbered list whose items open with
a bold term, or a table when the content is a matrix.

## Consequences
- What this buys.
- What it costs, and the residual risk. The closing bullets carry the downside.
```

There is no front matter, no date, no deciders list, and no separate section for
alternatives — where options matter, present them as a table inside `## Decision`
before stating the choice. `## Consequences` is always a flat bullet list mixing
benefits and costs, never two sub-lists.

Cross-references to other records are written inline without a link, as
`(ADR-0002)`.

## Writing a pattern

File name: lowercase kebab-case matching the title, no number and no prefix.

Exactly one `##` heading, `## The Pattern`. Everything below it is `###`, in
sentence case. **Blank line after `##` and `###` headings** — the opposite of the
decision records. One pattern nests to `####` and runs its list straight on from
the heading; that depth is rare enough not to be a rule either way.

```markdown
# Title In Title Case

## The Pattern

One to three sentences stating the pattern in the present tense.

### How it works

1. **Bold imperative or claim.** The explanation.

### When to use

Prose, optionally followed by a bullet list of conditions.

### Related decisions

- [ADR-000N: Full Title](../decisions/000N-slug.md) — optional gloss
```

`### Related decisions` closes the file when a decision backs the pattern, and
links carry the full `ADR-000N:` prefix in the link text.

## Prose conventions

- **Third person.** Write about "the agent", "the team", "this repository".
  Second person is effectively absent from the repository; keep it that way.
- **Present tense, declarative.** State how things are, not how they might be.
- **Short paragraphs**, one to three sentences. Sentences run 20–35 words.
- **Bullets over tables.** Reserve a table for a genuine matrix.
- **Bold-lead list items**: a bolded term or imperative, a period or colon, then
  the explanation.
- **Em dashes are `—`, spaced.** Arrows `→` are used for flows.
- Do not hard-wrap prose. One paragraph is one line, however long.
- One trailing newline at end of file. LF line endings.
- Brevity is the norm: decisions run 25–35 lines, patterns 35–65.

## Build and Test

There is none. No CI, no linter, no link checker, no package manifest.

Verification is reading: check that a new decision's number is unused, that
relative links resolve, and that the document says something a stranger could
apply.

```bash
# relative links in a file resolve
grep -o '](\.\./[^)]*)' <file> | sed 's/^](//;s/)$//'
```

## Git conventions

Read `.repository-policy.yaml` before branching, committing or integrating. It
declares this repository's workflow in machine-readable form and is authoritative
over any assumption drawn from a branch name.

At present it declares trunk-based development with **direct commits to `main`**;
there is no pull-request gate. Do not infer the opposite because the stable branch
is called `main`.

- Conventional commits: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`.
  Lowercase subject, imperative mood, no trailing period, no scope.
- `docs:` covers nearly everything here, since nearly everything is documentation.
- Keep commits atomic: one decision, one pattern, or one coherent revision.
- No secrets, tokens, API keys or connection strings in any committed file.

## Scope

In scope: generic, transferable engineering knowledge about working with AI
coding agents.

Out of scope: application code, tooling and installers, workflow or specification
products, chronological project journals, and raw research notes. A document that
only makes sense to someone who worked on the original project does not belong
here until it has been rewritten so that it does not.
