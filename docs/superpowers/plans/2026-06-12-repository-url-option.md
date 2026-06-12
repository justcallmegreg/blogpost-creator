# Optional Repository URL Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Let the skill optionally include the git `origin` URL (normalized to HTTPS) in the blog post, social post, and chat message, asked once in Phase 2 and only when an origin remote exists.

**Architecture:** Edit `SKILL.md` across Phases 1, 2, 4, and 5 to capture/normalize the URL, ask the opt-in question, and conditionally add the URL to each artifact. Update `README.md`. Prose skill; verification is structure checks plus a dry-run.

**Tech Stack:** Markdown only.

---

## File Structure

- `SKILL.md` — Phase 1 captures + normalizes the origin URL; Phase 2 adds the
  conditional opt-in question; Phase 4 adds a conditional frontmatter field +
  footer; Phase 5 adds a conditional repo line to both promo files.
- `README.md` — note the optional repository URL.

Both files exist. All tasks are exact-string edits; apply in order.

> **Reference:** spec at `docs/superpowers/specs/2026-06-12-repository-url-option-design.md`.

---

### Task 1: Phase 1 — capture and normalize the origin URL

**Files:**
- Modify: `SKILL.md` (add a step to Phase 1's numbered list)

- [ ] **Step 1: Add a "Repository URL" investigation step**

Replace this exact block:

```
3. **Codebase survey.** Read the README, the package/manifest file (e.g.
   `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`), the top-level
   directory structure, and a few key modules. Aim to answer: what is this
   project, and what problem does it solve?
```

with:

```
3. **Codebase survey.** Read the README, the package/manifest file (e.g.
   `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`), the top-level
   directory structure, and a few key modules. Aim to answer: what is this
   project, and what problem does it solve?
4. **Repository URL.** Run `git remote get-url origin`. If it returns a URL,
   normalize it to a browsable HTTPS form for later use: convert
   `git@host:user/repo.git` to `https://host/user/repo`, and strip any trailing
   `.git`. If the command fails or returns nothing, there is no repository URL —
   note that and move on.
```

- [ ] **Step 2: Verify**

Run: `grep -c '4. \*\*Repository URL.\*\*' SKILL.md && grep -c 'git remote get-url origin' SKILL.md`
Expected: `1` then `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: capture and normalize origin URL in Phase 1"
```

---

### Task 2: Phase 2 — add the conditional opt-in question

**Files:**
- Modify: `SKILL.md` (add a question to the Phase 2 list)

- [ ] **Step 1: Add question 6**

Replace this exact block:

```
5. **Length target.** Confirm the target. Default: Medium, 5–8 minutes
   (~1000–1600 words). The user may choose shorter or longer.

Wait for the user's answers before writing. If the user explicitly says to use
your best judgement, choose sensible defaults and proceed.
```

with:

```
5. **Length target.** Confirm the target. Default: Medium, 5–8 minutes
   (~1000–1600 words). The user may choose shorter or longer.
6. **Repository URL — only if one was found in Phase 1.** Ask whether to add the
   repository URL (`<url>`) to the posts. If Phase 1 found no `origin` URL, omit
   this question entirely.

Wait for the user's answers before writing. If the user explicitly says to use
your best judgement, choose sensible defaults and proceed.
```

- [ ] **Step 2: Verify**

Run: `grep -c '6. \*\*Repository URL — only if one was found in Phase 1.\*\*' SKILL.md`
Expected: `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add conditional repository-URL opt-in question"
```

---

### Task 3: Phase 4 — conditional frontmatter field and footer

**Files:**
- Modify: `SKILL.md` (add a subsection after the File template block)

- [ ] **Step 1: Insert the repository-link subsection just before the Reading time subsection**

To avoid touching the template's closing code fence, anchor on the start of the
"Reading time" subsection. Replace this exact block (the heading plus its first
line):

> ### Reading time
>
> After drafting, count the words in the body and set `<N> = round(word_count /

with:

> ### Repository link — only if the user opted in
>
> If the user agreed in Phase 2 to add the repository URL:
>
> - Add a `repository: <url>` line to the YAML frontmatter, immediately after the
>   `tags` line.
> - Add a visible footer as the **last line** of the post: `Source: <url>`.
>
> If the user declined, or no `origin` URL was found, add neither.
>
> ### Reading time
>
> After drafting, count the words in the body and set `<N> = round(word_count /

(The `>` markers above are quoting for this plan only — do not include them in the
file. Everything else, including indentation and the trailing `round(word_count /`
fragment, must be matched and written verbatim.)

- [ ] **Step 2: Verify**

Run: `grep -c '### Repository link — only if the user opted in' SKILL.md && grep -c 'Source: <url>' SKILL.md`
Expected: `1` then `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add conditional repository field and footer to the post"
```

---

### Task 4: Phase 5 — conditional repo line in promo artifacts

**Files:**
- Modify: `SKILL.md` (add a paragraph to the Phase 5 intro)

- [ ] **Step 1: Add the repo-line instruction after the Phase 5 intro paragraph**

Replace this exact block:

```
reuse the title chosen in Phase 3 and link to the post with the literal
placeholder `{{POST_URL}}` — do not ask the user for a URL; they replace the
placeholder at publish time.
```

with:

```
reuse the title chosen in Phase 3 and link to the post with the literal
placeholder `{{POST_URL}}` — do not ask the user for a URL; they replace the
placeholder at publish time.

If the user opted in to the repository URL in Phase 2, also add it as a visible
line in **both** promo files, on its own line after the call to action: in the
social post write `Repo: <url>`; in the chat message use a casual equivalent such
as `Code's here: <url>`. If the user declined, add no repository line. This
repository link is separate from the `{{POST_URL}}` article link; both may appear.
```

- [ ] **Step 2: Verify**

Run: `grep -c 'in the social post write ' SKILL.md && grep -c 'Code.s here: <url>' SKILL.md`
Expected: `1` then `1`.

- [ ] **Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add conditional repo line to promo artifacts"
```

---

### Task 5: Update `README.md`

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Add a note about the optional repository URL**

Replace this exact block:

```
The blog post never uses emojis; the social and chat posts may use a few tasteful
ones. Both promo posts link to the article with a `{{POST_URL}}` placeholder that
you replace at publish time.
```

with:

```
The blog post never uses emojis; the social and chat posts may use a few tasteful
ones. Both promo posts link to the article with a `{{POST_URL}}` placeholder that
you replace at publish time.

If the repository has an `origin` remote, the skill offers to add its URL (as a
browsable HTTPS link) to all three artifacts — a `repository` field plus a
`Source:` footer in the post, and a repo link in each promo post.
```

- [ ] **Step 2: Verify**

Run: `grep -c 'offers to add its URL' README.md`
Expected: `1`.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "docs: note the optional repository URL"
```

---

### Task 6: End-to-end dry run (opt-in and opt-out)

**Files:**
- None modified (verification only).

- [ ] **Step 1: Create a fixture WITH an origin remote**

```bash
rm -rf /tmp/bpc-fixture && mkdir -p /tmp/bpc-fixture && cd /tmp/bpc-fixture && git init -q
git remote add origin git@github.com:acme/widget.git
printf '# Widget\n\nA tiny CLI that caches API responses to disk.\n' > README.md
mkdir docs && printf 'We struggled with slow repeated API calls. We added a disk cache keyed by request hash.\n' > docs/notes.md
git add -A && git commit -q -m "feat: add disk cache to avoid slow repeated API calls"
git commit -q --allow-empty -m "fix: handle cache miss when hash file is corrupted"
```

- [ ] **Step 2: Run the skill (opt IN to the repository URL)**

In a Claude session with `/tmp/bpc-fixture` as the working directory, invoke the
skill. Confirm it asks the repository-URL question in Phase 2 (the origin is an SSH
URL, so the proposed value must be the normalized `https://github.com/acme/widget`).
Answer YES. Use angle = disk cache, audience = practitioners, adoption = yes,
length = Medium, and pick a title. Let it write all three files.

- [ ] **Step 3: Verify the URL was added and normalized**

Run:
```bash
post=$(ls /tmp/bpc-fixture/blog/*.md | grep -v social | grep -v chat)
echo "=== post frontmatter repository ==="; grep -m1 '^repository: ' "$post"
echo "=== post Source footer ==="; grep -m1 '^Source: ' "$post"
echo "=== social repo line ==="; grep -m1 '^Repo: ' /tmp/bpc-fixture/blog/*.social.md
echo "=== chat repo line ==="; grep -m1 'github.com/acme/widget' /tmp/bpc-fixture/blog/*.chat.md
echo "=== no .git suffix / no SSH form anywhere ==="; grep -R 'acme/widget.git\|git@github.com' /tmp/bpc-fixture/blog/ && echo "BAD: raw/ssh URL found" || echo "URL NORMALIZED OK"
```
Expected: the post has `repository: https://github.com/acme/widget` and a
`Source: https://github.com/acme/widget` footer; the social post has a
`Repo: https://github.com/acme/widget` line; the chat file references
`github.com/acme/widget`; and the normalization check prints `URL NORMALIZED OK`
(no `.git`, no `git@` SSH form anywhere).

- [ ] **Step 4: Run the skill on a repo with NO origin (opt-out path)**

```bash
rm -rf /tmp/bpc-fixture2 && mkdir -p /tmp/bpc-fixture2 && cd /tmp/bpc-fixture2 && git init -q
printf '# Widget2\n\nA tiny CLI.\n' > README.md
git add -A && git commit -q -m "feat: initial"
```
Invoke the skill with `/tmp/bpc-fixture2` as the working directory. Confirm the
skill does NOT ask the repository-URL question (no origin remote) and that the
generated files contain no `repository:`, `Source:`, or `Repo:` lines:
```bash
grep -R '^repository: \|^Source: \|^Repo: ' /tmp/bpc-fixture2/blog/ && echo "BAD: URL leaked" || echo "NO-ORIGIN CLEAN OK"
```
Expected: `NO-ORIGIN CLEAN OK`.

- [ ] **Step 5: Clean up**

```bash
rm -rf /tmp/bpc-fixture /tmp/bpc-fixture2
```

No commit (verification only).

---

## Self-Review

**Spec coverage** (against `docs/superpowers/specs/2026-06-12-repository-url-option-design.md`):

- Capture + normalize origin URL in Phase 1 (SSH→HTTPS, strip `.git`, handle none) → Task 1.
- Conditional Phase 2 opt-in question, omitted when no origin → Task 2.
- Blog post: `repository` frontmatter field after `tags` + `Source:` footer when opted in → Task 3.
- Social post `Repo: <url>` line and chat casual repo line after the CTA when opted in; nothing when declined → Task 4.
- Repository link independent of `{{POST_URL}}` → Task 4 (explicit sentence).
- README note → Task 5.
- Behavioral verification of both opt-in (normalized) and no-origin paths → Task 6.

No spec requirement is left without a task.

**Placeholder scan:** `<url>` is the skill's own template placeholder, intentional. No "TBD"/"add X" gaps.

**Type/name consistency:** Field spelled `repository` and footer `Source:` consistently (Task 3, Task 5, Task 6 checks). Social line label `Repo:` consistent (Task 4, Task 6). Normalized URL form `https://github.com/acme/widget` (no `.git`) used consistently in Task 6.
