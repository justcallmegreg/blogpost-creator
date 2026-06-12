# blogpost-creator Promo Artifacts Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Extend the existing `blogpost-creator` skill so that, after writing the blog post, it crafts a catchy reused title and produces two promo artifacts — a social media post and a chat message.

**Architecture:** Edit the single `SKILL.md` in place: insert a new Phase 3 (craft the title), renumber the existing write/report phases, and insert a new Phase 5 (write social + chat sibling files). Update `README.md`. The skill is prose; verification is structure checks plus a real dry-run against a fixture repo.

**Tech Stack:** Markdown only. No build, no dependencies, no scripts.

---

## File Structure

- `SKILL.md` — the whole skill. After this plan it has six phases: Investigate, Question round, Craft the title (NEW), Write the post, Write promo artifacts (NEW), Report.
- `README.md` — updated to describe three artifacts and the `{{POST_URL}}` placeholder.

Both files exist already. All tasks are edits to existing content using exact string replacement. Apply tasks in order; later tasks assume earlier renumbering is done.

> **Reference:** spec at `docs/superpowers/specs/2026-06-12-blogpost-creator-promo-artifacts-design.md`. Current `SKILL.md` is 156 lines with phases numbered 1–4; current `README.md` is 31 lines.

---

### Task 1: Update intro and frontmatter description

**Files:**
- Modify: `SKILL.md` (frontmatter `description`, intro paragraph, "Run the four phases" line)

- [ ] **Step 1: Update the frontmatter description**

Replace this exact line:

```
description: Use when the user wants to generate a blog post from a code repository — investigates ./docs, ./notes, git commit history and the codebase, asks a focused round of questions, then writes a short, fact-based, easy-to-read markdown post about what a reader will learn.
```

with:

```
description: Use when the user wants to generate a blog post from a code repository — investigates ./docs, ./notes, git commit history and the codebase, asks a focused round of questions, crafts a catchy title, then writes a short, fact-based markdown post plus matching social media and chat promo posts.
```

- [ ] **Step 2: Add a sentence about the promo artifacts to the intro**

Replace this exact block:

```
Turn the current repository into a short, fact-based, easy-to-read blog post. The
post centers on what a reader will learn: the struggle the project faced and how
it was solved.

Run the four phases below in order. Do not skip Phase 2 (questions) unless the
user explicitly tells you to use your best judgement.
```

with:

```
Turn the current repository into a short, fact-based, easy-to-read blog post. The
post centers on what a reader will learn: the struggle the project faced and how
it was solved. After finalizing the post, the skill also produces two matching
promotional artifacts: a social media post and a chat message.

Run the six phases below in order. Do not skip Phase 2 (questions) or Phase 3
(title selection) unless the user explicitly tells you to use your best judgement.
```

- [ ] **Step 3: Verify**

Run: `grep -c 'six phases' SKILL.md && grep -c 'social media and chat promo posts' SKILL.md`
Expected: `1` then `1`.

- [ ] **Step 4: Commit**

```bash
git add SKILL.md
git commit -m "feat: update intro and description for promo artifacts"
```

---

### Task 2: Insert Phase 3 — Craft the title

**Files:**
- Modify: `SKILL.md` (insert a new section between the end of Phase 2 and the start of the post-writing phase)

- [ ] **Step 1: Insert the new Phase 3 section**

Replace this exact block (the end of Phase 2 plus the heading that follows it):

```
Wait for the user's answers before writing. If the user explicitly says to use
your best judgement, choose sensible defaults and proceed.

## Phase 3 — Write the post
```

with:

```
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
```

- [ ] **Step 2: Verify the new phase exists and the old heading was renamed**

Run: `grep -c '## Phase 3 — Craft the title' SKILL.md && grep -c '## Phase 4 — Write the post' SKILL.md && grep -c '## Phase 3 — Write the post' SKILL.md`
Expected: `1` then `1` then `0`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add Phase 3 craft-the-title step"
```

---

### Task 3: Adapt the write-post phase (now Phase 4)

**Files:**
- Modify: `SKILL.md` (slug bullet references the chosen title; add a no-emoji writing rule)

- [ ] **Step 1: Point the slug rule at the chosen title**

Replace this exact line:

```
- `<title-as-slug>` is the post title, lowercased, with non-alphanumeric runs
```

with:

```
- `<title-as-slug>` is the chosen title (from Phase 3), lowercased, with non-alphanumeric runs
```

- [ ] **Step 2: Add a no-emoji rule to the writing rules**

Replace this exact block:

```
- **Ground every claim** in what the repo actually shows (docs, commits, code).
  Do not invent features, results, or history.
```

with:

```
- **Ground every claim** in what the repo actually shows (docs, commits, code).
  Do not invent features, results, or history.
- **No emojis.** The blog post never uses emojis. (Emojis are allowed only in the
  social and chat artifacts written in Phase 5.)
```

- [ ] **Step 3: Verify**

Run: `grep -c 'The blog post never uses emojis' SKILL.md && grep -c 'chosen title (from Phase 3)' SKILL.md`
Expected: `1` then `1`.

- [ ] **Step 4: Commit**

```bash
git add SKILL.md
git commit -m "feat: tie post writing to chosen title and forbid emojis"
```

---

### Task 4: Insert Phase 5 — Write the promotional artifacts

**Files:**
- Modify: `SKILL.md` (insert a new section between the end of the post phase's "Section guidance" and the Report phase)

- [ ] **Step 1: Insert the new Phase 5 section**

Replace this exact block (the end of "Section guidance" plus the Report heading that follows it):

```
- **How Should You Adopt It?** (optional) — concrete first steps.

## Phase 4 — Report
```

with:

````
- **How Should You Adopt It?** (optional) — concrete first steps.

## Phase 5 — Write the promotional artifacts

After the blog post file is written, create two sibling files that share the
post's date and slug. Both reuse the title chosen in Phase 3 and link to the post
with the literal placeholder `{{POST_URL}}` — do not ask the user for a URL; they
replace the placeholder at publish time.

### Social media post — `./blog/YYYY-MM-DD-<slug>.social.md`

For a professional, external audience where appearances matter. Write it in this
order:

1. The title (same string as the post).
2. **One or two sentences that lead with the problem statement** — the hook.
3. A short, **factual announcement and summary** of what the post covers.
4. One line on **what the reader gains** (the benefit).
5. A call to action: `Read it here → {{POST_URL}}`

Tone: factual and professional. Emojis are allowed but must be **tasteful —
1–3, functional not decorative**.

### Chat message — `./blog/YYYY-MM-DD-<slug>.chat.md`

For a casual, internal audience. Use the same backbone (title, problem hook,
summary, benefit, and the `{{POST_URL}}` call to action) but in a relaxed,
internal-community voice (for example, opening with "Just shipped a write-up
on…"). Emojis are allowed and can be a little more relaxed, but still tasteful.

Write plain markdown only — no Slack-specific or platform-specific markup — so it
pastes into any comms tool.

## Phase 6 — Report
````

- [ ] **Step 2: Verify both promo sub-sections and the renumbered Report heading exist**

Run: `grep -c '.social.md' SKILL.md && grep -c '.chat.md' SKILL.md && grep -c '{{POST_URL}}' SKILL.md && grep -c '## Phase 6 — Report' SKILL.md`
Expected: `1` then `1` then `3` then `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add Phase 5 social and chat artifact templates"
```

---

### Task 5: Expand the Report phase (now Phase 6)

**Files:**
- Modify: `SKILL.md` (report all three files)

- [ ] **Step 1: Replace the Report body**

Replace this exact block:

```
After writing the file, tell the user:

- The output path.
- The computed reading time and word count.
- The chosen angle and audience scope.

Offer to revise (length, tone, or focus) if they want changes.
```

with:

```
After writing all three files, tell the user:

- The three output paths (post, social, chat).
- The chosen title.
- The computed reading time and word count of the post.
- The chosen angle and audience scope.

Offer to revise any of the three artifacts (title, length, tone, or focus) if they
want changes.
```

- [ ] **Step 2: Verify**

Run: `grep -c 'The three output paths' SKILL.md && grep -c 'After writing all three files' SKILL.md`
Expected: `1` then `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: report all three artifacts in Phase 6"
```

---

### Task 6: Update `README.md`

**Files:**
- Modify: `README.md` ("What it does" list + a note on emojis and the URL placeholder)

- [ ] **Step 1: Replace the "What it does" body and the post-structure note**

Replace this exact block:

```
When invoked inside a repository, the skill:

1. Investigates `./docs`, `./notes`, the git commit history, and the codebase.
2. Asks one focused round of questions (angle, audience, the struggle to center
   on, whether to include adoption sections, and length).
3. Writes a structured markdown post to `./blog/YYYY-MM-DD-<title-as-slug>.md`
   with YAML frontmatter and a visible reading-time / audience header.
4. Reports the output path, reading time, word count, angle, and audience.

The post follows: Problem Statement → Solution → Considerations Behind the
Solution → Conclusions → (optional) Why / How Should You Adopt It?
```

with:

```
When invoked inside a repository, the skill:

1. Investigates `./docs`, `./notes`, the git commit history, and the codebase.
2. Asks one focused round of questions (angle, audience, the struggle to center
   on, whether to include adoption sections, and length).
3. Proposes 2–3 catchy titles and lets you pick or edit one.
4. Writes a structured markdown post to `./blog/YYYY-MM-DD-<title-as-slug>.md`
   with YAML frontmatter and a visible reading-time / audience header.
5. Writes two matching promotional artifacts beside it — a social media post
   (`.social.md`) and a chat message (`.chat.md`).
6. Reports the three output paths, the title, reading time, word count, angle,
   and audience.

The post follows: Problem Statement → Solution → Considerations Behind the
Solution → Conclusions → (optional) Why / How Should You Adopt It?

The blog post never uses emojis; the social and chat posts may use a few tasteful
ones. Both promo posts link to the article with a `{{POST_URL}}` placeholder that
you replace at publish time.
```

- [ ] **Step 2: Verify**

Run: `grep -c 'two matching promotional artifacts beside it' README.md && grep -c '{{POST_URL}}' README.md`
Expected: `1` then `1`.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "docs: describe three artifacts and URL placeholder"
```

---

### Task 7: End-to-end dry run against a sample repo

**Files:**
- None modified (verification only; generated output under the fixture is discarded).

- [ ] **Step 1: Create a throwaway sample repo**

```bash
rm -rf /tmp/bpc-fixture && mkdir -p /tmp/bpc-fixture && cd /tmp/bpc-fixture && git init -q
printf '# Widget\n\nA tiny CLI that caches API responses to disk.\n' > README.md
mkdir docs && printf 'We struggled with slow repeated API calls. We added a disk cache keyed by request hash.\n' > docs/notes.md
git add -A && git commit -q -m "feat: add disk cache to avoid slow repeated API calls"
git commit -q --allow-empty -m "fix: handle cache miss when hash file is corrupted"
```

- [ ] **Step 2: Run the skill against the fixture**

In a Claude session whose working directory is `/tmp/bpc-fixture`, invoke the
blogpost-creator skill. Answer Phase 2 (angle: the disk cache; audience:
practitioners; adoption sections: yes; length: Medium). In Phase 3, choose one of
the proposed catchy titles. Let it write all three files in Phase 5.

- [ ] **Step 3: Verify three sibling files with the same slug exist**

Run:
```bash
ls /tmp/bpc-fixture/blog/
```
Expected: three files — `YYYY-MM-DD-<slug>.md`, `YYYY-MM-DD-<slug>.social.md`,
`YYYY-MM-DD-<slug>.chat.md` — all sharing the same date and `<slug>`.

- [ ] **Step 4: Verify the blog post has no emojis and the title matches everywhere**

Run:
```bash
# Title line of the post:
grep -m1 '^# ' /tmp/bpc-fixture/blog/*[!l].md 2>/dev/null
# The post body must contain NO emoji. This pattern matches common emoji ranges; expect no output:
grep -P '[\x{1F000}-\x{1FAFF}\x{2600}-\x{27BF}]' /tmp/bpc-fixture/blog/*.md | grep -v '.social.md' | grep -v '.chat.md' || echo "POST EMOJI-FREE OK"
```
Expected: the post's `# Title` prints; the emoji check prints `POST EMOJI-FREE OK`
(no emoji in the post file). Confirm by eye that the same title string appears at
the top of the `.social.md` and `.chat.md` files.

- [ ] **Step 5: Verify both promo files have the CTA placeholder**

Run:
```bash
grep -c '{{POST_URL}}' /tmp/bpc-fixture/blog/*.social.md
grep -c '{{POST_URL}}' /tmp/bpc-fixture/blog/*.chat.md
```
Expected: each prints `1` (or more) — the `{{POST_URL}}` placeholder is present in
both promo files.

- [ ] **Step 6: Read all three files and confirm tone**

Read the three files. Confirm: the post is fact-based, simple English, emoji-free,
and grounded only in the fixture's disk-cache + corrupted-hash facts. The social
post leads with the problem, is professional, has at most a few tasteful emojis,
and ends with the CTA. The chat message is more casual, also with the CTA. The
title is identical across all three. If any rule is violated, fix `SKILL.md` and
repeat from Step 2.

- [ ] **Step 7: Clean up the fixture**

```bash
rm -rf /tmp/bpc-fixture
```

No commit (verification only).

---

## Self-Review

**Spec coverage** (against `docs/superpowers/specs/2026-06-12-blogpost-creator-promo-artifacts-design.md`):

- New flow with six phases / two new steps → Tasks 1 (intro), 2 (Phase 3), 4 (Phase 5), 5 (Phase 6 renumber).
- Phase 3 title: propose 2–3, user picks/edits, reused identically, title rules (surprising statement/question, problem→single solution, success not failure, truthful-not-clickbait guardrail, no emojis in title) → Task 2.
- Blog post emoji-free + uses chosen title → Task 3.
- Phase 5 social post path + structure (title, problem hook, factual summary, benefit, CTA) + tasteful emoji rule → Task 4.
- Phase 5 chat message path + casual voice + same backbone + platform-agnostic + tasteful emoji → Task 4.
- `{{POST_URL}}` placeholder, no prompt → Task 4.
- Sibling files share date+slug → Task 4 (paths) + verified Task 7 Step 3.
- Phase 6 reports three paths + title → Task 5.
- README updated (three artifacts + placeholder) → Task 6.
- Behavioral verification → Task 7.

No spec requirement is left without a task.

**Placeholder scan:** `{{POST_URL}}` and `<slug>` / `YYYY-MM-DD` are intentional output placeholders in the skill's own templates, not plan gaps. No "TBD"/"add error handling"-style gaps in the delivered content.

**Type/name consistency:** Phase numbers are consistent after renumbering — Phase 3 (Craft the title), Phase 4 (Write the post), Phase 5 (promo artifacts), Phase 6 (Report); Task 2 Step 2 and Task 4 Step 2 assert the old "## Phase 3 — Write the post" and "## Phase 4 — Report" headings are gone/renamed. File suffixes `.social.md` and `.chat.md`, and the placeholder `{{POST_URL}}`, are spelled identically in the spec, Task 4, Task 6, and the Task 7 checks.
