# Blog Post Subtitle Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a subtitle (a short factual summary of the post) to the blog post — as a YAML frontmatter field and a visible italic line under the title.

**Architecture:** Edit `SKILL.md` Phase 4 (frontmatter template, body template, writing rules) and `README.md`. The subtitle is a blog-post-only element; social and chat artifacts are untouched. Prose skill; verification is structure checks plus a real dry-run.

**Tech Stack:** Markdown only.

---

## File Structure

- `SKILL.md` — Phase 4 gains a `subtitle` frontmatter field, a visible italic
  subtitle line in the body template, and a subtitle writing rule.
- `README.md` — one line noting the post includes a subtitle.

Both files exist. All tasks are exact-string edits. Apply in order.

> **Reference:** spec at `docs/superpowers/specs/2026-06-12-blogpost-subtitle-design.md`.

---

### Task 1: Add subtitle to the Phase 4 template

**Files:**
- Modify: `SKILL.md` (the `### File template` block in Phase 4)

- [ ] **Step 1: Add the `subtitle` frontmatter field and the visible italic line**

Replace this exact block:

```
title: "<Title>"
date: <YYYY-MM-DD>
reading_time: "<N> min"
audience: "<audience scope>"
tags: [<derived tags>]
---

# <Title>

*<N> min read · For <audience scope>*
```

with:

```
title: "<Title>"
subtitle: "<One or two sentences summarizing the post.>"
date: <YYYY-MM-DD>
reading_time: "<N> min"
audience: "<audience scope>"
tags: [<derived tags>]
---

# <Title>

*<One or two sentences summarizing the post.>*

*<N> min read · For <audience scope>*
```

- [ ] **Step 2: Verify**

Run: `grep -c 'subtitle: "<One or two sentences summarizing the post.>"' SKILL.md && grep -c '\*<One or two sentences summarizing the post.>\*' SKILL.md`
Expected: `1` then `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add subtitle to blog post template"
```

---

### Task 2: Add the subtitle writing rule

**Files:**
- Modify: `SKILL.md` (the `### Writing rules` list in Phase 4)

- [ ] **Step 1: Add a subtitle rule after the no-emoji rule**

Replace this exact block:

```
- **No emojis.** The blog post never uses emojis. (Emojis are allowed only in the
  social and chat artifacts written in Phase 5.)
```

with:

```
- **No emojis.** The blog post never uses emojis. (Emojis are allowed only in the
  social and chat artifacts written in Phase 5.)
- **Subtitle.** Write a subtitle of **at most two sentences** that plainly
  summarizes what the post is about. It is a factual summary, not a second
  headline — the catchy-title rules do not apply. Use the same text in the
  frontmatter `subtitle` field and the visible italic line under the title.
```

- [ ] **Step 2: Verify**

Run: `grep -c '**Subtitle.**' SKILL.md && grep -c 'at most two sentences' SKILL.md`
Expected: `1` then `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add subtitle writing rule"
```

---

### Task 3: Update `README.md`

**Files:**
- Modify: `README.md` (the post-structure note)

- [ ] **Step 1: Add a subtitle note**

Replace this exact line:

```
The post follows: Problem Statement → Solution → Considerations Behind the
```

with:

```
The post opens with a subtitle that summarizes it in one or two sentences, then
follows: Problem Statement → Solution → Considerations Behind the
```

- [ ] **Step 2: Verify**

Run: `grep -c 'opens with a subtitle that summarizes it' README.md`
Expected: `1`.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "docs: note the post subtitle"
```

---

### Task 4: Apply the subtitle to the existing test post

**Files:**
- Modify: `blog/2026-06-12-your-repo-already-knows-what-your-blog-post-should-say.md`

This is the post generated during the last test run; it predates the subtitle and
should now carry one so the working example matches the updated skill.

- [ ] **Step 1: Add the `subtitle` frontmatter field**

In `blog/2026-06-12-your-repo-already-knows-what-your-blog-post-should-say.md`,
replace this exact line:

```
title: "Your Repo Already Knows What Your Blog Post Should Say"
```

with:

```
title: "Your Repo Already Knows What Your Blog Post Should Say"
subtitle: "A walkthrough of blogpost-creator, a Claude Code skill that reads a repository and produces a grounded blog post plus matching social and chat posts. It explains the skill's six phases and how to run it."
```

- [ ] **Step 2: Add the visible italic subtitle line under the title**

Replace this exact block:

```
# Your Repo Already Knows What Your Blog Post Should Say

*6 min read · For DevRel and technical writers*
```

with:

```
# Your Repo Already Knows What Your Blog Post Should Say

*A walkthrough of blogpost-creator, a Claude Code skill that reads a repository and produces a grounded blog post plus matching social and chat posts. It explains the skill's six phases and how to run it.*

*6 min read · For DevRel and technical writers*
```

- [ ] **Step 3: Verify (subtitle present in frontmatter and body; post still emoji-free)**

Run:
```bash
f=blog/2026-06-12-your-repo-already-knows-what-your-blog-post-should-say.md
grep -c '^subtitle: ' "$f"
grep -c '^\*A walkthrough of blogpost-creator' "$f"
grep -P '[\x{1F000}-\x{1FAFF}\x{2600}-\x{27BF}\x{2190}-\x{21FF}\x{2B00}-\x{2BFF}]' "$f" >/dev/null && echo "EMOJI FOUND" || echo "POST EMOJI-FREE OK"
```
Expected: `1`, then `1`, then `POST EMOJI-FREE OK`.

- [ ] **Step 4: Commit**

```bash
git add blog/2026-06-12-your-repo-already-knows-what-your-blog-post-should-say.md
git commit -m "docs: add subtitle to the example post"
```

---

### Task 5: End-to-end dry run against a sample repo

**Files:**
- None modified (verification only).

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
practitioners; adoption sections: yes; length: Medium), pick a title in Phase 3,
and let it write all three files.

- [ ] **Step 3: Verify the subtitle is present and correct**

Run:
```bash
post=$(ls /tmp/bpc-fixture/blog/*.md | grep -v social | grep -v chat)
echo "--- frontmatter subtitle ---"; grep -m1 '^subtitle: ' "$post"
echo "--- visible subtitle (italic line above reading-time) ---"; sed -n '/^# /,/min read/p' "$post"
echo "--- sentence count check (expect 1 or 2) ---"; grep -m1 '^subtitle: ' "$post" | sed 's/^subtitle: "//; s/"$//' | grep -o '[.!?]' | wc -l
```
Expected: a `subtitle:` frontmatter line exists; an italic subtitle line appears
between the `# Title` and the `*N min read · For ...*` line; the subtitle is at most
two sentences (the `[.!?]` count is 1 or 2). Confirm the frontmatter subtitle text
and the visible italic subtitle text are identical.

- [ ] **Step 4: Confirm it reads as a factual summary**

Read the post. Confirm the subtitle plainly summarizes what the post is about (not
a clickbait restatement of the title), is emoji-free, and is grounded in the
fixture. The social and chat files must NOT have gained a subtitle line. If any
rule is violated, fix `SKILL.md` and repeat from Step 2.

- [ ] **Step 5: Clean up**

```bash
rm -rf /tmp/bpc-fixture
```

No commit (verification only).

---

## Self-Review

**Spec coverage** (against `docs/superpowers/specs/2026-06-12-blogpost-subtitle-design.md`):

- `subtitle:` frontmatter field after `title:` → Task 1.
- Visible italic subtitle line under the title, above the reading-time line → Task 1.
- Same text in both places → Task 1 template + Task 2 rule ("Use the same text…").
- Content rules: ≤2 sentences, factual summary not a headline, simple English,
  emoji-free → Task 2 rule (emoji-free already enforced by the existing no-emoji
  rule).
- Crafted in Phase 4, auto-generated → Task 2 (rule lives in Phase 4).
- Social/chat unchanged → no task touches them; Task 5 Step 4 verifies absence.
- README note → Task 3.
- Example post updated → Task 4.
- Behavioral verification → Task 5.

No spec requirement is left without a task.

**Placeholder scan:** `<One or two sentences summarizing the post.>` and `<Title>`
etc. are intentional template placeholders in the skill's own output template, not
plan gaps. No "TBD"/"add X"-style gaps.

**Type/name consistency:** The field is spelled `subtitle` in the frontmatter
(Task 1), the rule (Task 2), the README (Task 3), the example post (Task 4), and
the dry-run checks (Task 5). The visible line is italic (single `*…*`) in Task 1
and Task 4, matching the existing reading-time line style.
