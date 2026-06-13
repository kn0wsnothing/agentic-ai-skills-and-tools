# Breadcrumb

A session wrap-up skill for AI agents. At the end of any work session, it writes a timestamped one-liner to your daily note, a structured session log to your logs directory, updates agent working context, and routes durable memory to the right destination.

This is the "close the loop" skill — the last thing that runs before you walk away.

---

## How it works

Six steps run in sequence, with a propose-first gate before any write outside the auto-write zone:

1. **Session log** — write a structured record of what happened to `{logs}/`
2. **Breadcrumb** — compose a one-sentence timestamped summary
3. **Task-state scan** — walk every task surface touched in the session and classify changes
4. **Vault delta proposal** — assemble all proposed writes (daily note, agent context, project notes, memory)
5. **Gate** — present the proposal and wait for explicit approval before writing
6. **Apply + verify** — write confirmed changes, report what landed

---

## Prerequisites

Your agent needs:

- **File read/write access** to your notes or workspace
- **A daily notes directory** — one file per day (e.g., `{daily_notes}/YYYY-MM-DD.md`)
- **A logs directory** — where session logs live (e.g., `{logs}/`)
- **An agent context file** — a persistent summary of where this agent left off (e.g., `{memory}/agents/{agent-name}-context.md`)
- **Optionally: project notes** — if you track active projects, their task lists benefit from task-state scans
- **A clock** — the breadcrumb timestamp should be wall-clock time, not estimated

---

## Quick start

1. **Set up the directory structure** (see Configuration below)
2. **Copy `SKILL.md`** into your agent's persistent instructions
3. **Configure the paths** in the Configuration table
4. **Trigger it** — say "breadcrumb", "wrap up", "log this session", or "we're done"
5. **Review the proposal**, reply `APPLY`

---

## Configuration

| Parameter | Example | Description |
|-----------|---------|-------------|
| `{daily_notes}` | `notes/daily/` | Directory for daily note files (YYYY-MM-DD.md) |
| `{logs}` | `notes/agent-logs/` | Directory for session log files |
| `{memory}` | `notes/memory/` | Root for memory files |
| `{projects}` | `notes/projects/active/` | Active project notes (optional) |
| `{inbox}` | `notes/inbox/` | Unprocessed capture queue (optional) |
| `{agent_name}` | `claude` | Canonical name used in log frontmatter |
| `{timezone}` | your timezone | Timezone for breadcrumb timestamps |

Define these once in your agent's system prompt or `CLAUDE.md`. The skill uses them everywhere.

---

## File layout

```
{workspace}/
├── {daily_notes}/
│   └── YYYY-MM-DD.md          # Daily note — breadcrumb appended here
├── {logs}/
│   └── YYYY-MM-DD-{slug}.md   # Session log — written at step 1
├── {memory}/
│   ├── agents/
│   │   └── {agent}-context.md # Agent context — updated at step 6
│   ├── semantic/               # Long-term facts (optional)
│   └── episodic/               # Significant decisions (optional)
└── {projects}/
    └── {project}/
        └── {project}.md        # Project notes — task-state updated
```

---

## Best practices

**Run it every session.** Even a five-minute session that only confirmed a fact is worth a one-liner breadcrumb. The value compounds — a complete daily note is navigable; a spotty one isn't.

**Trust the gate.** The propose-first pattern (step 5) exists because writes outside the auto zone (logs and temp files) touch your canonical record. Don't bypass it. Review the proposal.

**One bullet in the daily note, always.** If more than one sentence is needed, it goes in the session log file — not the daily note. The daily note is a navigable index, not a transcript.

**Keep slugs descriptive.** Session logs are named `YYYY-MM-DD-{agent}-{topic-slug}.md`. The slug should describe what the session accomplished (`newsletter-outline-v2`, `schema-debug-analytics`), never a number (`-1`, `-2`). Numbers make the logs directory illegible.

**Task-state scan is not optional.** Even if you didn't touch a project file directly, a thinking or planning session may have resolved tasks. The scan catches this. If it returns nothing, the proposal says so explicitly — silence is not the same as "nothing changed".

**Don't add forward-looking lines.** The log records completed work only. `Next:`, `TODO:`, and `Pending:` have no place in a breadcrumb. Those belong in your task system.

---

## Platform examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic (any file-access agent) | [examples/generic.md](examples/generic.md) |

---

## FAQ

**How is this different from just writing a session log?**
The session log is one output of six. Breadcrumb also ticks off completed tasks in project notes, routes durable facts to memory, updates agent context so tomorrow's session can pick up where this one left off, and leaves a navigable one-liner in the daily note. It's the full close-of-session ritual, not just a log write.

**What if I don't have project notes?**
Skip step 3's project scan. The skill still writes the daily note breadcrumb, session log, and agent context — those three are the minimum useful output.

**Can I automate it?**
Yes. Trigger it via hook (e.g., a Claude Code `stop` hook), cron, or platform-native scheduling. The session-end trigger is the most common pattern. See the platform examples.

**What if the task-state scan is expensive (many large project files)?**
The skill includes a delegation escape hatch: if the scan would require reading more than ~3 long project notes, delegate the reads to a smaller model or subprocess, then classify in-line afterward.

**What does APPLY do exactly?**
It appends the breadcrumb to the daily note, updates agent context, writes any proposed project/memory notes, and reports the list of files written. Nothing is written before you confirm.

---

## Design rationale

See [SPEC.md](SPEC.md) for the design decisions: why the gate is where it is, why the slug is non-negotiable, and how the task-state scan emerged from a real failure mode.
