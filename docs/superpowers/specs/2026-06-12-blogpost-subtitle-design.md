# Design: blog post subtitle

**Date:** 2026-06-12
**Status:** Approved
**Extends:** `docs/superpowers/specs/2026-06-11-blogpost-creator-design.md` and
`docs/superpowers/specs/2026-06-12-blogpost-creator-promo-artifacts-design.md`

## Summary

Add a **subtitle** (a "deck") to the blog post: a short factual summary of what the
post is about, shown next to the title. It appears both as a YAML frontmatter field
and as a visible italic line under the title. The subtitle is a blog-post element
only; the social and chat artifacts are unchanged.

## Goals

- Give the blog post a one-or-two-sentence summary directly under the title so a
  reader knows what the post is about at a glance.
- Make the subtitle available both as structured metadata (frontmatter) and as
  visible text.

## Non-Goals

- No subtitle in the social or chat artifacts; their existing problem-led hook and
  summary already serve that role.
- The subtitle is not a second headline. The catchy/clickbait title rules do not
  apply to it.

## Requirements

**Content rules.** The subtitle is:

- **At most 2 sentences.**
- A **plain factual summary** of what the post covers — not a hook, not persuasive,
  not clickbait.
- **Simple English**, consistent with the post's writing rules.
- **Emoji-free**, like the rest of the blog post.

**Placement.** In the blog post only:

- A `subtitle:` field in the YAML frontmatter, placed immediately after `title:`.
- A visible **italic** line containing the same text, directly under the `# Title`
  heading and above the existing `*<N> min read · For <audience>*` line.

The frontmatter `subtitle` and the visible italic line use the same text.

**When crafted.** The subtitle is generated in Phase 4 (writing the post), since it
summarizes the post's content. Unlike the title, it is auto-generated rather than
chosen from options; the user can revise it via the Phase 6 report.

## Output template (blog post)

```markdown
---
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

## Problem Statement
...
```

## Documentation

- Update `SKILL.md` Phase 4: add `subtitle` to the frontmatter template, add the
  visible italic subtitle line to the body template, and add a short subtitle rule
  to the writing rules.
- Update `README.md`: note that the post includes a subtitle summarizing it.

## Edge cases

- The subtitle is required for every post (there is always something to summarize);
  it is not optional.
- All prior post rules still apply: reading time, six sections, grounding, no
  emojis, etc.
