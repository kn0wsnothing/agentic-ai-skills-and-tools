# Chief-of-Staff Brief — Scheduling

The brief should run before your weekly review, early enough that you have time to read it first.

---

## Recommended Timing

| Brief | When | Purpose |
|-------|------|---------|
| Chief-of-Staff Brief | Monday 6–7am (or Sunday evening) | Sweep week's state, queue decisions |
| Weekly Review | Monday morning | Act on decisions the brief surfaced |

Adjust to your timezone and review cadence. The key constraint: the brief runs before the review, not during it.

---

## Linux/macOS (cron)

```bash
# Monday at 6am
0 6 * * 1 cd /path/to/workspace && claude --print --model claude-opus-4-6 \
  "Run the chief-of-staff brief following skills/chief-of-staff-brief/SKILL.md. \
   Workspace: /path/to/workspace. Today is $(date +%Y-%m-%d). \
   Write output to /path/to/workspace/briefs/." \
  >> /var/log/chief-of-staff-brief.log 2>&1
```

Edit crontab with `crontab -e`.

---

## GitHub Actions

```yaml
name: Chief-of-Staff Brief
on:
  schedule:
    - cron: '0 22 * * 0'  # Sunday 10pm UTC — adjust for your timezone
  workflow_dispatch:

jobs:
  brief:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      - name: Run brief
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          TODAY=$(date +%Y-%m-%d)
          claude --print --model claude-opus-4-6 \
            "Run the chief-of-staff brief following skills/chief-of-staff-brief/SKILL.md. \
             Workspace: $GITHUB_WORKSPACE. Today is $TODAY. \
             Write output to $GITHUB_WORKSPACE/briefs/."
      - name: Commit brief
        run: |
          git config user.name "Chief-of-Staff Bot"
          git config user.email "bot@example.com"
          git add briefs/
          git diff --cached --quiet || git commit -m "Chief-of-staff brief — $(date +%Y-%m-%d)"
          git push
```

---

## Pairing with Other Weekly Skills

If you run a nightly distillation or memory consolidation pass, the brief should run after them — it reads session logs that the distillation writes.

```bash
# Crontab example — full weekly preparation pipeline
0  21 * * 0  /path/to/scripts/distillation.sh        # Sunday 9pm — capture week
0   3 * * 1  /path/to/scripts/memory-consolidate.sh  # Monday 3am — prune memory
0   6 * * 1  /path/to/scripts/chief-of-staff.sh      # Monday 6am — brief ready for review
```

---

## Verifying the Schedule

Check that the cron job ran and produced output:

```bash
# Check the log
tail -50 /var/log/chief-of-staff-brief.log

# Check for today's brief
ls -la /path/to/workspace/briefs/ | grep $(date +%Y-%m-%d)
```

---

For macOS launchd, Windows Task Scheduler, and Python scheduling patterns, the same structure applies — swap the shell command for the equivalent invocation on your platform.
