# Day Closure — Claude Code

## Setup

### 1. Add to CLAUDE.md

```markdown
### Day closure
When the user says "close the day", "day closure", or "/day-closure",
follow the instructions in `skills/day-closure/SKILL.md`.

Workspace configuration:
- WORKSPACE: /path/to/your/notes
- DAILY_NOTE_PATH: {WORKSPACE}/daily/YYYY-MM-DD.md
- REFLECTION_SECTION: ## How today went
- TOMORROW_SIGNAL_SECTION: ## Today's context
- REFLECTION_HOOK: false
```

Replace `/path/to/your/notes` with your actual notes path.

### 2. Copy the skill

```bash
mkdir -p skills/day-closure
cp /path/to/SKILL.md skills/day-closure/
```

### 3. Configure section headings

Open `SKILL.md` and update the Configuration table to match your daily note format. The defaults (Focus, Notes, Work closure, How today went, Today's context) work if your daily notes use those headings. If yours differ, update them.

---

## Running

### On demand

```
Close the day.
```

or

```
/day-closure
```

Claude reads your today's note, proposes a read, waits for your go/no-go, then writes the close and tomorrow's signal in one pass.

### After a work-closure skill

If you run a work-closure skill at end of day, it can call day-closure automatically:

```
# In your work-closure SKILL.md:
After writing the work summary, offer: "Ready to close the day? I'll do the full close now, or you can run /day-closure later."
```

### Retroactive close (skipped yesterday)

If you skipped yesterday's close, morning-review can offer it. Or trigger manually:

```
Close yesterday.
```

Claude will write everything to yesterday's note and, if anything carries forward, add a carry line to today's context section.

---

## Daily note format

The skill works with any daily note format that has the configured section headings. A minimal working example:

```markdown
---
date: 2025-03-25
---

## Today's context
(forward signal from yesterday lands here)

## Focus
- [ ] Main thing for today

## Notes
- 14:30 — Met with team about migration plan

## Work closure
(upstream work-closure writes here)

## How today went
(day-closure writes here)
```

If tomorrow's note doesn't exist at write time, Claude creates it from your template. Make sure your template path is accessible.

---

## Optional: personal reflection hook

If you keep a reflective practice (a daily prompt, a values check, a divination tool, a prayer) and want to close open loops at day's end, set `REFLECTION_HOOK: true` in your CLAUDE.md configuration.

You'll also need to tell Claude where your practice's journal lives and what an "open loop" looks like in your specific format. Add a note in CLAUDE.md:

```markdown
### Reflection hook (optional)
REFLECTION_HOOK: true
Practice journal: {WORKSPACE}/practice/journal.md
An "open loop" is an entry with an empty "Evening review:" field.
```

The skill will glance for open loops during its pre-read and offer to close them as part of the close — one clause, opt-in.

---

## WAT mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the full step sequence |
| **Agent** | Claude (Sonnet sufficient; Opus for better interpretive reads) |
| **Tools** | Claude Code's file read/write, `date` command for timestamps |

## Tips

- **Two turns is the target.** If a close is taking more than four turns, something is off — either the agent is manufacturing depth, or there's a real thread worth going into. The short close should feel nearly free.
- **The forward signal is the durable output.** Even if the reflection entry is short, the signal shapes tomorrow's context. Write it carefully.
- **Don't skip on hard days.** The short close is deliberately cheap for this reason — it should cost less than skipping.
