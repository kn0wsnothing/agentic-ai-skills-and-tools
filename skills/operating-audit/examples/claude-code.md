# Operating Audit — Claude Code

## Setup

### 1. Define your workspace paths

Add to your `CLAUDE.md` or project system prompt:

```markdown
## Operating Audit — path configuration

{workspace}       = /path/to/your/notes/
{daily_notes}     = {workspace}/daily/
{logs}            = {workspace}/agent-logs/
{projects}        = {workspace}/projects/active/
{governance}      = {workspace}/system/
{health_script}   = {workspace}/system/scripts/health-check.py  # optional
{audit_window_weeks} = 12
```

Replace paths with your actual structure. `{health_script}` is optional — skip if you don't have one.

### 2. Register the skill trigger

Add to your `CLAUDE.md`:

```markdown
### Operating Audit
When I say "operating audit", "/operating-audit", "audit how I work", or "quarterly operating audit":
follow the instructions in `skills/operating-audit/SKILL.md`. Extract evidence before any analysis.
Show the deliverable in-session before any file write.
```

### 3. Copy the skill

```bash
mkdir -p skills/operating-audit
cp /path/to/SKILL.md skills/operating-audit/
```

---

## Running

### Manual trigger

```
Operating audit.
```

or

```
Audit how I work.
```

Claude Code starts with Step 1 (evidence extraction), runs through diagnosis, and presents the deliverable in-session. Expect 20–40 minutes for a full audit over a 12-week window.

### Specifying a window

```
Operating audit — last 6 weeks only.
```

```
Run an operating audit for Q1 (January through March).
```

The skill adjusts the daily-notes and log extraction window accordingly.

---

## Evidence extraction

Claude Code handles the three main extraction tasks differently:

### Daily-notes extraction

For windows ≤ 4 weeks, Claude Code reads the files in-session using the Read tool. For longer windows (the default 12 weeks), it delegates to a Sonnet subagent to avoid loading too much into Opus's context:

```
Delegate: read all daily notes from {daily_notes}/ between YYYY-MM-DD and YYYY-MM-DD.
For each day extract: Focus items and completion state, carried items with dates, 
"How today went" key quotes (verbatim), explicit statements of avoidance or friction (verbatim).
Return rollups: items recurring 3+ days without closing, Focus-vs-actual divergences, 
recurring reflection themes.
```

If you don't want subagent delegation, tell Claude Code to process the window in monthly chunks in-session.

### Log extraction

Claude Code runs the targeted bash extraction directly:

```bash
cd {logs}/ && \
for f in $(ls | grep -E '^{YYYY-MM}'); do
  echo "=== $f"
  awk '/^## (Open items|Next actions|Open threads|Pending)/{flag=1; print; next} /^## /{flag=0} flag' "$f" | head -25
done
```

Adjust the date pattern to match your log filename format. The `head -25` limit keeps the output manageable — increase if your pending sections run longer.

### Health-check script

If you have a `{health_script}` configured:

```bash
python3 "{health_script}"
```

Claude Code reads the output and incorporates it as Step 1a telemetry. If the script isn't available or errors, this step is skipped and the audit continues.

---

## Governance evaluation

The dogfood clause requires knowing what governance components exist. Add a note to your `CLAUDE.md` or your system config listing them:

```markdown
## Governance layer

Components I maintain:
- Weekly process brief (generated Monday mornings)
- System health dashboard ({governance}/dashboard.md — updated weekly)
- Triage gate (new project intake checklist in {governance}/triage.md)
```

With this context, Claude Code knows which components to evaluate in Step 1e. Without it, it will ask before the audit runs.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — four-step audit pipeline |
| **Agent** | Claude Code (Opus recommended for the full audit; Sonnet for subagent extraction) |
| **Tools** | File read, Bash (log extraction), optional subagent delegation for bulk reads |

---

## Subagent delegation

For large workspaces (daily notes spanning months, many log files), the extraction step delegates reads to a Sonnet subagent. Claude Code handles this via the Agent tool — the skill's `mcp-bulk-reader` references translate to spawning a subagent with file access.

If you don't have subagent support or prefer in-session reads, tell Claude Code:

```
Run the operating audit without subagent delegation. Process everything in-session.
```

This works — it uses more context and takes longer for large windows.

---

## Saving the deliverable

The deliverable is shown in-session by default. To save it:

```
Save the audit to {workspace}/reviews/operating-audit-YYYY-MM-DD.md
```

Saving the deliverable matters for the dogfood clause: the "two consecutive audits" threshold requires knowing what the previous audit found. If you don't save, the threshold degrades to single-window evaluation.

Recommended location: a dated file in your reviews or archive folder. Include `type: operating-audit` in the frontmatter.

---

## Scheduling

The operating audit is designed for quarterly cadence — alongside a quarterly review if you run one. Claude Code doesn't auto-schedule it; trigger it manually or wire it into your quarterly review checklist.

For a cron-triggered pre-extraction (the bash log scan can run non-interactively):

```bash
# Quarterly — first Sunday of the month
# Runs log extraction only; the full audit needs interactive back-and-forth
0 9 1-7 1,4,7,10 0 cd /path/to/notes && \
  for f in $(ls agent-logs/ | grep -E '^$(date -d "-90 days" +%Y)'); do \
    echo "=== $f"; \
    awk '/^## (Open items|Next actions)/{flag=1; print; next} /^## /{flag=0} flag' "agent-logs/$f" | head -25; \
  done > /tmp/audit-log-extract-$(date +%F).txt
```

Then start the interactive audit: "Operating audit — I've pre-extracted the logs at `/tmp/audit-log-extract-YYYY-MM-DD.txt`. Use that for Step 1c."

---

## Tips

**Save each audit deliverable.** The dogfood clause's two-consecutive-audits threshold requires a prior audit to compare against. No saved deliverable means no threshold tracking.

**Don't skip Step 1 even when you think you know the findings.** The point of evidence-first extraction is to surface patterns you don't already know. The findings you think you already know aren't the audit's value — the ones you didn't expect are.

**Push back on readings.** The audit is opinionated and will sometimes get the reading wrong. Correct it during the deliverable review — the evidence extraction is deterministic, but the diagnosis is interpretive.

**The one thing avoided is the deliverable's sharpest output.** If it doesn't sting a little, it wasn't named sharply enough. Ask the agent to re-examine if the final tension feels comfortable.
