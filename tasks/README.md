# Tasks

This directory holds one Markdown file per **post-authoring task** — a lightweight, git-tracked board that lets the author juggle several in-flight blog posts and remember, per post, what has and has not been done on the way to publication. Each task is created and maintained by the `task-utils` skill; the post files it tracks are scaffolded and promoted by the `post-utils` skill (see `.claude/skills/`).

## Conventions

- One task per file, and **one post per task**. Naming: `task-NNN-slug.md` (zero-padded 3-digit sequence, kebab-case slug). Never reuse a number.
- Every task is generated from [`templates/task-template.md`](../templates/task-template.md). Do not hand-copy the template — invoke the `task-utils` skill instead.
- A task owns exactly one post, identified by its **section** (`arch` / `casual` / `cbm` / `genai` / `sh`) and **post slug**. Task creation checks no existing task already points at that section+slug.
- Each task tracks the post's **state** (`draft` or `published`) and the four **quality gates**: `language-check`, `adversarial-challenge`, `frontmatter-and-links` (pre-promotion) and `build-clean` (post-promotion). A task is `done` only when all four gates are green.
- Keep the index below in sync with each task's status, post state, and gate progress. The `task-utils` skill is responsible for updating the row.
- Switch tasks by opening a different task file — its **Notes / next actions** section restores context.

## Index

| ID | Task | Section | Post state | Status | Gates |
|----|------|---------|------------|--------|-------|
| task-001 | [Coding from my phone: a remote agentic setup](task-001-coding-from-my-phone.md) | genai | published | active | 3/4 |
| task-002 | [One year with agentic AI](task-002-one-year-with-agentic-ai.md) | genai | draft | active | 0/4 |

<!--
When adding a row, use the shape:
| task-001 | [Title](task-001-slug.md) | genai | draft | active | 0/4 |
Sort by ID ascending. The Gates column is the count of green quality gates (0–4).
-->
