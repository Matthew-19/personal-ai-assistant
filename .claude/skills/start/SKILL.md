---
name: start
description: Session entry point for /start only. Skip if the opening message is a session handoff — the handoff IS the brief.
user-invocable: true
model: sonnet
effort: medium
allowed-tools: Read, Glob, Bash
---

**STOP — session handoff check:** If the user's opening message contains a structured session handoff (sections like "Decisions locked", "Key files", "Pick up here"), do NOT run this skill. The handoff is the brief. Read it, synthesize context, and proceed directly to work.

## Phase 1 — Session Brief

**MEMORY.md is already loaded in context — do not re-read it.**

**Session handoff detected?** Skip all file reads. Synthesize the brief from the handoff content directly.

**Otherwise**, use `Read` directly (never spawn an agent) on the memory files:
- `memory/user.md` — who is this person, what do they care about
- `memory/project.md` — what's in progress, key decisions
- `memory/feedback.md` — what's working, what's not

Output:

```
## Session Brief

### Active Projects
<what's in progress across repos>

### Recent Decisions
<active decisions relevant today>

### Open Items
<anything blocked, unresolved, or needing a decision>

### Suggested next step
<the most logical thing to work on>
```

## Phase 2 — Route

Ask the user: **"What are we working on?"** — Existing task | New idea | Just checking in

| Answer | Action |
|---|---|
| Existing task / ticket | Proceed directly to work. Check `memory/project.md` for context. |
| New idea / feature | Clarify the goal, then proceed. |
| Just checking in | Present the brief and open items. Wait for direction. |
