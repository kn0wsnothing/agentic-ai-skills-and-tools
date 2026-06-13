# Nightly Distillation — Scheduling

The distillation should run daily in the evening, **before** the Memory Dream Cycle.

---

## Recommended Timing

| Skill | Time | Purpose |
|-------|------|---------|
| Nightly Distillation | 9:00 PM | Capture today's work |
| Memory Dream Cycle | 3:00 AM | Consolidate and prune |

Adjust to your timezone. The key constraint: distillation runs first.

---

## Linux/macOS (cron)

```bash
# Daily at 9pm
0 21 * * * cd /path/to/project && claude --print --model claude-opus-4-6 \
  "Run the nightly distillation following skills/nightly-distillation/SKILL.md." \
  >> /var/log/distillation.log 2>&1
```

---

## GitHub Actions

```yaml
name: Nightly Distillation
on:
  schedule:
    - cron: '0 13 * * *'  # 9pm HKT = 1pm UTC (adjust timezone)
  workflow_dispatch:

jobs:
  distill:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      - name: Run distillation
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude --print --model claude-opus-4-6 \
            "Run the nightly distillation following skills/nightly-distillation/SKILL.md."
      - name: Commit changes
        run: |
          git config user.name "Distillation Bot"
          git config user.email "bot@example.com"
          git add MEMORY.md memory/
          git diff --cached --quiet || git commit -m "📋 Nightly distillation — $(date -Iseconds)"
          git push
```

---

## Pairing with Dream Cycle

If you're scheduling both, leave at least a few hours between them:

```bash
# Crontab example
0 21 * * * /path/to/scripts/distillation.sh   # 9pm — capture
0  3 * * * /path/to/scripts/dream-cycle.sh     # 3am — consolidate
```

The distillation writes new content. The dream cycle reads that content during its contradiction scan and consolidation. Running them simultaneously would cause file conflicts.

---

For macOS launchd, Windows Task Scheduler, and Python scheduling examples, see the [Memory Dream Cycle scheduling guide](../../memory-dream-cycle/examples/cron-schedule.md) — same patterns apply, just swap the script/command.
