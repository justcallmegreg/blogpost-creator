# Design: Deepened Phase 5 — Promotional Artifacts

**Date:** 2026-07-08
**Status:** Approved, ready for implementation plan
**Scope:** Rewrite Phase 5 of the `blogpost-creator` skill (`SKILL.md`) in place, plus one small addition to Phase 2.

## Problem

`blogpost-creator` already emits `.social.md` and `.chat.md` in Phase 5, but the
guidance is thin: social is a rigid 5-line backbone and chat is "the same
backbone, but relaxed." Neither captures the author's intended voice — a
**factual, professor-like excitement about sharing something genuinely cool that
a reader can learn** — nor the requirements for interest-tickling, engagement,
controlled emoji use, and accessibility to a non-expert reader.

This design replaces the Phase 5 guidance with two properly-specified voices, a
shared set of principles, and a mandatory accessibility finishing pass. It also
adds one optional question to Phase 2.

## Non-goals

- No new standalone skill. The work deepens Phase 5 **in place** (decided during
  brainstorming).
- No changes to Phases 1, 3, 4, 6, the repository-URL handling, or the
  `{{POST_URL}}` placeholder mechanism, except the single Phase 2 addition below.
- The scientific-method arc is a **framing lens only**; it never becomes a
  visible template with exposed step names.

## Shared DNA (applies to both artifacts)

1. **Voice: factual professor-excitement.** Enthusiasm comes from the idea being
   genuinely interesting, never from hype adjectives. Ban praise/hype words such
   as "amazing", "game-changer", "revolutionary", "you'll love". Excitement is
   carried by the framing of the puzzle, not by adjectives.
2. **Framing lens, not a template.** Cast the project as a small investigation —
   *here was a puzzle → here's what turned out to actually work* — phrased
   naturally. Never expose the literal scientific-method step names
   (hypothesis, experiment, model, validation, theory).
3. **Promise transferable understanding.** The hook and payoff must point at
   something generalizable the reader learns ("how X actually works"), not
   project-specific trivia. This delivers the "fundamental knowledge, not niche
   stuff" message.
4. **Concrete over abstract.** The hook leads with a specific tension or detail,
   not a generic statement.
5. **Consistency and grounding.** Both artifacts reuse the Phase 3 title
   verbatim, link via `{{POST_URL}}`, honor the optional repository line, and
   ground every claim in what the post/repo actually shows. No invented results.

## Social post — `./blog/YYYY-MM-DD-<slug>.social.md`

For a professional, external audience where appearances matter.

**Length:** ~120–200 words, 2–3 short paragraphs.

**Structure:**
1. **Hook — curiosity gap.** A genuine question or surprising tension the post
   resolves; something the reader recognizes.
2. **The turn.** The one core insight — what turned out to be true / what worked,
   framed as a small finding.
3. **Payoff + soft invite.** What the reader will now understand or be able to
   do, then the call to action: `Read it here → {{POST_URL}}`.

**Constraints:**
- Emoji: **0–2, functional only.** Never clustered, never decorative.
- Engagement is **pull**, not push — no explicit "comment below" style prompts.
- Hashtags: include 2–3 targeted hashtags **only if** the user opted in during
  Phase 2; otherwise none.
- Optional repository line (existing behavior): `Repo: <url>` after the call to
  action, only if the user opted in.

## Chat message — `./blog/YYYY-MM-DD-<slug>.chat.md`

For a casual, internal audience.

**Length:** ~50–90 words, 3–5 punchy lines.

**Structure:**
1. **Warm opener + compressed hook** (e.g. "Just dug into something neat…").
2. **The cool bit.** The core insight in one plain line.
3. **Invite + one light nudge.** `Read it here → {{POST_URL}}` plus a single
   discussion prompt (for example, "Ever hit this? Curious how others handle
   it.").

**Constraints:**
- Emoji: **2–4 as punctuation.** Never clustered.
- Engagement is **pull + one light nudge** (the single discussion prompt above).
- Optional casual repository line (existing behavior), e.g. `Code's here: <url>`,
  only if the user opted in.
- Plain markdown only — no Slack-specific or platform-specific markup.

## Accessibility finishing pass (runs last, on both artifacts)

Before writing the files, apply two checks to each artifact:

1. **Analogy / plain restatement.** The single core concept must be expressed in
   everyday terms — an analogy or a plain restatement — so a reader with zero
   background understands it.
2. **Jargon strip.** Remove or plainly define any term an average reader would
   not know. Keep at most one unavoidable domain term, defined inline in a few
   words.

**Guardrail:** this pass clarifies; it must not delete the specific insight or
flatten the excitement. If simplifying would remove the core finding, rephrase
rather than cut.

## Phase 2 addition

Add one line to Phase 2's existing single consolidated question round:

> **Hashtags** — add 2–3 targeted hashtags to the social post? (default: no)

- Social-only; the chat message never uses hashtags.
- Stays inside the same single message as the other Phase 2 questions — do not
  introduce a second question round.
- If the user opts out or does not answer, add no hashtags.

## Acceptance criteria

- Phase 5 of `SKILL.md` is rewritten to specify the shared DNA, the social post,
  the chat message, and the accessibility pass as above.
- The social post section specifies the 3-part structure, ~120–200 word length,
  0–2 functional emojis, pull-only engagement, and conditional hashtags.
- The chat message section specifies the 3-part structure, ~50–90 word length,
  2–4 punctuation emojis, and the single discussion nudge.
- The accessibility pass and its guardrail are stated as a required last step for
  both artifacts.
- Phase 2 gains the hashtag opt-in line inside its existing question round; no
  second round is added.
- Existing behavior (title reuse, `{{POST_URL}}`, repository line opt-in,
  filename/collision-suffix rules) is preserved.
