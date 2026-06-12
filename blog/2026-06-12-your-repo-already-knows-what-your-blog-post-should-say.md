---
title: "Your Repo Already Knows What Your Blog Post Should Say"
subtitle: "A walkthrough of blogpost-creator, a Claude Code skill that reads a repository and produces a grounded blog post plus matching social and chat posts. It explains the skill's six phases and how to run it."
date: 2026-06-12
reading_time: "6 min"
audience: "DevRel and technical writers"
tags: [claude-code, skills, devrel, technical-writing, content]
---

# Your Repo Already Knows What Your Blog Post Should Say

*A walkthrough of blogpost-creator, a Claude Code skill that reads a repository and produces a grounded blog post plus matching social and chat posts. It explains the skill's six phases and how to run it.*

*6 min read · For DevRel and technical writers*

## Problem Statement

Writing about a project, then promoting it, is two chores stacked on top of each
other.

The first chore is reconstruction. Before you can write a word, you reopen the
code, scroll the commit history, and try to recall which decisions mattered. The
material you need is already in the repository — in the docs, the notes, the commit
messages, and the code — but it is scattered, so assembling it takes real effort.
You start from a blank page even though the project has been documenting itself the
whole time.

The second chore is repetition. Once the post is written, you rewrite the same
message for each channel: a polished version for social media, a casual version for
an internal chat. Each rewrite needs its own hook and its own tone, so a single
post quietly becomes three writing tasks.

This post explains how the `blogpost-creator` skill removes both chores. It reads
the repository for you, asks a few focused questions, and produces a blog post plus
two matching promotional posts. After reading, you will know what the skill does,
how its six phases work, and how to run it.

## Solution

`blogpost-creator` is a skill for Claude Code. A skill is a set of instructions
that Claude loads on demand to perform a specific task. This one lives in a single
file, `SKILL.md`, with a `README.md` that explains how to install it.

When you run it inside a repository, it works in six phases.

**Phase 1 — Investigate.** The skill reads `./docs` and `./notes` if they exist,
reads the git commit history, and surveys the codebase: the README, the manifest
file, the structure, and a few key modules. The goal is to answer two questions —
what is this project, and what problem does it solve.

**Phase 2 — Ask one round of questions.** The skill shows what it understood, then
asks a single batch: the angle, the audience, the struggle to center on, whether to
include optional adoption sections, and the target length. It pre-fills its best
guesses, so you usually confirm rather than write answers from scratch.

**Phase 3 — Craft the title.** The skill proposes two or three catchy titles and
you pick or edit one. That single title is reused, identically, across all three
artifacts. The rules keep it honest: it should name a problem and promise a single
solution, frame a success rather than a failure, and stay a promise the post
actually delivers rather than tipping into clickbait.

**Phase 4 — Write the post.** The skill writes a markdown file to
`./blog/YYYY-MM-DD-<title-as-slug>.md`. It opens with structured metadata (title,
date, reading time, audience, tags) and a visible header, then follows a fixed
shape: Problem Statement, Solution, Considerations Behind the Solution,
Conclusions, and two optional adoption sections. The post never uses emojis.

**Phase 5 — Write the promotional artifacts.** Beside the post, the skill writes
two sibling files that share its date and title. The social media post
(`.social.md`) is professional: it leads with the problem, summarizes the post,
states the benefit, and ends with a call to action. The chat message (`.chat.md`)
carries the same backbone in a casual, internal voice. Both may use a few tasteful
emojis, and both link to the article with a `{{POST_URL}}` placeholder you replace
at publish time.

**Phase 6 — Report.** The skill tells you the three file paths, the chosen title,
the reading time, the word count, and the angle and audience it used.

## Considerations Behind the Solution

Several design choices shape how the skill behaves.

**The repository is the only source.** The writing rules instruct the skill to base
every claim on what the repository actually shows and to invent nothing. This is
the direct answer to AI drafts that hallucinate features. It is also why the title
of this post is fair: the material really is already in the repo.

**One title, reused everywhere.** Rather than writing a separate headline per
channel, the skill settles one title and carries it across the post, the social
post, and the chat message. This keeps the message consistent and makes the title
the single most important decision, which is why it gets its own phase and your
direct input.

**Emojis are split by channel.** The blog post is timeless reference material, so
it stays emoji-free to protect its authority. The social and chat posts are
scannable announcements, so they allow a few tasteful emojis. The rule is explicit
rather than left to taste.

**One question round, not a conversation.** The skill investigates on its own, then
asks once. This avoids a fully autonomous tool that guesses the angle wrong and a
fully conversational tool that interrupts repeatedly.

**Plain markdown output.** All three artifacts are markdown files. You can paste
them into a static site generator, a CMS, a scheduler, or a chat tool without
conversion, and the chat message uses no platform-specific markup so it works
anywhere.

The skill also handles thin inputs. If there is no `./docs` or `./notes`, it relies
on the git history and code and notes that the evidence is thinner. If the history
is shallow or absent, it says so. If the directory is not a git repository, it
works from the docs, notes, and code. When the adoption sections do not apply, it
omits them cleanly.

## Conclusions

`blogpost-creator` turns two chores — reconstructing a project and rewriting its
announcement for each channel — into one short, guided pass. It reads the
repository, settles a title with you, and produces a grounded post plus a social
post and a chat message that share that title. You still edit and shape the result,
but you start from drafts instead of a blank page.

The ideas worth taking away: a draft built from a repository should be grounded in
that repository and nothing else, and one shared title plus a fixed structure can
produce a whole channel set without three separate writing tasks.

## Why Should You Adopt It?

If you write and promote developer content, the skill removes the blank-page step
and the per-channel rewrite at the same time. Because it draws only on the
repository, the drafts reflect the real project, which means less time spent
checking invented claims. Because all three artifacts share one title and one set
of facts, your message stays consistent across channels. And because everything is
plain markdown, it fits whatever publishing and scheduling tools you already use.

## How Should You Adopt It?

Install the skill by linking it into your Claude Code skills directory:

```bash
ln -s "$(pwd)" ~/.claude/skills/blogpost-creator
```

Then open a repository you want to write about and ask Claude to use the
blogpost-creator skill on it. Answer the single round of questions, pick a title
from the ones it proposes, and let it write the post and the two promo files to
`./blog/`. Edit the drafts as you would any first draft, replace the `{{POST_URL}}`
placeholder with the live link, and publish through your usual tooling.
