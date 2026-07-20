<!--
Plan template. Copied by the `plan` skill when a new plan is created. Do not edit an actual plan by hand from this template — invoke the skill. Fill every `{{placeholder}}`. Delete any bracketed guidance in italics once the section is populated.
-->

# {{Plan name}}

- **ID:** plan-{{NNN}}
- **Slug:** {{slug}}
- **Status:** draft
- **Created:** {{YYYY-MM-DD}}
- **Last updated:** {{YYYY-MM-DD}}

> Status values: `draft` → `accepted` → `in-progress` → `implemented`. A plan may be moved back from `implemented` to `in-progress` only when the human explicitly asks for changes to a historical plan.

## Summary

{{One or two paragraphs. What this plan proposes to change, at a glance. Written so a reader can decide in 30 seconds whether to read the rest.}}

## Rationale

{{Why this plan exists. The problem being solved, the constraints in play, the alternatives that were considered and rejected, and the reasoning behind the chosen direction. If the plan is a response to a defect, incident, or upstream decision, name it here.}}

## Open Questions

*All questions in this section must be answered by a human before the plan can move from `draft` to `accepted`. The `plan` skill will not promote status while unanswered questions remain.*

- [ ] **Q1.** {{Question text}}
  - **Answer:** _(pending)_
- [ ] **Q2.** {{Question text}}
  - **Answer:** _(pending)_

## Steps

*Each step is a self-contained, reviewable unit of work. Steps are numbered and executed in order unless the executor is explicitly asked to run a subset. Per-step execution status values: `pending`, `in-progress`, `done`, `skipped`, `blocked`.*

### Step 1 — {{Step title}}

- **Execution status:** pending
- **Intent:** {{What this step changes and why.}}
- **Touches:** {{Files, folders, or systems this step will modify.}}
- **Acceptance:** {{How we know the step is done — tests, build passes, observable behaviour, etc.}}
- **Execution summary:** _(filled in by the `execute-plan` skill once the step runs — what actually changed, deviations from intent, and any follow-ups discovered.)_

### Step 2 — {{Step title}}

- **Execution status:** pending
- **Intent:** {{...}}
- **Touches:** {{...}}
- **Acceptance:** {{...}}
- **Execution summary:** _(pending)_

<!-- Add further steps as needed, keeping the same shape. -->

## Change Log

*Appended to whenever the plan is materially revised — including when a plan is re-opened from `implemented` to `in-progress`. Do not rewrite history; add a new entry.*

- {{YYYY-MM-DD}} — Plan created (status: draft).
