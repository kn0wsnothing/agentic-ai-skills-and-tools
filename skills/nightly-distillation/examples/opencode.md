# Nightly Distillation — OpenCode

## Setup

Same memory structure as the [Memory Dream Cycle](../../memory-dream-cycle/examples/opencode.md).

### Add to your instructions

In your `AGENTS.md` or equivalent:

```markdown
### Nightly Distillation
When asked to run a distillation, follow the instructions in
`skills/nightly-distillation/SKILL.md`. This captures today's work into structured
memory files.
```

## Session Discovery

OpenCode is typically a single-session agent. The distillation reviews the current conversation history. For multi-session days, run the distillation in each session or summarize other sessions manually.

## Running

```
Run the nightly distillation. Follow skills/nightly-distillation/SKILL.md.
```

For scheduled runs, wrap in a cron job if OpenCode supports a non-interactive mode. See [cron-schedule.md](cron-schedule.md).

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 6-phase capture pipeline |
| **Agent** | OpenCode's AI model |
| **Tools** | OpenCode's file read/write, shell access |
