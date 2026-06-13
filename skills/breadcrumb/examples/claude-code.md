# Breadcrumb — Claude Code

How to wire up the breadcrumb skill in a Claude Code project.

---

## Setup

### 1. Copy the skill

```bash
mkdir -p .claude/skills/breadcrumb
cp /path/to/skills/breadcrumb/SKILL.md .claude/skills/breadcrumb/
```

Or symlink if you're maintaining a shared skills library:

```bash
ln -s /path/to/skills/breadcrumb/SKILL.md .claude/skills/breadcrumb/SKILL.md
```

### 2. Configure paths in CLAUDE.md

Add a configuration block to your `CLAUDE.md`:

```markdown
## Breadcrumb configuration

| Parameter | Value |
|-----------|-------|
| `{daily_notes}` | `notes/daily/` |
| `{logs}` | `notes/agent-logs/` |
| `{memory}` | `notes/memory/` |
| `{projects}` | `notes/projects/active/` |
| `{inbox}` | `notes/inbox/` |
| `{agent_name}` | `claude` |
| `{timezone}` | America/New_York (or your timezone) |
```

Adjust paths to match your workspace structure.

### 3. Add the trigger instruction

In `CLAUDE.md`, add:

```markdown
## Session wrap-up

When the user says "breadcrumb", "wrap up", "log this session", or "we're done":
follow the instructions in `.claude/skills/breadcrumb/SKILL.md`.
```

---

## Running manually

At the end of any session:

```
wrap up
```

or

```
breadcrumb
```

The skill will write the session log immediately, then present a proposal for everything else. Reply `APPLY` to confirm.

---

## Automating with a stop hook

You can trigger breadcrumb automatically when a Claude Code session ends by adding a `stop` hook to your project settings.

In `.claude/settings.json`:

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Session ended — run breadcrumb if substantive work happened'"
          }
        ]
      }
    ]
  }
}
```

Note: a fully automated breadcrumb (no user confirmation step) requires bypassing the propose-first gate. Only do this if you trust the task-state scan to be correct in your setup, or if you've simplified the skill to write only the session log and breadcrumb (skipping project note updates).

A lighter automation: hook runs a reminder prompt, user responds `APPLY` after reviewing.

---

## Timestamp

For the breadcrumb timestamp, the skill runs:

```bash
date +"%H:%M"
```

Make sure your system clock is set correctly and `{timezone}` in your configuration matches your working timezone.

---

## WAT mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 7-step session wrap-up |
| **Agent** | Claude (Sonnet or Opus; Opus recommended for complex task-state scans) |
| **Tools** | File read/write, shell access for timestamp |

---

## Tips

- **Run it every session, even short ones.** The five-minute session where you confirmed a decision still deserves a one-liner breadcrumb. The value is a complete daily note, not just covering long sessions.
- **Slug the session well.** The first thing you write after "wrap up" shapes the slug. Mention the topic: "wrap up — we worked on the authentication flow redesign" produces a better slug than a bare "wrap up".
- **Review the task-state proposal before APPLY.** The scan is good but not infallible. Glance at the task classifications before confirming.
- **Single-session limitation.** Claude Code is typically one session per terminal. If you run multiple sessions per day in different projects, each session needs its own breadcrumb call.
