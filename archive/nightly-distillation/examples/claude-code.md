# Nightly Distillation — Claude Code

## Setup

Same memory structure as the [Memory Dream Cycle](../../memory-dream-cycle/examples/claude-code.md). If you've already set that up, skip to step 2.

### 1. Add to CLAUDE.md

```markdown
### Nightly Distillation
When asked to run a distillation (or on a schedule), follow the instructions in
`skills/nightly-distillation/SKILL.md`. This captures today's work into structured
memory files.
```

### 2. Copy the skill

```bash
mkdir -p skills/nightly-distillation
cp /path/to/SKILL.md skills/nightly-distillation/
```

## Session Discovery

Claude Code is typically a single-session agent. For distillation, the "session" is your current conversation. The skill will review the conversation history and extract what matters.

If you run multiple Claude Code sessions per day (different terminal windows, different projects), you'll need to either:
- Run distillation at the end of each session, or
- Consolidate manually by telling the agent what happened in other sessions

## Running

### Manual (end of day)

```
Run the nightly distillation. Follow skills/nightly-distillation/SKILL.md.
Review today's conversation and capture what's worth remembering.
```

### Automated

```bash
# Cron job — runs daily at 9pm
0 21 * * * cd /path/to/project && claude --print --model claude-opus-4-6 \
  "Run the nightly distillation following skills/nightly-distillation/SKILL.md." \
  >> /var/log/distillation.log 2>&1
```

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 6-phase capture pipeline |
| **Agent** | Claude (Opus recommended) |
| **Tools** | Claude Code's file read/write, shell access |

## Tips

- **Run before the dream cycle.** If you use both, schedule distillation first (9pm), dream cycle second (3am).
- **Single-session limitation.** Claude Code doesn't have a session list API. The distillation reviews the current conversation. For multi-session days, consider running it in each session.
