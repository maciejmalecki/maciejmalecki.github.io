# Meta Changelog

Audit trail of deliberate changes to Claude Code internal assets in this repository — rules (`CLAUDE.md`), agents (`.claude/agents/`), skills (`.claude/skills/`), commands (`.claude/commands/`), and configuration (`.claude/settings.json`).

Each entry is a separate Markdown file named `MET-nnnn_<slug>.md`, based on the template at [`templates/changelog-entry.md`](../templates/changelog-entry.md).

## Index

| ID | Date | Who | Summary |
|----|------|-----|---------|
| [MET-0001](MET-0001_add-meta-changelog.md) | 2026-07-03 | Maciej Małecki | Recreated the `claude-meta-changelog` skill and its template from the `research` repo |
| [MET-0002](MET-0002_add-plan-skills.md) | 2026-07-03 | Maciej Małecki | Transferred `plan` and `execute-plan` skills from `dialogue-core` (adapted, not verbatim) |
| [MET-0003](MET-0003_rewrite-claudemd-jekyll.md) | 2026-07-03 | Maciej Małecki | Rewrote `CLAUDE.md` placeholder with real Jekyll project docs (build/dev commands, architecture, deploy flow) |
| [MET-0004](MET-0004_move-plan-template.md) | 2026-07-04 | Maciej Małecki | Moved plan template to `templates/plan-template.md`; updated `plan` skill and `plans/README.md` references |
| [MET-0005](MET-0005_add-adversarial-challenge.md) | 2026-07-04 | Maciej Małecki | Added `adversarial-challenge` skill that red-teams a single blog post against its front-matter and thesis |
| [MET-0006](MET-0006_add-english-correction.md) | 2026-07-04 | Maciej Małecki | Added `english-correction` skill that corrects an article to British English and recent-post tone, in place |
| [MET-0007](MET-0007_add-git-utils.md) | 2026-07-04 | Maciej Małecki | Added `git-utils` skill wrapping git (branch/pull/commit/push conventions) via a Haiku subagent |
| [MET-0008](MET-0008_add-gh-utils.md) | 2026-07-04 | Maciej Małecki | Added `gh-utils` skill wrapping gh (PR creation and common operations) via a Haiku subagent |
| [MET-0009](MET-0009_add-post-utils.md) | 2026-07-04 | AI | Added `post-utils` skill that scaffolds section drafts and promotes them to published via a Haiku subagent |
| [MET-0010](MET-0010_add-task-utils.md) | 2026-07-04 | AI | Added `task-utils` skill: a git-tracked post-authoring task board tracking post state and four quality gates |
| [MET-0011](MET-0011_claudemd-tasks-and-skills.md) | 2026-07-04 | AI | Documented the `tasks/` directory and the `post-utils`/`task-utils` skills in `CLAUDE.md` |
| [MET-0012](MET-0012_add-task-template.md) | 2026-07-04 | AI | Added `templates/task-template.md` and `tasks/README.md` scaffold for the post-authoring task board |
| [MET-0013](MET-0013_fix-post-utils-frontmatter-contract.md) | 2026-07-04 | AI | Fixed three front-matter-contract defects in `post-utils`/`task-utils`/task template found by challenging plan-003 |
| [MET-0014](MET-0014_add-research-access.md) | 2026-07-04 | Maciej Małecki | Added `research-access` skill: read-only, Haiku-delegated grounding from the sibling `../research` repo (subjects, observations, bibliography) |
| [MET-0015](MET-0015_task-template-grounding.md) | 2026-07-04 | Maciej Małecki | Added a Research grounding section (subjects / observations / bibliography links) to `templates/task-template.md` |
| [MET-0016](MET-0016_task-utils-grounding.md) | 2026-07-04 | Maciej Małecki | Extended `task-utils` to write and reconcile a task's `../research` grounding links |
| [MET-0017](MET-0017_claudemd-research-access.md) | 2026-07-04 | Maciej Małecki | Documented `research-access` and the read-only `../research` grounding relationship in `CLAUDE.md` |
| [MET-0018](MET-0018_fix-grounding-link-level.md) | 2026-07-04 | Maciej Małecki | Fixed research-grounding link level to `../../research/...` in task template and `task-utils` §5(a) (dead-link bug found by challenging the impl) |
| [MET-0019](MET-0019_add-linkedin-memo.md) | 2026-07-05 | Maciej Małecki | Added `linkedin-memo` skill + extended `task-utils`/`CLAUDE.md` for optional build-excluded LinkedIn summary memos cross-linked via the task (executes plan-005) |
| [MET-0020](MET-0020_add-editorial-guidelines.md) | 2026-07-12 | Maciej Małecki | Added root `EDITORIAL.md` editorial charter and wired it into `CLAUDE.md`, `english-correction`, and `adversarial-challenge` (executes plan-006) |
| [MET-0021](MET-0021_plain-english-style-pref.md) | 2026-07-19 | Maciej Małecki | Added plain-wording and compound-sentence style preferences to `english-correction` and `EDITORIAL.md` §10, reflecting the author's non-native-speaker register |
| [MET-0022](MET-0022_one-sentence-per-line.md) | 2026-07-20 | Maciej Małecki | Added one-sentence-per-line source-formatting convention (blank line between paragraphs) to `EDITORIAL.md` §5 and enforced it as a re-lineation step in `english-correction` |
| [MET-0023](MET-0023_add-inline-review-skill.md) | 2026-07-20 | Maciej Małecki | Added `inline-review` skill: batch-processes inline review comments (`<!-- -->` / `//`, sentence/paragraph/section/brace scope) in a post, applies each instruction, strips the comments, edits in place |
