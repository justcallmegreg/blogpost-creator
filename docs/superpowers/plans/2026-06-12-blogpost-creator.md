# blogpost-creator Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a self-contained Claude skill that turns a code repository into a short, fact-based, easy-to-read blog post.

**Architecture:** A single `SKILL.md` holds the whole skill — a four-phase runtime flow (Investigate → one question round → Write → Report), an embedded output template, and embedded writing rules. A `README.md` explains install/symlink into `~/.claude/skills`. The skill is prose/instructions, not executable code; verification is done by structure checks and a real dry-run against a sample repo.

**Tech Stack:** Markdown only. No build, no dependencies, no scripts (reading time, slug, and date are computed inline by the model at run time).

---

## File Structure

- `SKILL.md` — the entire skill: frontmatter + four-phase flow + output template + writing rules + edge cases. One responsibility: instruct the model how to produce a blog post from a repo.
- `README.md` — human-facing: what the skill is, how to install it (symlink into `~/.claude/skills`), and how to invoke it.

Both files live at the repo root. The skill is assembled incrementally so each commit leaves a coherent, readable `SKILL.md`.

> **Authoring note:** When writing `SKILL.md`, the implementer may consult the `superpowers:writing-skills` skill for frontmatter/description conventions, but must not change the design decisions fixed in the spec (`docs/superpowers/specs/2026-06-11-blogpost-creator-design.md`).

---

### Task 1: Scaffold `SKILL.md` (frontmatter + section skeleton)

**Files:**
- Create: `SKILL.md`

- [ ] **Step 1: Write the frontmatter and top-level skeleton**

Create `SKILL.md` with exactly this content:

```markdown
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

<!-- filled in Task 2 -->

## Phase 2 — Ask one round of questions

<!-- filled in Task 3 -->

## Phase 3 — Write the post

<!-- filled in Task 4 and Task 5 -->

## Phase 4 — Report

<!-- filled in Task 6 -->

## Edge cases

<!-- filled in Task 6 -->
```

- [ ] **Step 2: Verify the frontmatter parses and sections exist**

Run: `head -5 SKILL.md && grep -c '^## ' SKILL.md`
Expected: frontmatter `name`/`description` visible; grep prints `5` (five `##` sections).

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: scaffold blogpost-creator SKILL.md skeleton"
```

---

### Task 2: Write Phase 1 — Investigate

**Files:**
- Modify: `SKILL.md` (replace the `## Phase 1 — Investigate` placeholder)

- [ ] **Step 1: Replace the Phase 1 placeholder with the investigation instructions**

Replace this block:

```markdown
## Phase 1 — Investigate

<!-- filled in Task 2 -->
```

with:

```markdown
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
```

- [ ] **Step 2: Verify the section is populated and the placeholder is gone**

Run: `grep -n 'filled in Task 2' SKILL.md; grep -c 'Candidate angles' SKILL.md`
Expected: first grep prints nothing (placeholder removed); second prints `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add Phase 1 investigation instructions"
```

---

### Task 3: Write Phase 2 — Question round

**Files:**
- Modify: `SKILL.md` (replace the `## Phase 2` placeholder)

- [ ] **Step 1: Replace the Phase 2 placeholder**

Replace this block:

```markdown
## Phase 2 — Ask one round of questions

<!-- filled in Task 3 -->
```

with:

```markdown
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
```

- [ ] **Step 2: Verify**

Run: `grep -n 'filled in Task 3' SKILL.md; grep -c 'Adoption sections' SKILL.md`
Expected: first grep prints nothing; second prints `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add Phase 2 question round"
```

---

### Task 4: Write Phase 3 — Output template

**Files:**
- Modify: `SKILL.md` (replace the `## Phase 3` placeholder with template; writing rules added in Task 5)

- [ ] **Step 1: Replace the Phase 3 placeholder**

Replace this block:

```markdown
## Phase 3 — Write the post

<!-- filled in Task 4 and Task 5 -->
```

with:

````markdown
## Phase 3 — Write the post

### Where to write

Write the post to `./blog/YYYY-MM-DD-<title-as-slug>.md`.

- Create the `./blog/` directory if it does not exist.
- `YYYY-MM-DD` is today's date.
- `<title-as-slug>` is the post title, lowercased, with non-alphanumeric runs
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

<!-- filled in Task 5 -->
````

- [ ] **Step 2: Verify the template block is present**

Run: `grep -c 'Considerations Behind the Solution' SKILL.md`
Expected: `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add Phase 3 output path and template"
```

---

### Task 5: Write Phase 3 — Writing rules

**Files:**
- Modify: `SKILL.md` (replace the `### Writing rules` placeholder inside Phase 3)

- [ ] **Step 1: Replace the writing-rules placeholder**

Replace this block:

```markdown
### Writing rules

<!-- filled in Task 5 -->
```

with:

```markdown
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

### Section guidance

- **Problem Statement** — the struggle, and who hits it.
- **Solution** — what was built/done to solve it; concrete and specific.
- **Considerations Behind the Solution** — the trade-offs and reasoning; why this
  approach over alternatives.
- **Conclusions** — what the reader now knows / can do.
- **Why Should You Adopt It?** (optional) — factual benefits.
- **How Should You Adopt It?** (optional) — concrete first steps.
```

- [ ] **Step 2: Verify both the rules and section guidance exist**

Run: `grep -c 'Section guidance' SKILL.md; grep -n 'filled in Task 5' SKILL.md`
Expected: first prints `1`; second prints nothing.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add Phase 3 writing rules and section guidance"
```

---

### Task 6: Write Phase 4 and Edge cases

**Files:**
- Modify: `SKILL.md` (replace the `## Phase 4` and `## Edge cases` placeholders)

- [ ] **Step 1: Replace the Phase 4 placeholder**

Replace this block:

```markdown
## Phase 4 — Report

<!-- filled in Task 6 -->
```

with:

```markdown
## Phase 4 — Report

After writing the file, tell the user:

- The output path.
- The computed reading time and word count.
- The chosen angle and audience scope.

Offer to revise (length, tone, or focus) if they want changes.
```

- [ ] **Step 2: Replace the Edge cases placeholder**

Replace this block:

```markdown
## Edge cases

<!-- filled in Task 6 -->
```

with:

```markdown
## Edge cases

- **No `./docs` or `./notes`.** Rely on git history and the codebase. Note that
  evidence is thinner.
- **Empty or shallow git history.** Say so and lean on code and docs.
- **Not a git repository.** Skip the git step; work from docs, notes, and code.
- **Adoption sections not applicable.** Omit both adoption sections cleanly; do
  not leave empty headings.
```

- [ ] **Step 3: Verify all placeholders are gone**

Run: `grep -c 'filled in Task' SKILL.md`
Expected: `0`.

- [ ] **Step 4: Commit**

```bash
git add SKILL.md
git commit -m "feat: add Phase 4 reporting and edge cases"
```

---

### Task 7: Write `README.md`

**Files:**
- Create: `README.md`

- [ ] **Step 1: Create `README.md`**

Create `README.md` with this content:

```markdown
# blogpost-creator

A Claude skill that turns a code repository into a short, fact-based,
easy-to-read blog post about what a reader will learn from it.

## What it does

When invoked inside a repository, the skill:

1. Investigates `./docs`, `./notes`, the git commit history, and the codebase.
2. Asks one focused round of questions (angle, audience, the struggle to center
   on, whether to include adoption sections, and length).
3. Writes a structured markdown post to `./blog/YYYY-MM-DD-<title-as-slug>.md`
   with YAML frontmatter and a visible reading-time / audience header.
4. Reports the output path, reading time, word count, angle, and audience.

The post follows: Problem Statement → Solution → Considerations Behind the
Solution → Conclusions → (optional) Why / How Should You Adopt It?

## Install

Symlink (or copy) this repo into your Claude skills directory so the skill is
discoverable:

​```bash
ln -s "$(pwd)" ~/.claude/skills/blogpost-creator
​```

Then invoke it from any repository by asking Claude to create a blog post for the
project (for example: "use the blogpost-creator skill on this repo").
```

> **Implementer note:** Remove the zero-width spaces before the triple backticks in the install snippet — they are only here to keep this nested code block from terminating early. The final `README.md` must contain a normal ```` ```bash ```` fenced block.

- [ ] **Step 2: Verify**

Run: `grep -c 'ln -s' README.md && grep -c 'blogpost-creator' README.md`
Expected: first `1`; second `>= 2`.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "docs: add README with install and usage"
```

---

### Task 8: End-to-end dry run against a sample repo

**Files:**
- None modified (verification only; any generated `./blog` output under the fixture is discarded).

This task confirms the skill produces a correctly structured post.

- [ ] **Step 1: Create a throwaway sample repo**

```bash
mkdir -p /tmp/bpc-fixture && cd /tmp/bpc-fixture && git init -q
printf '# Widget\n\nA tiny CLI that caches API responses to disk.\n' > README.md
mkdir docs && printf 'We struggled with slow repeated API calls. We added a disk cache keyed by request hash.\n' > docs/notes.md
git add -A && git commit -q -m "feat: add disk cache to avoid slow repeated API calls"
git commit -q --allow-empty -m "fix: handle cache miss when hash file is corrupted"
```

- [ ] **Step 2: Run the skill against the fixture**

In a Claude session whose working directory is `/tmp/bpc-fixture`, invoke the
blogpost-creator skill. Answer the Phase 2 questions (angle: the disk cache;
audience: practitioners; include adoption sections: yes; length: Medium).

- [ ] **Step 3: Verify the output structure**

Run:
```bash
ls /tmp/bpc-fixture/blog/ && \
head -8 /tmp/bpc-fixture/blog/*.md && \
grep -c '^## ' /tmp/bpc-fixture/blog/*.md
```
Expected:
- A file named `YYYY-MM-DD-<slug>.md` exists.
- The first lines show YAML frontmatter with `title`, `date`, `reading_time`,
  `audience`, `tags`, followed by the `# Title` and the `*N min read · For ...*`
  header line.
- The `^## ` count is `6` (Problem Statement, Solution, Considerations,
  Conclusions, Why, How) — or `4` if adoption sections were declined.

- [ ] **Step 4: Confirm tone and grounding by reading the post**

Read the generated file. Confirm: simple English, no invented features (only the
disk cache and corrupted-hash handling that the fixture actually contains), and
length within the agreed band. If any rule is violated, fix `SKILL.md` and repeat
from Step 2.

- [ ] **Step 5: Clean up the fixture**

```bash
rm -rf /tmp/bpc-fixture
```

No commit (verification only).

---

## Self-Review

**Spec coverage** (against `docs/superpowers/specs/2026-06-11-blogpost-creator-design.md`):

- Packaging = single `SKILL.md` + `README.md` in repo → Tasks 1–7.
- Phase 1 Investigate (docs, notes, git commit messages, codebase) → Task 2.
- Phase 2 single question round (angle, audience, struggle, adoption, length) → Task 3.
- Phase 3 output path `./blog/YYYY-MM-DD-<title-slug>.md`, collision handling, slug rule → Task 4.
- Output template: YAML frontmatter + visible header + the five/seven sections → Task 4.
- Reading time = round(words/200) → Task 4.
- Writing rules (simple English, fact-focused, lead with learning, grounded) → Task 5.
- Phase 4 report → Task 6.
- Edge cases (no docs/notes, shallow/no git, not a repo, optional sections) → Task 6.
- README install/usage → Task 7.
- Verification of real output → Task 8.

No spec requirement is left without a task.

**Placeholder scan:** The `<!-- filled in Task N -->` markers are intentional scaffolding removed by later tasks; Task 6 Step 3 asserts none remain. No "TBD"/"add error handling"-style placeholders in the delivered content.

**Type consistency:** Section names are identical across spec, template (Task 4), and section guidance (Task 5): Problem Statement, Solution, Considerations Behind the Solution, Conclusions, Why Should You Adopt It?, How Should You Adopt It?. Output path string is identical in Tasks 4 and 8 and the spec.
