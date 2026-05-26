---
name: git
description: Git workflow — branch, commit, and open PRs following Conventional Commits. Use for any git operation in this repo.
user-invocable: true
---

# Git Skill

Handles git operations for the personal-ai-assistant repo: branching, staging, committing, and opening PRs.

## Input forms

- `/git` — status check: show what's changed and current branch
- `/git commit` — stage and commit changes (interactive: shows diff, asks for confirmation)
- `/git commit <files...>` — stage and commit specific files
- `/git branch <name>` — create and switch to a new branch (from `origin/main`)
- `/git pr` — push current branch and open a PR
- `/git help` — print this usage summary

## Branch Naming

| Type | Pattern | Example |
|---|---|---|
| Feature | `feat/<kebab-slug>` | `git branch feat/add-dark-mode` |
| Fix | `fix/<kebab-slug>` | `git branch fix/auth-race-condition` |
| Chore | `chore/<kebab-slug>` | `git branch chore/update-deps` |

Rules:
- All lowercase, hyphens only
- Slug: 2–5 words describing the change
- Always branch from `origin/main` (run `git fetch origin` first)

## Commit Messages — Conventional Commits

```
<type>(<scope>): <short description>

[optional body — explain the why, not the what]
```

**Types:** `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `style`

**Common scopes:**
- `.claude/` files, `CLAUDE.md`, `.claudeignore` → `chore(claude):`
- Dependencies → `chore(deps):`
- CI/CD → `chore(ci):`
- Everything else → use the feature area (`memory`, `skills`, `agents`, `config`)

**Rules:**
- Subject line ≤ 72 chars, lowercase, no trailing period
- Body explains *why* — the diff shows what

## Workflow

### 1. Check status

```bash
git status --short
git branch --show-current
```

### 2. Create a branch

```bash
git fetch origin
git checkout -b <branch-name> origin/main
```

### 3. Stage and commit

1. Show what will be staged: `git status --short`
2. For commits touching more than 10 files, show a summary and ask for confirmation
3. Stage explicitly — never `git add .` blindly
4. Write commit message following conventions above
5. `git commit -m "..."`

### 4. Open a PR

1. Push: `git push -u origin <current-branch>`
2. Create PR: `gh pr create` with this body template:

```
## What
<1–3 bullets summarizing the change>

## Why
<problem being solved or context>

## How to test
<steps to verify>
```

## Safety Rules

- Never commit directly to `main`
- Never force push (`git push --force`)
- Never run `git reset --hard` or `git clean -f` without explicit confirmation
- Always confirm what will be staged before committing
- Flag any uncommitted changes that weren't part of the task

## Token Efficiency

Never use the `Read` tool to understand what changed. Use git commands instead:
- What changed: `git diff --staged` or `git diff HEAD`
- What files: `git status --short`
- Branch history: `git log --oneline origin/main..HEAD`
