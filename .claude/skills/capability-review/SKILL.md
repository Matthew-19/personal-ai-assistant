---
name: capability-review
description: End-of-session skill that reads delegation patterns and proposes new skills or agents that would improve future efficiency. Read-only — proposes only, never writes files.
user-invocable: true
tools: Read, Glob, Grep
---

# Capability Review

At the end of a session, run this skill to surface repeated delegation patterns that might benefit from a new skill or agent.

## What this skill does

1. Reads `.claude/agents/*.md` and `.claude/skills/*/SKILL.md` to know what already exists
2. Reflects on what was delegated or done manually this session
3. Identifies patterns repeated 3+ times
4. Proposes — does NOT create — new skills or agents

## What this skill does NOT do

- Write any files
- Create or modify agent/skill definitions
- Act autonomously — every proposal requires explicit user approval

## Output format

For each proposal:

```
Proposed skill/agent: <name>
Type: skill | agent
Trigger: what would invoke it
Repeated pattern: describe the 2–3 instances that triggered this proposal
What it would do: one paragraph
Risk: any concern about overlap with existing agents/skills
Recommendation: create | defer | skip
```

If no patterns meet the threshold, output: "No new capability proposals this session."

## Threshold rules

- Minimum 3 repetitions of the same pattern
- The pattern must involve non-trivial work (multiple steps or a long prompt)
- Propose reuse of existing skills/agents first
