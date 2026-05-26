# Personal AI Assistant — Claude Code Context

## What This Project Is
A personal AI assistant powered by Claude Code agents, skills, and a persistent memory system. Built to demonstrate (and use) modern AI-assisted workflows — and as evidence for the AI HERO reward.

## How It's Organized
- `.claude/agents/` — Lead agent definitions (general, research, writing)
- `.claude/skills/` — Slash commands for structured workflows
- `.claude/settings.json` — Hooks, permissions, local config
- `memory/` — Persistent context across sessions (user, project, feedback)

## Lead Agents
| Agent | When to Use |
|---|---|
| `general-lead` | Day-to-day tasks, code, debugging, file ops |
| `research-lead` | Web research, YouTube lookups, synthesis |
| `writing-lead` | Docs, notes, presentation content |

## Skills
| Skill | Purpose |
|---|---|
| `/start` | Session startup — load memory, present brief, route to work |
| `/session-handoff` | End-of-session context dump for `/clear` continuity |
| `/documentation` | Post-session doc sweep across repos |
| `/capability-review` | Propose new skills from repeated delegation patterns |
| `/memory-audit` | Monthly staleness check on all memory files |
| `/structure-audit` | Directory / docs / skills health check (read-only) |

## Permission Modes

- **Session default:** `plan` (set in user `settings.json` via `permissions.defaultMode`). Every session starts in plan mode — explore and design the approach first, then exit to `/plan` to execute.
- **Execution mode after plan:** When you exit plan mode, the session switches to `auto`. For sensitive work, switch to `accept-edits` or `manual` instead.
- **Never use `--bypass-permissions`.** The deny list in project `settings.json` handles dangerous commands.
- **Dangerous commands blocked globally** (see project `settings.json` deny list): `rm -rf`, `--force` pushes, `git reset --hard`, `mkfs`, `dd`.

## Key Conventions
- All long-running work gets a session handoff at end
- Memory files are the source of truth — check them before asking the user to repeat something
- Default to `fcc-claude` (free proxy). Switch to official `claude` when subscribed or when the task needs the most reliable model.

## How to Run

Both binaries read the same `~/.claude/settings.json`, so `plan` mode works for either:

```bash
# Free Claude Code proxy (current default — uses fcc-server on :8082)
fcc-claude

# Official Claude Code (same flags, same settings — use when subscribed)
claude

# Start the free model proxy if not already running
fcc-server &
```

## Notes
- Part of the broader studio at `M:\Stuff\Work/`
- Connected repos: `obsidian-second-brain`, `AIM-Research-Orchestrator`
- AI HERO Phase 1 artifact
