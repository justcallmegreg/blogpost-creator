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

```bash
ln -s "$(pwd)" ~/.claude/skills/blogpost-creator
```

Then invoke it from any repository by asking Claude to create a blog post for the
project (for example: "use the blogpost-creator skill on this repo").
