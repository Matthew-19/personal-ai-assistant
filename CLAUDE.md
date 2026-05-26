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
| `/start` | Session startup — load memory files, check open work |
| `/session-handoff` | End-of-session context dump |
| `/documentation` | Doc audits and health checks |
| `/capability-review` | Propose new skills from session patterns |
| `/memory-audit` | Monthly staleness check on memory files |
| `/structure-audit` | Directory / docs / skills health check |

## Permission Modes
- **Day-to-day:** `claude --permission-mode auto` (allowlist in `.claude/settings.local.json`)
- **Sensitive:** default mode or `accept-edits`

## Key Conventions
- All long-running work gets a session handoff at end
- Memory files are the source of truth — check them before asking the user to repeat something
- Prefer the free-claude-code proxy (`fcc-server` on :8082) for non-sensitive tasks

## How to Run
```bash
# Start Claude Code in this repo with auto-permissions
claude --permission-mode auto

# Free model proxy (if not already running)
fcc-server &
```

## Notes
- Part of the broader studio at `M:\Stuff\Work/`
- Connected repos: `obsidian-second-brain`, `AIM-Research-Orchestrator`
- AI HERO Phase 1 artifact
