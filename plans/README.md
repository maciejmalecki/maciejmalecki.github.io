# Plans

This directory holds structured plan documents that drive changes to this repository. Each plan is created and maintained by the `plan` skill and executed by the `execute-plan` skill (see `.claude/skills/`).

## Conventions

- One plan per file. Naming: `plan-NNN-slug.md` (zero-padded 3-digit sequence, kebab-case slug). Never reuse a number.
- Every plan is generated from [`templates/plan-template.md`](../templates/plan-template.md). Do not hand-copy the template — invoke the `plan` skill instead.
- Statuses: **draft** → **accepted** → **in-progress** → **implemented**. An `implemented` plan is a historical record; it may only be re-opened to `in-progress` when the human explicitly asks for changes.
- Keep this index in sync with plan status and one-line summaries. Both skills are responsible for updating the row below.

## Index

| ID | Plan | Status | Summary |
|----|------|--------|---------|
| plan-001 | [Relocate Jekyll blog source into -src and publish built site via GitHub Actions](plan-001-relocate-jekyll-source-and-ci-publish.md) | implemented | Copy the Jekyll source from the published repo into -src for authoring; build and publish only _site to the published repo's gh-pages via GitHub Actions. |
| plan-002 | [Clean up templates and add adversarial-challenge and english-correction authoring skills](plan-002-cleanup-and-new-authoring-skills.md) | implemented | Move the plan template into templates/; add skills for blog-post red-teaming, British-English correction, and Haiku-subagent git/gh workflow wrappers. |
| plan-003 | [Add a post-authoring helper skill with a task board tracking drafts, publication, and quality gates](plan-003-post-authoring-helper-skill.md) | implemented | Add two skills — post-utils (scaffold/promote section drafts) and task-utils (tasks/ board tracking post state and four quality gates) — both delegating mechanical file work to a Haiku subagent, with the reasoning gates run by invoking the english-correction and adversarial-challenge skills. |
| plan-004 | [Add a research-access skill that grounds posts in the ../research repository, and link tasks to research subjects, observations, and bibliography](plan-004-research-access-skill.md) | implemented | Add a read-only research-access skill (Haiku-delegated) that surfaces the sibling ../research repo's subjects, observations, and bibliography as grounding for posts; extend the task template and task-utils to link and validate that grounding. |
| plan-005 | [Optional LinkedIn summary memos for blog posts](plan-005-optional-linkedin-summary-memos.md) | implemented | Add an optional, build-excluded per-post LinkedIn memo (summary + advertisement + back-link to the published post) in a new linkedin-memos/ folder, created by a dedicated linkedin-memo skill and cross-linked to the article via its task. |
| plan-006 | [Editorial guidelines for the blog](plan-006-editorial-guidelines.md) | implemented | Add a root EDITORIAL.md capturing the blog's house style, tone, and structure (modelled on ../kaplus but adapted to Jekyll/Markdown, single-language, web publication), grounded in the recent posts as living exemplars, and wired into the english-correction/adversarial-challenge skills and CLAUDE.md. |
| plan-007 | [Split the one-year draft into a remote-control article and a one-year-summary article](plan-007-split-one-year-into-remote-and-summary.md) | implemented | Split genai/_drafts/one-year-with-agentic-ai.md into two cross-linked articles: a remote-control piece (published first, owning the full-automation section) and a one-year retrospective (published second). |

<!--
When adding a row, use the shape:
| plan-001 | [Title](plan-001-slug.md) | draft | One-line summary. |
Sort by ID ascending. Do not delete rows for implemented plans — they are the historical record.
-->
