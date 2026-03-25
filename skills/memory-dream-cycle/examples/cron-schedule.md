# Memory Dream Cycle — Scheduling

The dream cycle is designed to run on a schedule. Here's how to set it up on different platforms.

---

## When to Run

**Ideal timing:** After your daily memory capture/distillation process, during off-hours.

- If you have a nightly capture at 9pm → schedule the dream cycle at 3am
- If you capture manually → schedule it before your first session each day (e.g., 6am)
- At minimum → run weekly (but daily is better)

**Why off-hours?** The dream cycle rewrites your working memory file. Running it while you're actively using the agent could cause conflicts.

---

## Linux/macOS (cron)

### With Claude Code CLI

```bash
# Edit crontab
crontab -e

# Add this line (runs daily at 3am)
0 3 * * * cd /path/to/your/project && claude --print --model claude-opus-4-6 "Run the memory dream cycle following skills/memory-dream-cycle/SKILL.md." >> /var/log/dream-cycle.log 2>&1
```

### With a wrapper script

Create `scripts/dream-cycle.sh`:

```bash
#!/bin/bash
set -euo pipefail

PROJECT_DIR="/path/to/your/project"
LOG_FILE="/var/log/dream-cycle.log"

echo "=== Dream Cycle $(date -Iseconds) ===" >> "$LOG_FILE"

cd "$PROJECT_DIR"
claude --print --model claude-opus-4-6 \
  "Run the memory dream cycle following skills/memory-dream-cycle/SKILL.md." \
  >> "$LOG_FILE" 2>&1

echo "=== Done ===" >> "$LOG_FILE"
```

```bash
chmod +x scripts/dream-cycle.sh
crontab -e
# 0 3 * * * /path/to/your/project/scripts/dream-cycle.sh
```

---

## macOS (launchd)

Create `~/Library/LaunchAgents/com.dream-cycle.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.dream-cycle</string>
    <key>ProgramArguments</key>
    <array>
        <string>/bin/bash</string>
        <string>/path/to/your/project/scripts/dream-cycle.sh</string>
    </array>
    <key>StartCalendarInterval</key>
    <dict>
        <key>Hour</key>
        <integer>3</integer>
        <key>Minute</key>
        <integer>0</integer>
    </dict>
    <key>StandardOutPath</key>
    <string>/tmp/dream-cycle-stdout.log</string>
    <key>StandardErrorPath</key>
    <string>/tmp/dream-cycle-stderr.log</string>
</dict>
</plist>
```

Load it:

```bash
launchctl load ~/Library/LaunchAgents/com.dream-cycle.plist
```

---

## GitHub Actions

For repo-based memory (where memory files are committed):

```yaml
# .github/workflows/dream-cycle.yml
name: Memory Dream Cycle
on:
  schedule:
    - cron: '0 19 * * *'  # Adjust to your timezone (this is UTC)
  workflow_dispatch:       # Manual trigger from GitHub UI

jobs:
  dream-cycle:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run dream cycle
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude --print --model claude-opus-4-6 \
            "Run the memory dream cycle following skills/memory-dream-cycle/SKILL.md."

      - name: Commit changes
        run: |
          git config user.name "Dream Cycle Bot"
          git config user.email "bot@example.com"
          git add MEMORY.md memory/
          git diff --cached --quiet || git commit -m "🌙 Dream cycle — $(date -Iseconds)"
          git push
```

---

## Windows (Task Scheduler)

1. Create `scripts/dream-cycle.bat`:

```batch
@echo off
cd /d C:\path\to\your\project
claude --print --model claude-opus-4-6 "Run the memory dream cycle following skills/memory-dream-cycle/SKILL.md." >> C:\logs\dream-cycle.log 2>&1
```

2. Open Task Scheduler → Create Task
3. Set trigger: Daily at 3:00 AM
4. Set action: Start a program → `scripts/dream-cycle.bat`

---

## Python (schedule library)

If your agent runs as a long-lived Python process:

```python
import schedule
import subprocess
import time

def run_dream_cycle():
    result = subprocess.run(
        ["claude", "--print", "--model", "claude-opus-4-6",
         "Run the memory dream cycle following skills/memory-dream-cycle/SKILL.md."],
        capture_output=True, text=True, cwd="/path/to/project"
    )
    print(result.stdout)

schedule.every().day.at("03:00").do(run_dream_cycle)

while True:
    schedule.run_pending()
    time.sleep(60)
```

---

## Verifying the Schedule

After setting up scheduling, verify it works:

1. **Trigger manually first** to confirm the command works
2. **Check logs** after the first scheduled run
3. **Review MEMORY.md** — you should see changes if there was anything to consolidate
4. **Check for a `## Needs review` section** — resolve any flagged items
