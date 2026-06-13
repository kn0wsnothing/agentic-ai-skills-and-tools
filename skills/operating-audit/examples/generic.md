# Operating Audit — Generic Setup

This guide covers setup for any file-access agent: custom orchestrators, SDK-based agents, local LLM setups, or any platform not specifically listed.

---

## What your agent needs

The operating audit is primarily a read-heavy skill with a light write at the end. Minimum requirements:

- **File read access** — daily notes, session/activity logs, project files, governance docs
- **Shell/bash access** (optional but recommended) — used for the targeted log extraction; can be replaced with sequential file reads at higher context cost
- **File write access** (optional) — to save the deliverable; in-session output works if you don't need persistence
- **Interactive back-channel** — the user should be able to respond to the in-session deliverable and accept or reject changes

---

## Workspace structure

The skill uses path placeholders. Define these once and reference them everywhere:

| Placeholder | What it points to | Example |
|-------------|-------------------|---------|
| `{workspace}` | Root of your notes | `~/notes/` |
| `{daily_notes}` | Daily note files (YYYY-MM-DD.md) | `{workspace}/daily/` |
| `{logs}` | Agent session or activity log files | `{workspace}/logs/` |
| `{projects}` | Active project notes | `{workspace}/projects/active/` |
| `{governance}` | Governance layer files | `{workspace}/system/` |
| `{health_script}` | Health-check or aggregator script | `{workspace}/system/health-check.py` |
| `{audit_window_weeks}` | How far back to look | `12` |

Put these in your agent's system prompt or a persistent config file it loads every session.

---

## Daily notes format

The extraction (Step 1b) looks for specific sections. For best results, your daily notes should include:

```markdown
# YYYY-MM-DD

## Focus
Items I'm committing to today.
- [ ] Task one [status:: next]
- [x] Task completed [status:: done]

## Notes
Captures, observations, follow-ups from the day.

## How today went
End-of-day reflection. What actually happened, what didn't, what felt stuck.
```

The audit mines `## Focus` for stated intent vs. completion, `## Notes` for carried items and open threads, and `## How today went` for verbatim friction and avoidance signals.

If your daily notes use different section names, tell the agent what they're called before the audit runs.

---

## Log file format

The targeted log extraction (Step 1c) looks for sections named:
- `## Open items`
- `## Next actions`
- `## Open threads`
- `## Pending`

If your logs use different section names, adapt the bash pattern:

```bash
awk '/^## (Your section name 1|Your section name 2)/{flag=1; print; next} /^## /{flag=0} flag' "$f"
```

Or replace the bash extraction with sequential file reads and search for your equivalent sections.

---

## Project file format

Step 1d reads active project files. The skill looks for:
- **Next actions** — open tasks (lines with `- [ ]` or equivalent)
- **Waiting-fors** — items blocked on external input (section named `## Waiting for` or tagged `status:: waiting`)
- **Session log** — the last substantive entry date

Minimum working format:

```markdown
---
outcome: Verb-led outcome statement — what "done" looks like
status: active
---

# Project name

## Next actions
- [ ] Verb-led task [status:: next]

## Waiting for
- [ ] Waiting on something from someone [status:: waiting] [since:: YYYY-MM-DD]

## Session log
- YYYY-MM-DD — What happened last time this was touched
```

If your projects don't have `outcome:` frontmatter, the routing check in diagnosis loses precision — it can't distinguish "no next action because it's blocked" from "no next action because it's been abandoned."

---

## Governance layer

The dogfood clause evaluates your governance layer by the same standard it applies to everything else. Tell the agent what components you maintain:

```markdown
## My governance layer

- Weekly process brief (generated at the start of each week)
- System health dashboard (updated weekly, lives at {governance}/dashboard.md)
- Project triage checklist (run before any new project enters the active list)
```

If you don't maintain a governance layer, the dogfood step is skipped. The rest of the audit runs normally.

---

## Replacing bash with file reads

If your agent doesn't have shell access:

**Log extraction (Step 1c):** read each log file and search for the open-items / next-actions sections manually. Equivalent pattern in pseudocode:

```
for each log file in the window:
  read the file
  find lines after "## Open items" or "## Next actions" 
  stop at the next "## " heading
  return the first 25 lines of that section
```

More context cost than the bash version, equivalent results.

**Daily-notes extraction (Step 1b):** read each daily note file and extract the three target sections (Focus, Notes, How today went). For windows longer than 4 weeks, process in batches to avoid context saturation.

---

## Loading the skill

Load `SKILL.md` into your agent's system prompt or as a persistent instruction file.

**Python (Anthropic SDK):**
```python
from pathlib import Path

skill = Path("skills/operating-audit/SKILL.md").read_text()
workspace_config = """
{workspace} = /path/to/notes/
{daily_notes} = {workspace}/daily/
{logs} = {workspace}/logs/
{projects} = {workspace}/projects/active/
{governance} = {workspace}/system/
{audit_window_weeks} = 12
"""

messages = [
    {"role": "user", "content": f"{skill}\n\n{workspace_config}\n\nOperating audit."}
]
```

**System prompt injection:**
```
[contents of SKILL.md]

## Path configuration
{workspace} = /your/notes/root/
{daily_notes} = {workspace}/daily/
...
```

---

## Triggering the audit

The skill responds to natural-language triggers:
- "Operating audit"
- "/operating-audit"
- "Audit how I work"
- "Quarterly operating audit"
- "Run the behavioral audit"

Set up a keyword hook in your agent if your platform supports it.

---

## Saving the deliverable

The deliverable is shown in-session by default. To persist it:

1. At the end of the in-session review, tell the agent to save it
2. Recommended path: `{workspace}/reviews/operating-audit-YYYY-MM-DD.md`
3. Include `type: operating-audit` in frontmatter for later retrieval

Saving matters for the dogfood clause's two-consecutive-audits threshold. Without a saved prior audit, the threshold degrades to single-window evaluation.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — four-step audit pipeline |
| **Agent** | Your choice — use the strongest model available for diagnosis and deliverable |
| **Tools** | File read + shell (optional) + file write (optional) |

---

## Scheduling

Quarterly cadence is the design intent — long enough for real patterns to emerge, short enough to catch friction before it compounds. Wire into your quarterly review checklist or trigger manually.

**Cron — pre-extract the logs non-interactively:**

```bash
# Quarterly pre-extraction — log open-items sections only
# Run before the interactive audit session
#!/bin/bash
WINDOW_START=$(date -d "-90 days" +%Y-%m-%d)
OUTPUT="/tmp/audit-log-extract-$(date +%F).txt"

cd /path/to/logs/ && \
for f in $(ls | awk -F'-' -v start="$WINDOW_START" '$0 >= start'); do
  echo "=== $f" >> "$OUTPUT"
  awk '/^## (Open items|Next actions|Open threads|Pending)/{flag=1; print; next} /^## /{flag=0} flag' "$f" \
    | head -25 >> "$OUTPUT"
done

echo "Log extraction saved to $OUTPUT"
```

Then start the interactive audit: "Operating audit — use the pre-extracted log data at `/tmp/audit-log-extract-YYYY-MM-DD.txt` for Step 1c."

The daily-notes extraction and diagnosis still require an interactive session with a capable model.

---

## Tips

**Run on a capable model.** The diagnosis and deliverable require genuine editorial judgment — evaluating whether a pattern is friction or craft, whether the governance layer changed decisions, what the one avoided thing actually is. A weaker model will produce generic findings. Use the best model you have.

**First run: don't filter the window.** Run the full available window on the first audit, even if it's only a few weeks. You need baseline signal before you can compare.

**Save every deliverable.** The two-consecutive-audits dogfood threshold requires comparison across runs. No saves means no threshold tracking.

**The one thing avoided is the load-bearing output.** If it's comfortable, it wasn't named sharply enough. Ask for a re-examination if the finding feels soft.

**Don't soften the governance findings.** The dogfood clause is the most useful and most uncomfortable part of the audit. A governance component that can't show decision-changing instances in 12 weeks isn't governing anything. Name it directly.
