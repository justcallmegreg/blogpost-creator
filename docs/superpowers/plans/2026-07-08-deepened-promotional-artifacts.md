# Deepened Promotional Artifacts Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rewrite Phase 5 of the `blogpost-creator` skill to specify two properly-voiced promo artifacts plus an accessibility pass, and add a hashtag opt-in to Phase 2.

**Architecture:** This is a prose edit to a single Markdown skill file (`SKILL.md`). There is no code and no test runner. "Tests" are content-verification checks — `grep` assertions against the edited file and read-backs. Two edits, split into two independently-reviewable tasks: (1) the Phase 2 hashtag question, (2) the Phase 5 rewrite.

**Tech Stack:** Markdown, `grep`, `git`.

## Global Constraints

- **Single file:** all edits are to `SKILL.md` at the repo root. No other files change (example promos in `./blog/` are explicitly out of scope).
- **Preserve existing behavior:** title reuse from Phase 3, the `{{POST_URL}}` placeholder, the repository-line opt-in, and filename/collision-suffix rules must survive the rewrite unchanged.
- **Framing lens only:** the scientific-method arc is a mindset; never expose literal step names (hypothesis, experiment, model, validation, theory) in the emitted artifacts or in the instructions telling the model to expose them.
- **Line references** below (e.g. `:64-66`) reflect the file at plan-writing time and may drift; match on the quoted text, not the line number.
- **Commit style:** end commit messages with the repo's co-author trailer:
  `Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>`

---

### Task 1: Add the hashtag opt-in to Phase 2

**Files:**
- Modify: `SKILL.md` (Phase 2 question list, currently ends at item 6 "Repository URL", around `:64-66`)

**Interfaces:**
- Consumes: nothing from other tasks.
- Produces: a Phase 2 question #7 that Task 2's social-post section relies on ("include 2–3 targeted hashtags only if the user opted in during Phase 2").

- [ ] **Step 1: Add question 7 after the Repository URL item**

Find this block (the last item in the Phase 2 numbered list):

```markdown
6. **Repository URL — only if one was found in Phase 1.** Ask whether to add the
   repository URL (`<url>`) to the posts. If Phase 1 found no `origin` URL, omit
   this question entirely.
```

Insert a new item 7 immediately after it:

```markdown
7. **Hashtags.** Ask whether to add 2–3 targeted hashtags to the social media
   post. Default: no. This applies to the social post only — the chat message
   never uses hashtags. If the user opts out or does not answer, add none.
```

Do not add or split any question round — item 7 stays inside the same single
consolidated message described at the top of Phase 2.

- [ ] **Step 2: Verify the question was added and no second round was introduced**

Run:
```bash
grep -n "Hashtags" SKILL.md
grep -c "single\s*$\|single consolidated round" SKILL.md
```
Expected: the first `grep` shows the new item 7 line under Phase 2; the second confirms the Phase 2 preamble still describes a *single* consolidated round (count unchanged from before the edit — one match for "single consolidated round").

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add hashtag opt-in to Phase 2 question round

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>"
```

---

### Task 2: Rewrite Phase 5 — promotional artifacts

**Files:**
- Modify: `SKILL.md` (entire `## Phase 5 — Write the promotional artifacts` section, from its heading down to the `## Phase 6 — Report` heading)

**Interfaces:**
- Consumes: the Phase 2 hashtag opt-in from Task 1 (question #7).
- Produces: the final Phase 5 spec; Phase 6 immediately follows and is unchanged.

- [ ] **Step 1: Replace the Phase 5 section**

Replace everything from the line `## Phase 5 — Write the promotional artifacts`
up to (but not including) the line `## Phase 6 — Report` with the following:

````markdown
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
````

- [ ] **Step 2: Verify the rewrite is present and complete**

Run:
```bash
grep -n "Shared DNA\|Accessibility finishing pass\|professor-excitement\|curiosity gap\|Jargon strip" SKILL.md
```
Expected: one match for each of the five phrases, all located between the
`## Phase 5` and `## Phase 6` headings.

- [ ] **Step 3: Verify preserved behavior survived the rewrite**

Run:
```bash
grep -c "{{POST_URL}}" SKILL.md
grep -n "collision suffix\|Repo: <url>\|Code's here: <url>" SKILL.md
```
Expected: `{{POST_URL}}` still appears (2 or more matches); the collision-suffix
rule and both repository-line forms are still present in Phase 5.

- [ ] **Step 4: Verify the length and emoji ceilings are stated**

Run:
```bash
grep -n "120–200 words\|50–90 words\|0–2, functional\|2–4, as punctuation" SKILL.md
```
Expected: one match for each — the social length, chat length, social emoji
ceiling, and chat emoji ceiling.

- [ ] **Step 5: Read back the edited Phase 5**

Read `SKILL.md` from the `## Phase 5` heading through the `## Phase 6` heading.
Confirm: sections appear in order (Shared DNA → Social → Chat → Accessibility),
no leftover text from the old Phase 5 remains, and the section flows into an
unchanged `## Phase 6 — Report`.

- [ ] **Step 6: Commit**

```bash
git add SKILL.md
git commit -m "feat: deepen Phase 5 promo artifacts (voice, structure, accessibility)

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>"
```

---

## Self-Review

**Spec coverage:**
- Shared DNA (voice, framing lens, transferable understanding, concrete, grounding) → Task 2 Step 1, "Shared DNA" block. ✓
- Social post (structure, ~120–200 words, 0–2 emoji, pull engagement, conditional hashtags, repo line) → Task 2 Step 1 + Task 1 (hashtag opt-in). ✓
- Chat message (structure, ~50–90 words, 2–4 emoji, one nudge, no hashtags, plain markdown, repo line) → Task 2 Step 1. ✓
- Accessibility pass + guardrail → Task 2 Step 1, final block. ✓
- Phase 2 hashtag opt-in, no second round → Task 1. ✓
- Preserved behavior (title reuse, `{{POST_URL}}`, repo opt-in, collision suffix) → Task 2 Steps 3. ✓

**Placeholder scan:** No "TBD/TODO/handle edge cases" — every step shows the exact Markdown to insert or the exact command to run. ✓

**Type consistency:** N/A (prose). Cross-task reference: Task 1 produces the Phase 2 hashtag question that Task 2's social section consumes ("opted in to hashtags in Phase 2"); wording matches. ✓
