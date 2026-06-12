# Design: blogpost-creator promo artifacts (social + chat)

**Date:** 2026-06-12
**Status:** Approved
**Extends:** `docs/superpowers/specs/2026-06-11-blogpost-creator-design.md`

## Summary

Extend the `blogpost-creator` skill so that, after generating and finalizing the
blog post, it also produces two promotional artifacts: a **social media post**
(professional/external audience) and a **chat message** (casual/internal audience,
platform-agnostic). A new interactive title-crafting step proposes catchy titles
that are reused across all three artifacts.

## Goals

- Produce three artifacts per run: the blog post, a social media post, and a chat
  message.
- Make the title the centerpiece: catchy, problem/solution framed, success-toned,
  chosen interactively, and reused identically across all three artifacts.
- Keep the blog post emoji-free; allow tasteful emojis in social and chat bodies.
- Drive action with a call-to-action and a link placeholder in the promo posts.

## Non-Goals

- No publishing/posting to any platform. Markdown files are the handoff artifacts.
- No platform-specific markup (e.g. Slack mrkdwn). Chat output is plain markdown.
- No second question round for the promo artifacts; they reuse decisions already
  made for the post.

## Runtime flow

The existing flow gains two steps (Phase 3 title, Phase 5 promo artifacts):

```
Phase 1 — Investigate            (unchanged)
Phase 2 — One question round     (unchanged: angle, audience, struggle, adoption, length)
Phase 3 — Craft the title  ← NEW
Phase 4 — Write the blog post     (was Phase 3; uses the chosen title)
Phase 5 — Write social + chat ← NEW
Phase 6 — Report                 (was Phase 4; now reports all three files)
```

The two promo artifacts are generated automatically after the post, in the same
run. They reuse the title, angle, and audience already settled — no extra
questions.

### Phase 3 — Craft the title

The skill proposes **2–3 candidate titles**, the user picks or edits one, and that
single string becomes the title for all three artifacts.

Title rules:

- A **surprising statement or a question** that names a **problem / pain point**
  and promises a **single, simple solution**.
- Framed as a **success story**, not a failure (e.g. "How we fixed slow API
  calls", not "We struggled with API calls").
- **Guardrail — truthful, not clickbait:** the title must be a promise the post
  actually delivers. It may be compelling but must never overpromise beyond what
  the post's content supports. This protects the post's fact-focused credibility.
- **No emojis in the title.** The identical title is reused on the blog post,
  which forbids emojis, so the title itself must be emoji-free. Emojis appear only
  in the social/chat bodies.

### Phase 4 — Write the blog post

Unchanged from the base spec except that the title is the one chosen in Phase 3.
The post remains emoji-free and follows the existing template and writing rules.

### Phase 5 — Write the promo artifacts

After the post file is written, generate two sibling files sharing the post's date
and slug.

**Social media post** — `./blog/YYYY-MM-DD-<slug>.social.md`

Professional/external audience; appearances matter. Structure:

1. The title (same string as the post).
2. **1–2 sentences leading with the problem statement** (the hook).
3. A short, **factual announcement and summary** of what the post covers.
4. One line on **what the reader gains** (the benefit).
5. **Call to action:** `Read it here → {{POST_URL}}`

Tone: factual, professional. Emojis allowed but **tasteful: 1–3, functional not
decorative**.

**Chat message** — `./blog/YYYY-MM-DD-<slug>.chat.md`

Casual/internal audience. Same backbone (title, problem hook, summary, CTA with
`{{POST_URL}}`) but in a **casual, internal-community voice** (e.g. "Just shipped a
write-up on…"). Emojis allowed, slightly more relaxed but still tasteful.
Platform-agnostic plain markdown — no Slack-specific markup — so it pastes into
any comms tool.

Both files use the literal placeholder `{{POST_URL}}` for the link. The skill does
not prompt for a URL; the user replaces the placeholder at publish time.

### Phase 6 — Report

After writing all three files, report: the three file paths, the chosen title, the
computed reading time and word count of the post, and the angle/audience used.
Offer to revise any of the three.

## Output files (per run)

```
./blog/YYYY-MM-DD-<slug>.md          # the blog post (emoji-free)
./blog/YYYY-MM-DD-<slug>.social.md   # social media post (tasteful emojis)
./blog/YYYY-MM-DD-<slug>.chat.md     # chat message (casual, tasteful emojis)
```

`<slug>` is the chosen title, lowercased and hyphenated, identical across all
three filenames.

## Edge cases

- All base-spec edge cases (no docs/notes, shallow/no git, not a git repo, optional
  adoption sections) still apply to the post.
- If the user declines to pick from the proposed titles and supplies their own, use
  theirs verbatim (still apply the no-emoji-in-title rule for reuse safety).
- Title collision handling for filenames follows the base spec: if a file with the
  computed name exists, append `-2`, `-3`, etc. — applied consistently to all three
  sibling filenames so the set stays grouped.

## Documentation

- Update `SKILL.md`: renumber phases, add the Phase 3 title step and rules, add the
  Phase 5 social and chat templates and emoji rules, update Phase 6 reporting.
- Update `README.md`: state that the skill now produces three artifacts (post,
  social post, chat message) and explain the `{{POST_URL}}` placeholder.
