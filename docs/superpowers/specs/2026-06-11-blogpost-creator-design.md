# Design: `blogpost-creator` skill

**Date:** 2026-06-11
**Status:** Approved

## Summary

A Claude skill that generates a short, fact-based, easy-to-read blog post from a
code repository. When invoked inside a repo, it investigates the project (docs,
notes, git commit history, codebase), asks one focused round of questions to fix
the angle and audience, then writes a structured markdown post centered on *what
a reader will learn* and *what struggle the post explains how to solve*.

## Goals

- Turn a repository's real history and code into a publishable blog post.
- Keep posts short, in simple English, and easy to understand.
- Stay factual and non-opinionated; ground every claim in what the repo shows.
- Center each post on the learning: the struggle and what the reader can do after
  reading.
- Output a flexible markdown file with machine- and human-readable metadata.

## Non-Goals

- No publishing/deployment to a blog platform (markdown is the handoff artifact).
- No helper scripts (reading time, slug, date are computed inline by the model).
- No multi-post batch generation; one post per invocation.

## Packaging

The repository **is** the skill source, version-controlled here and
installed/symlinked into `~/.claude/skills`. Single self-contained `SKILL.md`
(approach A): investigation steps, the question round, the blogpost template, and
the writing rules all live in one file so they are always in context together.

### Repository layout

```
blogpost-creator/
├── SKILL.md          # the entire skill
└── README.md         # what it is + how to install/symlink into ~/.claude/skills
```

### `SKILL.md` frontmatter

- `name: blogpost-creator`
- `description:` "Use when the user wants to generate a blog post from a code
  repository — investigates ./docs, ./notes, git commit history and the codebase,
  asks a focused round of questions, then writes a short, fact-based,
  easy-to-read markdown post about what a reader will learn."

## Runtime flow

### Phase 1 — Investigate (autonomous)

- Read `./docs` and `./notes` if they exist; skip gracefully if absent.
- Read git history focused on commit messages: `git log --oneline -50` plus a
  fuller `git log` to find themes, struggles, and turning points.
- Survey the codebase: README, package/manifest files, top-level structure, and
  key modules — enough to understand what the project is and what problem it
  solves.
- Produce a short internal understanding: what the project is, the apparent
  struggles/turning points, and candidate blogpost angles.

### Phase 2 — One round of questions

The skill presents its understanding, then asks a single consolidated batch:

- **Focus/angle** — offers 2–3 candidate topics found during investigation.
- **Audience scope** — e.g. beginners, practitioners, a specific stack.
- **Struggle / "what you'll learn"** — pre-filled from investigation; user
  confirms or edits.
- **Adoption sections** — whether the optional "Why/How should you adopt it?"
  sections apply (is this about a tool/practice a reader could adopt?).
- **Length target** — confirm; default Medium, 5–8 min (~1000–1600 words).

### Phase 3 — Write

Generate the markdown and write to `./blog/YYYY-MM-DD-<title-as-slug>.md`.

- Create `./blog/` if missing.
- `slug` = the post title, lowercased and hyphenated.
- If a file with that name already exists, append `-2`, `-3`, etc.

### Phase 4 — Report

Tell the user the output path, the computed reading time, the word count, and the
chosen audience/angle.

## Output file structure

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
## Why Should You Adopt It?   <!-- optional -->
## How Should You Adopt It?   <!-- optional -->
```

Reading time = `round(word_count / 200)` minutes, computed inline after drafting.

## Writing rules (baked into `SKILL.md`, applied in Phase 3)

- **Simple English**: short sentences, minimal jargon; define jargon when
  unavoidable.
- **Fact-focused, not opinionated**: describe what was done and why it worked;
  avoid "I think / amazing / you should love". Adoption sections may be mildly
  persuasive but stay grounded in facts.
- **Lead with the learning**: center the post on the struggle and what the reader
  will be able to do after reading.
- **Respect the length target**: trim ruthlessly.
- **Ground every claim** in what was actually found in the repo
  (docs/commits/code) — no invented features.

## Edge cases

- No `./docs` or `./notes` → rely on git history + codebase; note the thinner
  evidence.
- Empty/shallow git history → say so; lean on code and docs.
- Not a git repo → still works from docs/notes/codebase.
- Optional adoption sections omitted cleanly when not applicable.
