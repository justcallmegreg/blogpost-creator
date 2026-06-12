# Design: optional repository URL in artifacts

**Date:** 2026-06-12
**Status:** Approved
**Extends:** the prior blogpost-creator specs in this directory.

## Summary

Add an opt-in step so the skill can include the git repository's `origin` URL in
all three artifacts: the blog post (frontmatter field plus a visible footer line)
and the social and chat posts (a visible link line). The question is only asked
when an `origin` remote exists.

## Goals

- Let the user choose, per run, whether to surface the repository URL in the
  generated artifacts.
- Use a browsable HTTPS URL even when the remote is configured over SSH.
- Add nothing when the user declines or when there is no `origin` remote.

## Non-Goals

- No support for remotes other than `origin`.
- No publishing; the URL is only written into the markdown.

## Requirements

### Capture and normalize (Phase 1)

- During Phase 1, capture the origin URL with `git remote get-url origin`.
- **Normalize to a browsable HTTPS URL:**
  - `git@github.com:user/repo.git` → `https://github.com/user/repo`
  - `https://github.com/user/repo.git` → `https://github.com/user/repo`
  - Strip a trailing `.git`. Leave an already-HTTPS URL otherwise unchanged.
- If `git remote get-url origin` fails or returns nothing (no origin remote, or not
  a git repo), record that there is no repository URL.

### The opt-in question (Phase 2)

- The Phase 2 question round gains one item **only when a repository URL was
  captured**: "Add the repository URL (`<url>`) to the posts?" (yes / no).
- If there is no repository URL, this question is **omitted silently** — do not ask,
  do not add anything.

### When the user opts in

- **Blog post:**
  - Add a `repository: <url>` field to the YAML frontmatter (after `tags`).
  - Add a visible footer line as the last line of the post: `Source: <url>`.
- **Social post:** add a visible `Repo: <url>` line immediately after the
  `Read it here → {{POST_URL}}` call-to-action line.
- **Chat message:** add a visible repository line in casual voice after the chat
  call-to-action, for example `Code's here: <url>`.

### When the user opts out (or no origin)

- Add nothing: no `repository` frontmatter field, no `Source:` footer, and no repo
  line in either promo artifact.

## Output examples (opted in)

Blog post frontmatter and footer:

```markdown
---
title: "<Title>"
subtitle: "<...>"
date: <YYYY-MM-DD>
reading_time: "<N> min"
audience: "<audience scope>"
tags: [<derived tags>]
repository: https://github.com/user/repo
---

# <Title>
...
Source: https://github.com/user/repo
```

Social post tail:

```
Read it here → {{POST_URL}}

Repo: https://github.com/user/repo
```

## Documentation

- Update `SKILL.md`: Phase 1 (capture + normalize the origin URL), Phase 2 (the
  conditional opt-in question), Phase 4 (frontmatter field + footer line when opted
  in), Phase 5 (repo line in both promo artifacts when opted in).
- Update `README.md`: note the optional repository URL.

## Edge cases

- **No `origin` remote / not a git repo:** the question is skipped and nothing is
  added.
- **SSH remote:** normalized to HTTPS before use.
- **User declines:** nothing added anywhere.
- The repository URL is independent of the `{{POST_URL}}` article-link placeholder;
  both can appear in a promo post (the article link and the source repo are
  different destinations).
