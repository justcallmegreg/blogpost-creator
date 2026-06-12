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
date: <YYYY-MM-DD>
reading_time: "<N> min"
audience: "<audience scope>"
tags: [<derived tags>]
---

# <Title>

*<N> min read · For <audience scope>*

## Problem Statement

## Solution

## Considerations Behind the Solution

## Conclusions

## Why Should You Adopt It?

## How Should You Adopt It?
```

### Reading time

After drafting, count the words in the body and set `<N> = round(word_count /
200)` minutes. Use the same value in the frontmatter `reading_time` and the
visible header line.

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

### Section guidance

- **Problem Statement** — the struggle, and who hits it.
- **Solution** — what was built/done to solve it; concrete and specific.
- **Considerations Behind the Solution** — the trade-offs and reasoning; why this
  approach over alternatives.
- **Conclusions** — what the reader now knows / can do.
- **Why Should You Adopt It?** (optional) — factual benefits.
- **How Should You Adopt It?** (optional) — concrete first steps.

## Phase 4 — Report

After writing the file, tell the user:

- The output path.
- The computed reading time and word count.
- The chosen angle and audience scope.

Offer to revise (length, tone, or focus) if they want changes.

## Edge cases

- **No `./docs` or `./notes`.** Rely on git history and the codebase. Note that
  evidence is thinner.
- **Empty or shallow git history.** Say so and lean on code and docs.
- **Not a git repository.** Skip the git step; work from docs, notes, and code.
- **Adoption sections not applicable.** Omit both adoption sections cleanly; do
  not leave empty headings.
