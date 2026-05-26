---
name: memory-audit
description: Audit memory files for staleness, remove outdated entries, and commit a versioned snapshot.
user-invocable: true
model: sonnet
effort: medium
allowed-tools: Bash, Read, Write, Edit, Glob, Grep
---

# Memory Audit Skill

Run monthly or when memory files feel stale. Covers the repo memory and the Claude auto-memory system.

## Part A — Repo memory

Memory directory: `memory/` (in this repo)

### 1. Read the index

Read `MEMORY.md` from the project memory directory at `~/.claude/projects/<project-hash>/memory/` if it exists.

### 2. Read every memory file

Read each file under `memory/` in this repo: `user.md`, `project.md`, `feedback.md`.

### 3. Assess each entry

**Project memory** — most likely to go stale. Check:
- Referenced branches: do they still exist?
- Referenced file paths: do the files still exist?
- Described initiative status: still active, or completed/cancelled?

**Feedback memory** — rarely go stale. Only remove if contradicted by newer feedback or no longer applicable.

**User memory** — check that stated preferences and context are still accurate.

### 4. Commit changes

```bash
cd <repo-root>
git add memory/
git commit -m "chore(memory): audit $(date +%Y-%m-%d)"
```

## Part B — Claude auto-memory

Memory directory: `~/.claude/projects/<project-hash>/memory/` (exact path shown in the MEMORY.md system-reminder header)

### 1. Read the index

Read `MEMORY.md` from that directory.

### 2. Read every memory file

Read each file referenced in the index. Classify by type (`user`, `feedback`, `project`, `reference`).

### 3. Assess each memory

Same criteria as Part A. Update or remove outdated entries. Merge redundant entries.

### 4. Update MEMORY.md

Reflect all changes in the index. Keep lines under ~150 characters.

### 5. Commit

```bash
cd ~/.claude/projects/<project-hash>/memory
git add -A
git commit -m "chore: memory audit $(date +%Y-%m-%d)"
```

## Report

Output a concise summary:
- Total entries reviewed
- How many updated, removed, merged
- Specific list of what changed and why
