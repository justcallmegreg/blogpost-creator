---
name: blogpost-creator
description: Use when the user wants to generate a blog post from a code repository — investigates ./docs, ./notes, git commit history and the codebase, asks a focused round of questions, crafts a catchy title, then writes a short, fact-based markdown post plus matching social media and chat promo posts.
---

# Blogpost Creator

Turn the current repository into a short, fact-based, easy-to-read blog post. The
post centers on what a reader will learn: the struggle the project faced and how
it was solved. After finalizing the post, the skill also produces two matching
promotional artifacts: a social media post and a chat message.

Run the six phases below in order. Do not skip Phase 2 (questions) or Phase 3
(title selection) unless the user explicitly tells you to use your best judgement.

## Phase 1 — Investigate

Gather evidence before writing anything. Skip any source that is absent — note
the gap rather than inventing content.

1. **Docs and notes.** If `./docs` exists, read its key files. If `./notes`
   exists, read it. These often state the project's intent and pain points
   directly.
2. **Git history (focus on commit messages).** Run:
   - `git log --oneline -50` to scan recent work.
   - `git log -30 --format='%h %ad %s%n%b' --date=short` for fuller messages and
     bodies.
   Look for recurring themes, the struggle, turning points, and fixes. If the
   repo has little or no history, say so and rely more on docs and code.
3. **Codebase survey.** Read the README, the package/manifest file (e.g.
   `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`), the top-level
   directory structure, and a few key modules. Aim to answer: what is this
   project, and what problem does it solve?
4. **Repository URL.** Run `git remote get-url origin`. If it returns a URL,
   normalize it to a browsable HTTPS form for later use: convert
   `git@host:user/repo.git` to `https://host/user/repo`, and strip any trailing
   `.git`. If the command fails or returns nothing, there is no repository URL —
   note that and move on.

Then form a short internal understanding (do not show it as a wall of text):

- **What it is** — one or two sentences.
- **The struggle** — the hard problem the project addressed.
- **Candidate angles** — 2–3 possible blog post topics, each phrased as
  "what you'll learn".

## Phase 2 — Ask one round of questions

Present your understanding from Phase 1 in a few short bullets, then ask a single
consolidated round of questions. Pre-fill answers from your investigation so the
user can simply confirm. Cover all of these in one message:

1. **Focus / angle.** Offer the 2–3 candidate angles you found and ask which to
   pursue (or let the user supply their own).
2. **Audience scope.** Who is this for? (e.g. beginners, practitioners, a
   specific stack.) Propose a default based on the project.
3. **Struggle / what you'll learn.** State the struggle you will center on,
   pre-filled from investigation; ask the user to confirm or edit.
4. **Adoption sections.** Ask whether the optional "Why should you adopt it?" and
   "How should you adopt it?" sections apply — they fit posts about a tool or
   practice a reader could adopt, and should be omitted otherwise.
5. **Length target.** Confirm the target. Default: Medium, 5–8 minutes
   (~1000–1600 words). The user may choose shorter or longer.
6. **Repository URL — only if one was found in Phase 1.** Ask whether to add the
   repository URL (`<url>`) to the posts. If Phase 1 found no `origin` URL, omit
   this question entirely.
7. **Hashtags.** Ask whether to add 2–3 targeted hashtags to the social media
   post. Default: no. This applies to the social post only — the chat message
   never uses hashtags. If the user opts out or does not answer, add none.

Wait for the user's answers before writing. If the user explicitly says to use
your best judgement, choose sensible defaults and proceed.

## Phase 3 — Craft the title

The title is the most important element. The same title is reused, identically,
across all three artifacts (post, social, chat), so craft it deliberately.

Propose **2–3 candidate titles** and ask the user to pick or edit one. Do not
proceed until a title is chosen. If the user supplies their own, use it verbatim
(still apply the rules below, especially "no emojis").

Title rules:

- **Surprising and specific.** A statement or a question that names a
  **problem / pain point** and promises a **single, simple solution**.
- **Frame success, not failure.** Prefer "How we fixed slow API calls" over "We
  struggled with API calls". People are drawn to success stories.
- **Truthful, not clickbait.** The title must be a promise the post actually
  delivers. Make it compelling, but never overpromise beyond what the content
  supports — an over-hyped title destroys credibility with a technical audience.
- **No emojis in the title.** The same title is reused on the blog post, which
  forbids emojis. Emojis belong only in the social and chat bodies, never the
  title.

## Phase 4 — Write the post

### Where to write

Write the post to `./blog/YYYY-MM-DD-<title-as-slug>.md`.

- Create the `./blog/` directory if it does not exist.
- `YYYY-MM-DD` is today's date.
- `<title-as-slug>` is the chosen title (from Phase 3), lowercased, with non-alphanumeric runs
  replaced by single hyphens and leading/trailing hyphens removed.
- If that filename already exists, append `-2`, then `-3`, and so on.

### File template

Produce exactly this structure. Omit the two adoption sections if the user said
they do not apply.

```markdown
---
title: "<Title>"
subtitle: "<One or two sentences summarizing the post.>"
date: "<YYYY-MM-DD>"
audience: "<audience scope>"
tags: [<derived tags>]
---

*<One or two sentences summarizing the post.>*

## Problem Statement

## Solution

## Considerations Behind the Solution

## Conclusions

## Why Should You Adopt It?

## How Should You Adopt It?
```

Template notes:

- **Quote the `date`.** Write it as `date: "<YYYY-MM-DD>"`. An unquoted YAML
  date parses as a date object, which fails a string-typed frontmatter schema and
  can make the post silently fail to render.
- **No `# <Title>` in the body.** The blog platform renders the title (and the
  published date and reading time) from frontmatter, so a title heading in the
  body would show twice. Start the body with the subtitle line.

### Repository link — only if the user opted in

If the user agreed in Phase 2 to add the repository URL:

- Add a `repository: <url>` line to the YAML frontmatter, immediately after the
  `tags` line.
- Add a visible footer as the **last line** of the post: `Source: <url>`.

If the user declined, or no `origin` URL was found, add neither.

### Reading time

Do not write a reading-time line into the post. The blog platform derives reading
time from word count and displays it next to the title, so an in-body "N min read"
line would duplicate it.

### Writing rules

Apply these while drafting:

- **Simple English.** Short sentences. Plain words. Minimal jargon — when a term
  is unavoidable, define it briefly the first time.
- **Fact-focused, not opinionated.** Describe what was done and why it worked.
  Avoid praise words and personal opinion ("I think", "amazing", "you should
  love"). The adoption sections may be mildly persuasive but must stay grounded
  in facts.
- **Lead with the learning.** The Problem Statement names the struggle. The rest
  shows how it was solved and what the reader will be able to do afterward.
- **Respect the length target.** Trim ruthlessly to stay within the agreed
  reading time.
- **Ground every claim** in what the repo actually shows (docs, commits, code).
  Do not invent features, results, or history.
- **No emojis.** The blog post never uses emojis. (Emojis are allowed only in the
  social and chat artifacts written in Phase 5.)
- **Subtitle.** Write a subtitle of **at most two sentences** that plainly
  summarizes what the post is about. It is a factual summary, not a second
  headline — the catchy-title rules do not apply. Use the same text in the
  frontmatter `subtitle` field and the visible italic line under the title.

### Section guidance

- **Problem Statement** — the struggle, and who hits it.
- **Solution** — what was built/done to solve it; concrete and specific.
- **Considerations Behind the Solution** — the trade-offs and reasoning; why this
  approach over alternatives.
- **Conclusions** — what the reader now knows / can do.
- **Why Should You Adopt It?** (optional) — factual benefits.
- **How Should You Adopt It?** (optional) — concrete first steps.

## Phase 5 — Write the promotional artifacts

After the blog post file is written, create two sibling files that share the
post's date and slug: a social media post and a chat message. If the post's
filename took a collision suffix (`-2`, `-3`, …), apply the same suffix to both
sibling filenames so the set stays grouped. Both reuse the title chosen in
Phase 3 and link to the post with the literal placeholder `{{POST_URL}}` — do not
ask the user for a URL; they replace the placeholder at publish time.

If the user opted in to the repository URL in Phase 2, also add it as a visible
line in **both** promo files, on its own line after the call to action: in the
social post write `Repo: <url>`; in the chat message use a casual equivalent such
as `Code's here: <url>`. If the user declined, add no repository line. This
repository link is separate from the `{{POST_URL}}` article link; both may appear.

### Shared DNA — applies to both artifacts

- **Voice: factual professor-excitement.** The enthusiasm comes from the idea
  being genuinely interesting, never from hype adjectives. Do not use praise or
  hype words such as "amazing", "game-changer", "revolutionary", or "you'll
  love". Excitement is carried by the framing of the puzzle, not by adjectives.
- **Framing lens, not a template.** Cast the project as a small investigation —
  *here was a puzzle → here's what turned out to actually work* — phrased
  naturally. Never expose scientific-method step names (hypothesis, experiment,
  model, validation, theory) in the text.
- **Promise transferable understanding.** The hook and payoff must point at
  something generalizable the reader learns ("how X actually works"), not
  project-specific trivia.
- **Concrete over abstract.** Open with a specific tension or detail, not a
  generic statement.
- **Grounding.** Reuse the Phase 3 title verbatim, link via `{{POST_URL}}`, and
  ground every claim in what the repo/post actually shows. Do not invent results.

### Social media post — `./blog/YYYY-MM-DD-<slug>.social.md`

For a professional, external audience where appearances matter. Length: about
**120–200 words**, in 2–3 short paragraphs. Write it in this order:

1. **Hook — curiosity gap.** A genuine question or surprising tension the post
   resolves; something the reader recognizes.
2. **The turn.** The one core insight — what turned out to be true / what worked,
   framed as a small finding.
3. **Payoff + soft invite.** What the reader will now understand or be able to
   do, then the call to action: `Read it here → {{POST_URL}}`.

Constraints:

- **Emoji: 0–2, functional only.** Never clustered, never decorative.
- **Engagement is pull, not push.** No explicit "comment below" style prompts.
- **Hashtags:** include 2–3 targeted hashtags **only if** the user opted in to
  hashtags in Phase 2; otherwise none. Place them on their own line at the end.

### Chat message — `./blog/YYYY-MM-DD-<slug>.chat.md`

For a casual, internal audience. Length: about **50–90 words**, in 3–5 punchy
lines. Write it in this order:

1. **Warm opener + compressed hook** (for example, "Just dug into something
   neat…").
2. **The cool bit.** The core insight in one plain line.
3. **Invite + one light nudge.** `Read it here → {{POST_URL}}` plus a single
   discussion prompt, such as "Ever hit this? Curious how others handle it."
   (adapt the wording to the post — it is an example, not a fixed string).

Constraints:

- **Emoji: 2–4, as punctuation.** Never clustered.
- **Engagement is pull plus the one light nudge** above — no more than one
  discussion prompt.
- **No hashtags** in the chat message.
- Write plain markdown only — no Slack-specific or platform-specific markup — so
  it pastes into any comms tool.

### Accessibility finishing pass — run last, on both artifacts

Before writing the two files, apply two checks to each artifact:

1. **Analogy / plain restatement.** The single core concept must be expressed in
   everyday terms — an analogy or a plain restatement — so a reader with no
   background understands it.
2. **Jargon strip.** Remove or plainly define any term an average reader would
   not know. Keep at most one unavoidable domain term, defined inline in a few
   words.

**Guardrail:** this pass clarifies; it must not delete the specific insight or
flatten the excitement. If simplifying would remove the core finding, rephrase
rather than cut it.

## Phase 6 — Report

After writing all three files, tell the user:

- The three output paths (post, social, chat).
- The chosen title.
- The word count of the post (and its approximate reading time, for reference —
  not embedded in the post).
- The chosen angle and audience scope.

Offer to revise any of the three artifacts (title, length, tone, or focus) if they
want changes.

## Edge cases

- **No `./docs` or `./notes`.** Rely on git history and the codebase. Note that
  evidence is thinner.
- **Empty or shallow git history.** Say so and lean on code and docs.
- **Not a git repository.** Skip the git step; work from docs, notes, and code.
- **Adoption sections not applicable.** Omit both adoption sections cleanly; do
  not leave empty headings.
