# Breadcrumb — Generic agent

How to wire up the breadcrumb skill on any agent that has file read/write access. This covers the common patterns; adapt to your agent's tool names and invocation model.

---

## Minimum requirements

Your agent needs:

- **File read** — to scan daily notes, project notes, agent context
- **File write** — to update those files
- **Clock access** — wall-clock time in your timezone for the breadcrumb timestamp
- **Conversation access** — to review what happened in the current session

Optional but useful:
- **Shell access** — simplifies timestamping
- **Multi-file read** — speeds up the task-state scan

---

## Directory structure

Set up these directories before loading the skill. Adjust names to match your workspace conventions:

```
{workspace}/
├── {daily_notes}/          # One YYYY-MM-DD.md per day
├── {logs}/                 # Session logs (auto-write zone)
├── {memory}/
│   ├── agents/             # Agent context files
│   ├── semantic/           # Long-term facts
│   └── episodic/           # Significant decisions
└── {projects}/             # Active project notes (optional)
    └── {project}/
        └── {project}.md
```

---

## Loading the skill

The breadcrumb skill is a set of natural-language instructions in `SKILL.md`. Load it by:

1. **Pasting it into your system prompt** — the simplest approach; the skill is stateless and short
2. **Referencing it by path** — if your agent loads files from a skills directory automatically
3. **Triggering it by name** — if your platform supports named skills or slash commands

The skill uses placeholder paths (`{daily_notes}`, `{logs}`, etc.). Replace these with your real paths, either in the SKILL.md copy itself or by defining them in your system prompt:

```
When running the breadcrumb skill, use these paths:
- {daily_notes} = notes/daily/
- {logs} = notes/agent-logs/
- {memory} = notes/memory/
- {agent_name} = my-agent
- {timezone} = Europe/London
```

---

## The gate and how to handle it

By default, the skill presents a proposal at step 5 and waits for the user to reply `APPLY`. This is intentional — daily notes and project notes are canonical records, and the task-state scan can occasionally mis-classify an item.

**If you want a fully automated (no-gate) breadcrumb**, restrict the skill to writes in the auto-write zone only: session log (`{logs}/`) and agent context (`{memory}/agents/`). Strip the proposal step and apply immediately. This sacrifices task-state accuracy but works for unattended runs.

**If your agent platform supports confirmation callbacks**, wire `APPLY` as a confirmation action that triggers step 6.

---

## Triggering

Standard trigger phrases (configure these in your agent's routing):

- `breadcrumb`
- `wrap up`
- `log this session`
- `we're done`

Or trigger programmatically at session end:

```python
# Python example — trigger at session teardown
def on_session_end(session):
    session.run("Run the breadcrumb skill to log this session.")
```

---

## Timestamping

The breadcrumb needs a wall-clock timestamp. How you get it depends on your platform:

| Platform | Approach |
|----------|----------|
| Shell access | `date +"%H:%M"` with `TZ` set |
| Python | `datetime.now(tz).strftime("%H:%M")` |
| Node | `new Date().toLocaleTimeString(locale, {hour: '2-digit', minute: '2-digit', timeZone: tz})` |
| No clock access | Ask the user to confirm the time before running |

Never estimate the timestamp. A wrong timestamp on a breadcrumb undermines the navigability of the daily note.

---

## Adapting the frontmatter

The session log frontmatter template in SKILL.md uses fields suited to a multi-agent setup:

```yaml
---
type: agent-log
session_id: YYYY-MM-DD-NNN
agent: {agent_name}
platform: {platform}
mode: operations | thinking | publishing
vault_changes: true | false
---
```

Simplify or extend this to match your system's conventions. The minimum useful fields are `type`, `agent`, and a date-derived `session_id`. Everything else is optional.

---

## Minimal implementation (log + breadcrumb only)

If you want the simplest possible version — just the session log and daily note breadcrumb, no task-state scan, no memory routing — use this condensed flow:

1. Write session log to `{logs}/YYYY-MM-DD-{agent}-{slug}.md` with frontmatter and a summary paragraph
2. Get the current time in `{timezone}`
3. Append to `{daily_notes}/YYYY-MM-DD.md`:
   ```
   - HH:MM — {one sentence of what happened} → [{slug}]({logs}/YYYY-MM-DD-{agent}-{slug}.md)
   ```

That's the minimum viable breadcrumb. Add the task-state scan and memory routing when you're ready for the full version.

---

## WAT mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 7-step session wrap-up |
| **Agent** | Any agent with file read/write + clock access |
| **Tools** | File I/O, shell or language runtime for timestamp |
