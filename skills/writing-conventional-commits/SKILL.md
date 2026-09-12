---
name: writing-conventional-commits
description: Use when the user asks to git commit, write or generate a commit message, follow Conventional Commits, Angular commit convention, commitlint, or type(scope): subject format; when drafting feat/fix/docs/style/refactor/perf/test/build/ci/chore/revert headers or BREAKING CHANGE footers; when about to write a 1-2 sentence prose commit; or when about to run git commit in the same turn without showing the message for confirmation.
---

# Writing Conventional Commits

## Overview

Commit messages use **Conventional Commits 1.0.0** (the spec Angular’s format evolved into). `@commitlint/config-conventional` is the usual linter.

**This skill replaces the default 1-2 sentence prose message.** Keep git safety: commit only when asked; HEREDOC; no agent attribution; no `--force` / skipped hooks.

Header = what (imperative). Body = why, and only when the summary is not enough.

## Confirm then commit

When the user asked to commit (`提交` / `commit` / `帮我提交`):

1. Inspect the diff. Decide type, split, and message yourself. Do not quiz them (`feat` vs `fix`, split vs not).
2. Show the exact commit message(s) that would run. If splitting, list each message in order.
3. **Stop.** Wait for an explicit go-ahead (`确认` / `OK` / `可以` / `提交` / an edited message).
4. Only then run `git commit` with HEREDOC.

Do not run `git commit` in the same turn as the first request. Impatience, “just commit”, and “don't stall” do not skip step 3.

If they reply with an edited message, use it (keep Conventional Commits shape unless they named another format).

## When NOT to Use

- Repo/user requires another template (WPS `[项目] 影响面@ <WebBug:>…`, custom commitlint)
- User names a different format for this commit
- User did not ask to commit

## Message contract

```
<type>[optional scope][optional !]: <description>

[optional body]

[optional footer(s)]
```

| Part | Required | Rule |
|------|----------|------|
| type | yes | Recommended set below. Never `update`/`add`/`wip`/`hotfix`/`bugfix`. |
| scope | no | Area in **this** repo (`auth`). Omit if cross-cutting. |
| `!` | if breaking | Immediately before `:`. |
| description | yes | Imperative ("add" not "added"). No first capital. No trailing `.`. Header ≤100 chars. |
| body | no | Use when why / old vs new is not obvious from the description. Wrap 100. |
| footer | no | `Fixes #n` / `Closes #n`. Breaks: below. |

Blank line between header, body, and footer when those parts exist.

A header-only commit is valid. Do not pad a body to hit a character minimum.

## Types

| Type | When |
|------|------|
| **feat** | New user-facing capability |
| **fix** | Bug fix |
| **refactor** | Same behavior, different structure |
| **perf** | Same behavior, faster |
| **docs** | Documentation only |
| **style** | Whitespace/formatting only |
| **test** | Tests only |
| **build** | Build system or dependencies |
| **ci** | CI config/scripts |
| **chore** | None of the above (e.g. `.gitignore`) |
| **revert** | Reverts a prior commit |

One type per commit. First match: docs-only → style-only → test-only → fix → feat → perf → build/ci → refactor → chore.

## Split mixed types

More than one type in the tree → **multiple commits**, stage by path. Propose the split in the confirmation; do not ask “要不要拆？” as an open question.

"马上发版" / "别写太长" / "one shot" does **not** mix `feat`+`fix`+`docs`+`style`.

User already forbade splitting (`不要拆` / `就一个 commit`) → one commit, primary type (`feat` > `fix` > rest), other work in the body. Never `feat,fix:`.

## Breaking changes

A public API/behavior break needs `!` on the header. A PR title is not a substitute.

```
feat(auth)!: replace getUser with getUserById
```

Add a `BREAKING CHANGE:` footer when callers need migration steps. `!` alone is enough when the description already states the break.

```
feat(auth)!: replace getUser with getUserById

BREAKING CHANGE: `getUser(id)` is removed.
Call `getUserById(id, { includeDeleted: false })` instead.
```

Revert:

```
revert: feat(auth): add JWT login

Refs: <sha>
```

## Example

`.gitignore` only → show this header, then wait; after go-ahead use HEREDOC:

```
chore: add .env to gitignore
```

JWT login + README → show this, then wait:

```
1. feat(auth): add JWT login middleware
2. docs: mention login in the README
```

Do not run `git commit` until they confirm.

Follow `git log` language for description/body (Chinese is fine). Keep `type` and `BREAKING CHANGE` in English.

## Rationalizations

| Excuse | Reality |
|--------|---------|
| "Git rule says 1-2 sentences / focus on why" | This skill replaces that shape. Why, if needed, is the **body**. |
| "Angular requires a body ≥20 chars" | That was the old convention. Body is optional here. |
| "They already said 提交; confirmation is redundant" | First “提交” means prepare the message. Second go-ahead means run git. |
| "Impatient / just commit / don't stall" | Show the message. Waiting one reply is the confirmation. |
| "Quiz feat vs fix / 要不要拆？" | Decide yourself, then show the plan for confirmation. |
| "One shot / 别写太长" | Short **description**. Mixed types still split. |
| "Senior said skip `!`; PR title covers it" | The commit must carry `!`. |
| "chore covers everything" | Extract = `refactor`. Whitespace = `style`. New endpoint = `feat`. |

## Common mistakes / red flags — rewrite before `git commit`

- Running `git commit` before they confirm the shown message
- Quizzing type/split instead of proposing the exact messages
- Prose header (`Enable JWT login…`, `Update auth API`)
- Capitalized description or trailing period (`feat: Add login.`)
- Invented types or `feat,fix:`
- Mixed types in one commit without an explicit user forbid-split
- Breaking API change without `!`
- Body that only restates the header
- Agent attribution trailers
