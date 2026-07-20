<!--
LinkedIn-memo template. Copied by the `linkedin-memo` skill when a memo is scaffolded for a *published* post. Do not edit an actual memo by hand from this template — invoke the skill. Fill every `{{placeholder}}`. Delete any bracketed guidance in italics once the section is populated.

A memo is an authoring-only artifact under `linkedin-memos/<YYYY-MM-DD>-<post-slug>.md` (same date + slug as the post's `_posts/` file). It is never published — the only thing keeping it out of the built site is the `exclude:` entry for `linkedin-memos` in `_config.yml`. This file carries YAML front-matter, so if that exclude entry is ever removed, Jekyll would render the memo as a live page and leak the draft ad copy. Do not remove `linkedin-memos` from `_config.yml`'s `exclude`.
-->
---
post: https://maciejmalecki.github.io/{{post-slug}}
post_task: ../tasks/task-{{NNN}}-{{task-slug}}.md
created: {{YYYY-MM-DD}}
---

<!--
Ready-to-paste LinkedIn post. Copy everything between the two PASTE markers and paste it straight into LinkedIn.

LinkedIn length guidance: the post body has a ~3000-character hard limit, and only the first ~140 characters show before the "…see more" fold — put the hook in the first sentence.
-->

<!-- PASTE BEGIN -->
{{Opening hook — one punchy sentence that stands on its own before the "…see more" fold.}}

{{One to three more sentences summarising what the article is about and why it is worth a reader's time.}}

{{Call to action — a short line inviting the reader to read the full article.}}

Read more: https://maciejmalecki.github.io/{{post-slug}}

[hashtags — a single line of 3–5 relevant tags, e.g. #Jekyll #Commodore64 #GenAI]
<!-- PASTE END -->
