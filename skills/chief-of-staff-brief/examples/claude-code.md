# Chief-of-Staff Brief — Claude Code

## Setup

### 1. Add to CLAUDE.md

```markdown
### Chief-of-Staff Brief
When asked to run a chief-of-staff brief (or on a schedule), follow the instructions in
`skills/chief-of-staff-brief/SKILL.md`. This sweeps the system state and writes a
one-page brief with queued decisions, pre-drafted nudges, and an allocation read.
Do not send anything, move any projects, or mutate any tasks — only write the brief file.
```

### 2. Copy the skill

```bash
mkdir -p skills/chief-of-staff-brief
cp /path/to/SKILL.md skills/chief-of-staff-brief/
```

### 3. Set up the required inputs

The skill reads four sources. Configure paths to match your setup:

| Input | What it is | Example path |
|-------|-----------|-------------|
| System health file | A markdown file regenerated regularly with your open loops, stale items, and waiting-fors | `workspace/system-health.md` |
| Session logs | Dated markdown files in a directory — one per agent session | `workspace/logs/YYYY-MM-DD-slug.md` |
| Work closure records | End-of-session logs noting what was absorbed, in-flight, and triaged | `workspace/closures/YYYY-MM-DD.md` |
| Standing intent | A file noting your current top priorities | `workspace/standing-intent.md` |

If you don't have some of these yet, start with what you have. The skill will note what's missing inline rather than failing.

---

## Running

### Manual (before weekly review)

```
Run the chief-of-staff brief. Follow skills/chief-of-staff-brief/SKILL.md.
My workspace is at ~/workspace. Today is YYYY-MM-DD.
```

Or more specifically:

```
Run the chief-of-staff brief following skills/chief-of-staff-brief/SKILL.md.
Health file: ~/workspace/system-health.md
Logs: ~/workspace/logs/ (last 7 days)
Closures: ~/workspace/closures/ (last 7 days)
Intent: ~/workspace/standing-intent.md
Write the brief to ~/workspace/briefs/
```

### Automated (Monday morning)

```bash
# Cron job — runs Monday at 6am
0 6 * * 1 cd /path/to/workspace && claude --print --model claude-opus-4-6 \
  "Run the chief-of-staff brief following skills/chief-of-staff-brief/SKILL.md. \
   Workspace: /path/to/workspace. Today is $(date +%Y-%m-%d). \
   Write the brief to /path/to/workspace/briefs/." \
  >> /var/log/chief-of-staff-brief.log 2>&1
```

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the sweep-and-pre-draft pipeline |
| **Agent** | Claude (Opus recommended for quality of editorial judgment) |
| **Tools** | Claude Code's file read/write access |

---

## Tips

**Read access is all the agent needs.** Except for the single brief write, the skill is read-only. It reads your logs, health file, closures, and intent — then writes one output file. No task mutations, no sends, nothing else.

**Start with the manual trigger.** Run it manually before your next weekly review and check the output. It takes one or two runs to calibrate — you'll want to adjust the health file format or intent file structure to match what the brief wants to surface.

**The brief is only as good as its inputs.** If your system health file is stale or your intent file is vague, the brief reflects that. The skill will flag staleness but can't synthesize good outputs from thin inputs.

**Headless runs flag uncertainty inline.** When the skill runs on schedule and something is ambiguous (a log's timestamps are missing, the health file seems old), it writes the brief anyway and notes the issue. Check the brief before your review, not after.
