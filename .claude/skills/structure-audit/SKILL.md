---
name: structure-audit
description: Audit docs, skills, and directory structure for gaps and staleness. Read-only — outputs a report with proposed changes.
user-invocable: true
model: sonnet
effort: medium
allowed-tools: Read, Glob, Grep, Bash
---

# Structure Audit Skill

Read-only audit of the repo's documentation, skills, and directory structure. Output a structured report. Do NOT modify any files — only propose.

## 1. Collect ground truth

```bash
# All skills
ls .claude/skills/

# All agents
ls .claude/agents/

# All docs
find . -name "*.md" -not -path "./.git/*" -not -path "./node_modules/*" | sort

# Top-level directory listing
ls -la
```

## 2. Audit README.md / CLAUDE.md

Read `CLAUDE.md`. Check:
- **Skills section** — does it list all user-invocable skills? Are descriptions accurate?
- **Agents section** — does it match `.claude/agents/`?
- **Active projects** — still active or completed/stale?
- **Paths and references** — do they still exist?

## 3. Audit each skill file

For each file under `.claude/skills/*/SKILL.md`:
- Does it have complete frontmatter (`name`, `description`, `user-invocable`)?
- For `user-invocable: true` skills: is it listed in `CLAUDE.md`?
- Does it reference any paths or tools that no longer exist?

## 4. Audit each agent file

For each file under `.claude/agents/*.md`:
- Does it have correct frontmatter?
- Does it reference any paths or skills that no longer exist?
- Is it listed in `CLAUDE.md`?

## 5. Output the report

```
## Structure Audit — {date}

### Issues (must fix)
- [file] ...

### Stale (low priority)
- ...

### Missing (gaps)
- ...

### Looks good
- ...

### Proposed changes
For each issue: the exact file, what to change, and why.
```

Do NOT implement any changes. Present the report and ask for approval.
