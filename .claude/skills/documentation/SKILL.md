---
name: documentation
description: Post-session doc sweep — update READMEs, docs, and notes across the studio. Delegates to lead agents.
user-invocable: true
model: sonnet
effort: high
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, Agent
---

# Documentation Skill

Audit what changed across the studio this session and create or update documentation accordingly.

## Step 1 — Detect changes

Run `git status --short` across all repos under `M:\Stuff\Work\repos\`:

```bash
for repo in M:/Stuff/Work/repos/*/; do
  STATUS=$(git -C "$repo" status --short 2>/dev/null)
  if [ -n "$STATUS" ]; then
    echo "=== $(basename $repo) ===" && echo "$STATUS"
  fi
done
```

Also check for untracked files in `M:\Stuff\Work\knowledge/` and `M:\Stuff\Work\memory/`.

Ignore: `node_modules/`, `dist/`, `*.lock`, `.env*`, `memory/`, `.claude/settings*`, auto-generated files.

If nothing changed, report and stop.

## Step 2 — Classify each change

| Change | Action |
|---|---|
| New directory or module | Create or update `README.md` |
| Modified existing docs | Review and update for accuracy |
| New skill or agent | Document in repo `CLAUDE.md` |
| Cross-repo contract change | Draft a decision note in `knowledge/decisions/` |
| New project or major workflow change | Update `memory/project.md` |

Skip: test files, config files, lock files, formatting-only changes, private/internal functions.

## Step 3 — Delegate if needed

For changes inside a specific repo, spawn the appropriate lead agent with:
- The exact list of changed files
- The specific action needed per file
- Line limits: 80 lines for module READMEs, 150 lines for repo READMEs

## Step 4 — Report

Return a concise summary:
- Repos checked
- Files documented (created or updated)
- Files skipped and why
