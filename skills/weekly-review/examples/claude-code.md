# Weekly Review — Claude Code

## Setup

### 1. Define your workspace paths

Add to your `CLAUDE.md` or project system prompt:

```markdown
## Weekly Review — path configuration

{workspace}    = /path/to/your/notes/
{inbox}        = {workspace}/00 Inbox/
{projects}     = {workspace}/01 Projects/
{areas}        = {workspace}/02 Areas/
{daily_notes}  = {workspace}/05 Daily/
{weekly_reviews} = {workspace}/reviews/weekly/
{someday}      = {workspace}/01 Projects/Someday-Maybe/
{logs}         = {workspace}/agent-logs/
{timezone}     = Your/Timezone
```

Replace paths with your actual structure.

### 2. Register the skill trigger

Add to your `CLAUDE.md`:

```markdown
### Weekly Review
When I say "weekly review", "/weekly-review", or "let's do the weekly review":
follow the instructions in `skills/weekly-review/SKILL.md`. One phase at a time.
The user decides everything.
```

### 3. Copy the skill

```bash
mkdir -p skills/weekly-review
cp /path/to/SKILL.md skills/weekly-review/
```

---

## Running

### Manual trigger

```
Weekly review.
```

Claude Code will start with pre-flight checks, then work through the six phases. Expect 60–90 minutes for a full review.

### From the CLI (non-interactive)

Not recommended — the weekly review is fundamentally interactive. Phase A's mind sweep, Phase B's routing confirmations, and Phase E's terminal sweep all require back-and-forth. Running headless produces a shallow output.

If you need to pre-seed the inbox scan or daily-notes extraction non-interactively, use a separate `--print` invocation for that phase only:

```bash
claude --print "Run only the weekly review Phase A collection (Steps 1–2 only — inbox + daily notes). 
Output the assembled list. Do not start the mind sweep. Stop after the list." \
>> /tmp/weekly-collection.txt
```

Then start the interactive review and paste the pre-collected list as context.

---

## Calendar integration

The weekly review's Steps 5–6 (previous and upcoming calendar) require reading calendar data. Claude Code doesn't have native calendar access — wire it via MCP or a script:

**Option A: Calendar MCP server.** If you have a calendar MCP configured, add it to your `CLAUDE.md` allowed tools. The skill's `mcp__calendar__list_events` calls will resolve automatically.

**Option B: Script-based export.** Export the week's events to a temp file before the review:

```bash
# Example: export Google Calendar via gcalcli
gcalcli --nocolor agenda "$(date -d 'last monday' +%Y-%m-%d)" "$(date +%Y-%m-%d)" \
  --details all > /tmp/last-week-calendar.txt
gcalcli --nocolor agenda "$(date +%Y-%m-%d)" "$(date -d 'next sunday' +%Y-%m-%d)" \
  --details all > /tmp/next-week-calendar.txt
```

Then at Step 5/6: "Read `/tmp/last-week-calendar.txt` for the previous calendar review."

**Option C: Skip it.** If calendar integration is too much friction, skip Steps 5–6. The review still works — you lose calendar-triggered captures, which are often significant, but everything else runs.

---

## Pre-flight checks

The skill runs three pre-flight checks automatically. With Claude Code:

**Stale task metadata:** runs as an in-session bash command. No setup needed.

**Unprocessed meeting notes:** if you have a `meeting-notes-process` skill, it's invoked here. If not, the skill prompts you to manually extract actions from recent unprocessed meeting notes.

**Analog captures:** if you use a paper or handwritten capture system and have a digital OCR or scan workflow, check for any OCR'd files in your inbox before starting. The skill scans `{inbox}/` for unprocessed analog capture files (detection pattern configurable — defaults to `OCR - *.md`).

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — six-phase GTD review pipeline |
| **Agent** | Claude Code (Opus recommended for the full review; Sonnet for subagent delegation) |
| **Tools** | File read/write, shell access, calendar MCP (optional) |

---

## Subagent delegation

For large workspaces (many projects, many daily notes), the skill delegates some reads to a smaller model to avoid expensive context loads. Claude Code handles this natively — the skill's `mcp-bulk-reader` references translate to spawning a Sonnet subagent via the Agent tool.

If you don't have subagent support configured, the skill falls back to in-session reads. It works — it's just slower and uses more context.

**Skip delegation for small workspaces:** ≤ 3 inbox items AND ≤ 5 active projects AND ≤ 10 daily-note unprocessed items. The spawn overhead doesn't pay back; do everything in-session.

---

## Tips

**Run on Friday afternoon or Sunday evening.** End-of-week timing means the week's record is complete. Sunday evening means Monday starts with the week-ahead signal already in place.

**Don't split phases across sessions.** The phases build on each other — the mind sweep primes the project review, the project review primes Get Creative. Splitting across multiple Claude Code sessions loses that coherence.

**Cache the project landscape for the whole review.** The Phase B project scan (bash grep for outcomes + open tasks) is cached and reused across Phases B, C, and E. Don't re-run the scan — use the cached output.

**The mind sweep is the bottleneck.** Everything else in Phase A can be parallelized. The mind sweep can't — it's a conversation. Don't rush it. Close each thread before moving to the next.
