# Morning Review — Generic Setup

This guide covers setup for platforms other than Claude Code. The skill itself (`SKILL.md`) is platform-agnostic — only the wiring differs.

---

## What the Skill Needs

| Capability | Required | Notes |
|------------|----------|-------|
| File read | Yes | Reads inbox, daily notes, project notes, meeting notes |
| File write | Yes | Writes routed tasks, Focus, journal entries |
| Bash / shell | Optional | Used for vault-wide scans (overdue items, open tasks). Can substitute with file listing if no shell access. |
| Calendar access | Optional | Surfaces meeting load for Focus capacity check |

---

## Workspace Layout

The skill uses a configurable workspace layout. Adapt these defaults to your own structure:

```
{workspace}/
├── Inbox/
│   └── Inbox.md              # Unprocessed captures — this empties during the review
├── Projects/
│   └── Active/
│       └── {project-name}/   # One folder per project
│           └── {name}.md     # Main project note with outcome, tasks, waiting-fors
├── Areas/                    # Ongoing responsibilities (no end date)
├── Daily/
│   └── YYYY-MM-DD.md        # Daily note — Focus, Notes, context sections
└── Meetings/                 # Unprocessed meeting notes
    └── Processed/            # Moved here after review
        └── YYYY-MM/
```

If your workspace uses different folder names, paths, or note structures, update the configuration table in SKILL.md to match.

---

## Task Format

The skill expects tasks in this format (adapt to your system's conventions):

```
- [ ] Verb-led task description [status:: next] [energy:: deep|quick|low]
```

Completed tasks:

```
- [x] Done task
```

Waiting-fors:

```
- [ ] Follow up with [person] about [topic] [status:: waiting] [since:: YYYY-MM-DD]
```

---

## Daily Note Format

Minimum sections the skill reads and writes:

```markdown
---
date: YYYY-MM-DD
---

## Today's context
[Forward signal from yesterday's closure — what to carry forward]

## Focus
- [ ] Priority item 1
- [ ] Priority item 2

## Notes
- HH:MM — Morning review complete. [one-line summary]
```

---

## Embedding the Skill

### In a system prompt

Add to your agent's system prompt or instructions file:

```
When the user says "morning review", "/morning-review", "let's do the review",
or "start the review", follow the morning review workflow in your skill library.
The skill is at: {path-to-SKILL.md}. One step at a time. The user decides everything.
```

### In an instructions file (CLAUDE.md, cursor rules, etc.)

```markdown
### Morning Review
Trigger: "morning review" | "/morning-review" | "let's do the review"
Skill file: skills/morning-review/SKILL.md
Mode: interactive — one step at a time, user decides all routing and Focus items
```

---

## Calendar Integration

Calendar access improves Phase A context and Phase D Focus capacity checks. Most agent platforms support calendar via:

- **MCP servers** (Claude Code, any MCP-compatible platform) — search for a CalDAV, iCloud, or Google Calendar MCP server
- **Tool use / function calling** — pass calendar events as context at session start
- **Manual** — paste today's schedule at the start of the review

Without calendar access, the skill works but Step 1b returns empty. The Focus capacity check in Step 7 will note that no calendar data is available and recommend conservative defaults.

---

## Overdue Scanning

Phase B Step 4 runs a vault-wide scan for overdue and due-today items. If your platform has no shell access, substitute with:

1. A file listing across Projects/ and Areas/ directories
2. Filtering for tasks with `due:` or `defer:` metadata
3. Comparing dates against today

The scan is non-negotiable — it ensures dated items from across the workspace surface into the morning's collection, not just whatever was on yesterday's Focus.

---

## FAQ

**Can I run this without a daily note?**
The daily note is where forward context, Focus, and the morning summary land. Without it, the skill has nowhere to read yesterday's signal (Step 1) and nowhere to write today's Focus (Step 7). It can run in a degraded mode but you'll lose the continuity thread. A minimal daily note with `## Today's context`, `## Focus`, and `## Notes` is the minimum.

**What if I don't have an Inbox.md?**
The inbox drain (Step 3) will skip silently if the file is empty or absent. You can still run the review for meeting notes + overdue items + open loops. But the inbox empties rule only applies to what exists — if you have captures elsewhere, you'll want to wire them in.

**Can the agent run this without me present?**
Phase B (collection) can run non-interactively and produce a pre-processed list for you to review. Phase C (clarify) and Phase D (Focus) require you to be present — routing decisions and Focus setting are yours to make, not the agent's. A fully automated run that routes and sets Focus without your input defeats the purpose.

**My projects aren't in a folder-per-project structure.**
The project landscape scan in Phase C needs some way to read project outcomes and open tasks. Adapt the scan commands to your structure. The key output needed: a list of active project names with their stated outcomes, and a list of currently open next actions per project.
