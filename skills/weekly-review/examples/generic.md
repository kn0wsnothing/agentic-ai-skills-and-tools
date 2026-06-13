# Weekly Review — Generic Setup

This guide covers setup for any file-access agent: custom orchestrators, SDK-based agents, local LLM setups, or any platform not specifically listed.

---

## What your agent needs

The weekly review is an interactive, multi-phase skill. Minimum requirements:

- **File read/write** — the agent reads daily notes, project files, meeting notes, inbox; writes dispositions, project updates, the weekly review note, and next Monday's daily note
- **Shell/bash access** (optional but recommended) — used for the stale-task metadata grep and the project landscape scan; can be replaced with pure-file alternatives at the cost of more reads
- **Calendar access** (optional) — Steps 5–6 (previous + upcoming calendar); skip these steps if not available
- **Interactive back-channel** — the user must be able to respond mid-session; non-interactive runs produce shallow output

---

## Workspace structure

The skill uses path placeholders. Define these once and reference them everywhere:

| Placeholder | What it points to | Example |
|-------------|-------------------|---------|
| `{workspace}` | Root of your notes | `~/notes/` |
| `{inbox}` | Where unprocessed captures land | `{workspace}/inbox/` |
| `{projects}` | Active (and on-hold) project notes | `{workspace}/projects/` |
| `{areas}` | Ongoing responsibilities | `{workspace}/areas/` |
| `{someday}` | Someday/Maybe list or folder | `{workspace}/projects/someday/` |
| `{daily_notes}` | Daily note files (YYYY-MM-DD.md) | `{workspace}/daily/` |
| `{weekly_reviews}` | Completed weekly review archive | `{workspace}/reviews/` |
| `{logs}` | Agent session logs | `{workspace}/logs/` |
| `{templates}` | Note templates | `{workspace}/templates/` |
| `{timezone}` | Your local timezone | `America/Chicago` |

Put these in your agent's system prompt or a persistent config file it loads every session.

---

## Project file conventions

The skill expects project notes to follow a consistent structure. Minimal working format:

```markdown
---
outcome: Verb-led outcome statement — what "done" looks like
status: active | on-hold | someday
---

# Project name

## Next actions
- [ ] Verb-led task [status:: next]
- [x] Completed task [status:: done] ✅ YYYY-MM-DD

## Waiting for
- [ ] Thing I'm waiting on — from: Person Name [status:: waiting] [since:: YYYY-MM-DD]

## Notes
Running notes, context, decisions.

## Session log
- YYYY-MM-DD — What happened in this session
```

The `outcome:` frontmatter field is required — Phase B routing matches tasks against it. Without it, the landscape scan shows `<no outcome>` and routing falls back to name-matching (which the skill explicitly discourages).

---

## Inbox conventions

The inbox is a single file plus an optional folder:

```
{inbox}/
├── inbox.md          # Main capture file — one bullet per item
└── *.md              # Loose files waiting for routing (attachments, captures, etc.)
```

The inbox file format:
```
- Verb-led capture or observation
- Another item, possibly with a source note
```

After Phase B: `inbox.md` is blank. After Phase E: `inbox.md` is blank again.

---

## Daily notes

One file per day, named `YYYY-MM-DD.md`. Minimum structure:

```markdown
# YYYY-MM-DD

## Focus
Items I'm committing to today.

## Notes
Captures, observations, follow-ups from the day.

## How today went
End-of-day reflection (optional — but the weekly review mines this section).
```

The skill reads the last 7 days' `## Notes` sections during Step 2 and the last 14 days for the mind-sweep priming. It also writes the week-ahead signal to next Monday's daily note at the end of the review.

---

## Replacing bash with file reads

If your agent doesn't have shell access, the bash-based steps can be replaced:

**Stale-task metadata scan (pre-flight):** read each project file and scan for the pattern `[x] ... [status:: next]`. Report matches, offer to update in bulk.

**Project landscape (Phase B):** read each project's main `.md` file and extract the `outcome:` field and all lines matching `status:: next` or `status:: waiting`. Build the same landscape manually.

This is slower and uses more context, but produces equivalent results. The bash version is preferred for large workspaces (10+ projects).

---

## Calendar integration

Steps 5–6 require access to the previous and upcoming week's calendar events.

**Option A: Pre-exported file.** Before starting the review, export your calendar to a temp file:
- Google Calendar: use `gcalcli agenda` or export via the Calendar API
- Apple Calendar: export to `.ics` and parse, or use a CalDAV client
- Any calendar: export the week's events to a plain-text or markdown file

Tell the agent: "Read `/tmp/this-week-events.txt` for the previous calendar review."

**Option B: Calendar MCP.** If your agent framework supports MCP and you have a calendar MCP server configured, wire it in. The skill references `list_events` calls — map these to your MCP tool names.

**Option C: Manual summary.** At Steps 5–6, the agent asks: "Can you summarize your key meetings from last week and any upcoming commitments for next week?" Works fine — you lose automatic follow-up extraction, gain simplicity.

**Option D: Skip.** If calendar integration isn't available, skip Steps 5–6. The review still works.

---

## Loading the skill

Load `SKILL.md` into your agent's system prompt or as a persistent instruction file. Example for different setups:

**Python (Anthropic SDK):**
```python
from pathlib import Path

skill = Path("skills/weekly-review/SKILL.md").read_text()
workspace_config = """
{workspace} = /path/to/notes/
{inbox} = {workspace}/inbox/
{projects} = {workspace}/projects/
...
"""

messages = [
    {"role": "user", "content": f"{skill}\n\n{workspace_config}\n\nWeekly review."}
]
```

**System prompt injection:**
```
[contents of SKILL.md]

## Path configuration
{workspace} = /your/notes/root/
{inbox} = {workspace}/00 Inbox/
...
```

---

## Triggering the review

The skill responds to natural-language triggers:
- "Weekly review"
- "/weekly-review"
- "Let's do the weekly review"
- "Time for the weekly"

Set up a keyword hook in your agent if your platform supports it.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — six-phase GTD review pipeline |
| **Agent** | Your choice — use the strongest model you have for the full review |
| **Tools** | File read/write + shell (optional) + calendar (optional) |

---

## Scheduling

The weekly review is best run on Friday afternoon or Sunday evening. Scheduling it ensures it actually happens — a review you have to remember to trigger gets skipped.

**Cron (Unix/Linux/macOS):**
```bash
# Sunday at 7pm — starts the review (interactive, so this just opens the session)
# For non-interactive pre-seeding only — full review needs human in the loop
0 19 * * 0 /path/to/your-agent --print \
  "Run weekly review Phase A collection only (inbox + daily notes scan, no mind sweep). Output the assembled list." \
  >> /tmp/weekly-collection-$(date +%F).txt
```

The Phase A collection (inbox + daily notes) can run non-interactively. The mind sweep, Phase B routing, and Phase E terminal sweep require interactive back-and-forth.

**Recommended pattern:** schedule the collection to pre-seed, then start the interactive review with the pre-seeded list as context. Saves 5–10 minutes at the start.

---

## Tips

**Protect the 60–90 minutes.** The review takes as long as it takes because the work requires it. A 20-minute "weekly review" that skips the mind sweep and doesn't empty the inbox isn't a review — it's a shallow task skim. Block the time.

**First run: do it manually.** Don't schedule until you've run the skill manually two or three times and understand what it produces. The first run usually surfaces structural issues (missing `outcome:` fields, inbox format mismatches, template gaps) that need fixing before automation makes sense.

**Read back the week-ahead signal.** The signal written to next Monday's daily note is what Monday's session will see. Read it back at the end of the review and verify it's actually useful — accurate, forward-looking, specific enough to orient a cold session.
