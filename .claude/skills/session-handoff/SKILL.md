---
name: session-handoff
description: Produce end-of-session handoff summary so context carries across /clear. Trigger on "session handoff", "wrap up", "hand off", or "handoff summary".
user-invocable: true
---

# Session Handoff

Produce a repeatable end-of-session summary so the user can `/clear` and start a fresh agent without losing continuity. The next agent should be able to pick up by reading this summary alone.

This is a **context-handoff artifact**, not a status report. The audience is a future instance of you, not a stakeholder.

## When to invoke

User says: "session handoff", "wrap up session", "hand off", "handoff summary", "let's wrap up", "summarize before I clear", or any near-equivalent. Also invoke proactively if the user says they're about to `/clear` without having run it yet.

## How to produce the summary

1. **Review the full conversation**, not just the last few turns.
2. **Pull state from these sources (in order):**
   - Plan files referenced this session (check `~/.claude/plans/` if a plan was mentioned).
   - Task list state — any in-progress or pending tasks.
   - Background processes started with `run_in_background` — shell IDs are load-bearing.
   - Files created or modified this session.
   - Memory files written or updated (`memory/` directory in this repo).
   - Unresolved questions — things asked that never got a clear answer.
3. **Do NOT audit the filesystem.** This is synthesis of what happened in THIS session.
4. **Produce the output in chat.** Do not write a file. Do not update memory. Chat-only.

## Output template — use exactly this structure, every time

```
# Session Handoff — <one-line title of what this session was about>

## Where it started
<2-3 sentences: what the user asked for, key framing or constraints>

## Decisions locked + what shipped
- <decision or change> — <why, and where it lives (absolute path if a file)>

## Key files for next session
- `<absolute path>` — <why the next agent should read this first>
- Plan file: `<path>` (if a plan drove the session)
- Memory files touched: `<paths>` (if any)

## Running state
- Background processes: <shell IDs + what they are + how to kill> — or "none"
- Dev servers / ports: <url + port> — or "none"
- Open worktrees / branches: <paths> — or "none"

## Verification — how to confirm things still work
- `<command>` — <expected outcome>

## Deferred + open questions
- Deferred: <item> — <why pushed to later>
- Open: <question needing the user's input> — <context>

## Pick up here
<1-2 sentences: the single most likely next action for a fresh agent>
```

## Hard rules

1. **Chat output only.** Never write the handoff to a file.
2. **Never invent state.** If a section has nothing to report, write "none".
3. **Absolute paths always.**
4. **If a plan file drove the session, name it first** in "Key files".
5. **No emojis, no hype.** Terse and concrete — paths, commands, shell IDs, decisions.
6. **Background process IDs are critical.** Include kill commands.
