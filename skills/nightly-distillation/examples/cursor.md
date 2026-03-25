# Nightly Distillation — Cursor

## Setup

Same memory structure as the [Memory Dream Cycle](../../memory-dream-cycle/examples/cursor.md).

### Add to Cursor Rules

In `.cursor/rules/` or `.cursorrules`:

```markdown
### Nightly Distillation
When asked to run a distillation, follow the instructions in
`skills/nightly-distillation/SKILL.md`. This captures today's work into structured
memory files.
```

## Session Discovery

Cursor is a single-session editor agent. The distillation reviews the current Composer or chat history. Like Claude Code, multi-session coverage requires running the distillation in each session or manually summarizing other sessions.

## Running

In Cursor chat or Composer:

```
Run the nightly distillation. Follow skills/nightly-distillation/SKILL.md.
```

For scheduled runs, use a cron job with Claude Code CLI — Cursor doesn't have native scheduling. See [cron-schedule.md](cron-schedule.md).

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 6-phase capture pipeline |
| **Agent** | Cursor's AI model |
| **Tools** | Cursor's file read/write, terminal access |
