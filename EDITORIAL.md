# Editorial guidelines — the blog

House style, tone, and structural conventions for posts on **"()=>{} | Void's blog"**. This is the *editorial* layer: the style a personal technical blog needs so its posts read as one voice. The *build and architecture* layer — Jekyll configuration, the `_layouts`/`_includes`/`_sass` machinery, sections, the `_drafts` → `_posts` lifecycle, and the deploy flow — lives in [`CLAUDE.md`](CLAUDE.md) and is not repeated here except where a rule sharpens it.

These guidelines describe the canonical form for **new and revised** posts. They do **not** oblige a retro-edit of existing posts; where an older post diverges, that is a known deviation, not a defect to fix in place.

## 1. Scope and audience

The reader is a technically literate practitioner. The blog's topics span software architecture (`arch`), generative and agentic AI in engineering (`genai`), retro Commodore computing (`cbm`), the shell and small hardware (`sh`), and the occasional off-topic piece (`casual`). Write for a reader who is comfortable with code and industry vocabulary but who came for the *argument or the story*, not a reference manual.

Posts are **prose, not software**: code listings serve the prose, never the reverse. The characteristic post opens with a lived observation — a memory, a colleague's article, an interview question that produced an awkward silence — and reaches the technical subject through it.

## 2. Content sections

Every post belongs to one content section, each a top-level directory with its own `_posts/`, `_drafts/`, and usually `img/`: `arch/`, `casual/`, `cbm/`, `genai/`, `sh/`. Choose the section that names the post's **primary** subject; cross-topic tagging is handled by `categories` (§4), not by filing the post in two places.

Scaffold and promote posts with the `post-utils` skill: a new draft is created in the chosen section's `_drafts/` with a complete front-matter block, and promoted by `git mv` into that section's `_posts/` with a `YYYY-MM-DD-` filename prefix when it is ready to publish. Do not hand-create or hand-rename post files — go through the skill so the lifecycle and front-matter stay consistent.

## 3. Front-matter contract

Every post opens with a YAML front-matter block between `---` fences. Grounded in the current posts:

```yaml
---
title: "Why don't I feel emotional about most of the code I write today?"
layout: post
categories: genai arch
description: Vibe coding with AI requires expertise to work effectively. You must narrow the corridor with proper constraints, architecture, and automation.
pic: /genai/img/floppy.jpg
---
```

- **`title`** — the post's title. Because the site uses `permalink: /:title`, the title must yield a clean, stable URL slug: write it so the derived slug reads well, and do not rename a published post's title (it changes the permalink). Quote the title when it contains punctuation (a `?`, a colon).
- **`layout: post`** — always `post` for a blog post.
- **`categories`** — **space-separated**, primary section first, cross-topic tags allowed (e.g. `categories: genai arch`, `categories: arch genai`). Categories describe what the post actually covers; they are also the contract the `adversarial-challenge` skill holds the post to (§5).
- **`description`** — one or two sentences stating the post's takeaway. This is a *promise*: a reader who finishes the post once should get what the `description` advertised. Keep it concrete.
- **`pic`** *(optional)* — a section-relative path to the lead / social image (e.g. `/genai/img/floppy.jpg`).

Leave front-matter in the author's own wording; the `english-correction` skill does not retone or Anglicise front-matter.

## 4. Length and scope

There is **no fixed length or word budget**. The web is not print — a post carries no page slot. Length and scope are decided **individually, per post**, and follow what the subject requires: a short reflection and a long multi-section argument are both fine when the subject warrants them.

The only standing guidance is that a post should be **as long as it needs to be and no longer**. Do not pad to hit a length, and do not impose a numeric target on a piece.

## 5. Structure and the post contract

- Open with a **thesis or a hook** — a memory, a concrete scene, or a claim — that states or sets up what the post delivers. The opening paragraphs carry the promise the rest of the post must earn.
- Use `###` for section headings (as the current posts do). Reserve `#`/`##` for the layout — do not start a post body with a top-level heading; the `title` front-matter is the heading.
- Keep section depth shallow. A post rarely needs more than `###`.

**The post contract.** A post's *contract* is the author-facing promise it must honour: its **front-matter** (`title`, `description`, `categories`) plus the **thesis stated in its opening paragraphs**. The `adversarial-challenge` skill holds the article against exactly this contract — does the body deliver the `description`'s promise, is the opening thesis actually argued, do the `categories` match the content. Write the opening so the contract can be honoured. Consistent with §4, the contract carries **no length component** — there is no slot to hit, only a promise to keep.

**Source-line formatting (one sentence per line).** In the body of a **post or page**, write **one sentence per source line**, and separate every paragraph with **at least one blank line**. This is a source-formatting convention for authoring and diff legibility only — it does **not** change the rendered page: kramdown treats a single newline inside a paragraph as a soft break, so a run of one-sentence lines with no blank line between them renders as one normal flowing paragraph, exactly as running prose. A blank line is what starts a new paragraph on the page. Concretely:

- End each sentence and start the next on a new line; do not pack several sentences onto one physical line.
- Put one (or more) blank lines between paragraphs; never let two paragraphs touch with no blank line between them.
- This is a **deliberate change** from the older posts, which were authored one-paragraph-per-line. Those are a known deviation (§ intro), not a model to copy; when the "corpus wins" rule of §10 is applied to *voice*, it does not override this *formatting* rule. New and revised posts follow the one-sentence-per-line form.
- Scope is **post/page body prose only**. It does **not** apply to repository plans or task files (the `plan`/`execute-plan` skills require one-paragraph-per-line there), nor to fenced code, tables, front-matter, or reference-link blocks — leave those in their natural form.

The `english-correction` skill applies this convention when it revises a post: it re-lineates the author's body prose to one sentence per line and ensures blank-line paragraph separation, without altering wording, order, or any of the do-not-touch regions.

## 6. Images and figures

- Block images use Markdown `![alt](/section/img/file.ext)` with a **section-relative** path under that section's `img/` folder (e.g. `![Ted Neward](/arch/img/neward.jpg)`).
- Provide meaningful **alt text**; it is not optional.
- A caption follows the image on its own line in *italics*, as the current posts do — e.g. `*Fowler's design stamina chart*.` — often carrying attribution after a `/` (`*… / Ted Neward in Geekon/Kraków/Poland, 2019*`).
- Treat images as assets: reference them by path; do not inline base64 or rewrite text baked into an image.

## 7. Links

- Prefer **reference-style** Markdown links, as the current posts use — `[interesting article][adam]` in the prose with `[adam]: https://…` defined lower down. It keeps paragraphs readable.
- Link to **primary sources**: the original article, the vendor's manual, the person's own post. When a claim leans on an external fact, the link is the support.
- Keep link *text* as the author's prose; the link *target* is data, not something to retone.

## 8. Code and listings

- Use fenced code blocks. Code **serves the prose**: introduce a listing in the surrounding text and keep it to what the point requires.
- Reproduce code, comments inside code, and any in-code text **verbatim**. Never "correct" the spelling, grammar, or style of code — that includes American spellings in identifiers and comments.
- Keep inline `code` spans for tool names, filenames, flags, and identifiers.

## 9. Quotations

- Quote other people's words in a **block quote** (`>`), reproduced **verbatim**, with attribution (e.g. the Karpathy quote in the vibe-coding post, ending in a `*Source: …*` line and a link).
- Never Anglicise, retone, or "fix" a quoted source, even where it uses American spelling or informal punctuation — the quote is evidence, not prose to edit.

## 10. Voice and tone

The register is **reflective, first-person, and essayistic** — the author thinking on the page ("When I thought about it, I said to myself…"), grounded in lived professional and scene experience, with a **light, wry touch** even on technical material and the occasional aside in parentheses. Openings hook with a memory, a colleague's piece, or a concrete scene before reaching the subject. Concrete detail is characteristic and welcome — a date, a price, a tool version, a model number — because it is both the author's style and what makes a claim checkable.

- **Single quotes** for emphasised terms and short quotations ('vibe coding', 'green field legacy', 'controlled agile').
- Measured, conversational register; contractions are fine.
- Write in the **author's own voice** — prefer the author's phrasings and cadence over a generic "polished" tone.

**Plain wording and sentence construction.** The author is not a native English speaker and writes — and prefers to read back — in a plainer register than a polished native editor would default to. Two standing preferences follow, and they apply when drafting a post and when the `english-correction` skill revises one:

- **Prefer simpler wording over sophisticated wording, and less elaboration over more.** Where a plain word carries the meaning, use it in place of an elevated or literary one (e.g. 'nothing special' over 'deliberately unspectacular', 'simple' over 'correspondingly unheroic', 'a good example' over 'a good specimen'). Do not add ornamental clauses or restate a point in a second, fancier way; say it once, plainly. This trims vocabulary and elaboration only — never the argument, the technical content, or the concrete detail (dates, prices, tool versions, model numbers) that §10 calls characteristic.
- **Prefer compound sentences over strings of short single ones.** Where several short, clipped sentences state one connected thought, join them into a compound sentence with 'and', 'but', 'so', or a semicolon (e.g. 'It sounds like a decade of progress. It has been twelve months.' → '…it sounds like a decade of progress, but it has been just twelve months.'). This matches how the author naturally writes English and reads more smoothly to them. It is a rhythm preference, not a licence to build run-ons: keep each joined sentence readable, and leave a short sentence short when the point genuinely stands alone.

These are voice preferences, so §10's living-exemplar rule still governs: where the recent corpus and this guidance seem to disagree, the corpus wins.

**The living exemplars.** The authoritative statement of the current voice is not this section but the **recent posts themselves** — the newest files under `genai/_posts/` and `arch/_posts/` (e.g. the 2025 vibe-coding, agentic-AI, emotion, and architecture pieces). The voice evolves; read those posts as the live model of it. The `english-correction` skill enforces tone by sampling that corpus on every run rather than following a frozen rulebook, and this section describes the same target it samples toward. When this section and the recent corpus seem to disagree, **the corpus wins**.

## 11. British English

Posts are written in **British English**. This is the site standard; the hard rules — `‑ise`/`‑isation` (realise, optimisation), `‑our` (colour, behaviour), `‑re` (centre), `‑ce`/`‑se` noun/verb pairs (licence/license, practice/practise), doubled `‑l‑` (travelled, modelling), single quotes for emphasis, and punctuation outside the closing quote — are owned and applied by the `english-correction` skill, which corrects a post in place and aligns its tone with the recent corpus. This section states the standard; see that skill for the full list and the do-not-touch boundaries (code, links, third-party quotes, proper nouns, front-matter).
