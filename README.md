# blogpost-creator

A Claude skill that turns a code repository into a short, fact-based,
easy-to-read blog post about what a reader will learn from it.

## What it does

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

The post opens with a subtitle that summarizes it in one or two sentences, then
follows: Problem Statement → Solution → Considerations Behind the
Solution → Conclusions → (optional) Why / How Should You Adopt It?

The blog post never uses emojis; the social and chat posts may use a few tasteful
ones. Both promo posts link to the article with a `{{POST_URL}}` placeholder that
you replace at publish time.

If the repository has an `origin` remote, the skill offers to add its URL (as a
browsable HTTPS link) to all three artifacts — a `repository` field plus a
`Source:` footer in the post, and a repo link in each promo post.

## Install

Symlink (or copy) this repo into your Claude skills directory so the skill is
discoverable:

```bash
ln -s "$(pwd)" ~/.claude/skills/blogpost-creator
```

Then invoke it from any repository by asking Claude to create a blog post for the
project (for example: "use the blogpost-creator skill on this repo").
