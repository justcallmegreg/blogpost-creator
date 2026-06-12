---
name: blogpost-creator
description: Use when the user wants to generate a blog post from a code repository — investigates ./docs, ./notes, git commit history and the codebase, asks a focused round of questions, then writes a short, fact-based, easy-to-read markdown post about what a reader will learn.
---

# Blogpost Creator

Turn the current repository into a short, fact-based, easy-to-read blog post. The
post centers on what a reader will learn: the struggle the project faced and how
it was solved.

Run the four phases below in order. Do not skip Phase 2 (questions) unless the
user explicitly tells you to use your best judgement.

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

<!-- filled in Task 3 -->

## Phase 3 — Write the post

<!-- filled in Task 4 and Task 5 -->

## Phase 4 — Report

<!-- filled in Task 6 -->

## Edge cases

<!-- filled in Task 6 -->
