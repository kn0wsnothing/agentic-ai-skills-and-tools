# Monthly Review — Claude Code

## Setup

### 1. Install the skill

```bash
mkdir -p {workspace}/skills/monthly-review
cp /path/to/SKILL.md {workspace}/skills/monthly-review/
```

### 2. Add to CLAUDE.md

```markdown
### Monthly Review
When asked to run the monthly review (or triggered on the first of the month),
follow the instructions in `skills/monthly-review/SKILL.md`. Interactive —
one step at a time, hard gate every step.
```

### 3. Configure your paths

Open `SKILL.md` and set `{workspace}` to your actual root knowledge-base directory. Update any path that doesn't match your structure:

| Default | Your equivalent |
|---|---|
| `{workspace}/05 Daily/YYYY-MM-DD.md` | Your daily note path |
| `{workspace}/02 Areas/Horizons/Goals.md` | Your goals file |
| `{workspace}/01 Projects/Active/` | Your active projects directory |
| `{workspace}/03 Reference/Monthly reviews/` | Where monthly reviews are archived |

---

## Running

### Manual (first of the month)

```
Run the monthly review. Follow skills/monthly-review/SKILL.md.
```

The agent will:
1. Compute the review window (last month + 5-day overlap)
2. Create the monthly review note from your template
3. Walk through all phases interactively — you gate every step

**Typical session length: 45–90 minutes** depending on how many projects/areas you have and how much you want to discuss.

### Scheduled (unattended start, interactive completion)

You can schedule the setup steps to run automatically and then complete the review interactively:

```bash
# Cron job — runs on the 1st of each month at 8am
0 8 1 * * cd /path/to/workspace && claude --print --model claude-opus-4-6 \
  "Start the monthly review. Create the note and run pre-flight only. Stop at the Phase A gate." \
  >> /var/log/monthly-review.log 2>&1
```

The agent creates the review note and halts. You then pick up interactively.

---

## What the Agent Reads

For May 2026 (running June 1):

- `05 Daily/2026-04-26.md` through `05 Daily/2026-05-31.md` — the full review window (36 files max; fewer if some days are missing)
- `03 Reference/Monthly reviews/` — most recent file (prior monthly review)
- `90 Memory/episodic/` — files dated 2026-04-26 to 2026-05-31 (if present)
- `02 Areas/Horizons/Goals.md` and `Vision.md`
- Each project folder under `01 Projects/Active/`
- `02 Areas/` top-level

---

## What It Produces

A review note at `03 Reference/Monthly reviews/2026-06-01.md`:

```markdown
---
type: monthly-review
month: 2026-05
window_start: 2026-04-26
window_end: 2026-05-31
run_date: 2026-06-01
feeds_quarterly: true
---

# Monthly review — May 2026

## Themes

### Continuing
| What you wrote | My read |
|---|---|
| "Finally making progress on the architecture work" (2026-05-14) | The systems-building thread from April carried through |

### New
| What you wrote | My read |
|---|---|
| "Started pairing with a colleague regularly" (2026-05-07, 05-15, 05-22) | Mentoring/collaboration emerged as a genuine thread |

### Weak signals
| What you wrote | My read |
|---|---|
| "Skipped the run again" (2026-05-09) | Physical routine showing friction — watch next month |

### Absences
| Goal | Window occurrences |
|---|---|
| "Publish one essay per month" | 0 mentions |

## Goals vs actual

### Stated but absent
- "Publish one essay per month" — no writing activity in the window.
  *Devil's advocate: essay work may have happened in reading/thinking not journalled. Check.*

### Acted but unstated
- Regular mentoring/pairing with colleagues — significant recurring theme with no corresponding goal in Goals.md.

## Areas observations
- **Writing pipeline:** zero published pieces in window; Drafts folder has 3 in-progress items, none moved.
- **Work:** high project load in first three weeks; eased in final week.
- **Health:** physical routine flagged as weak signal — not crisis, but worth watching.

## How the month went

May had a paradox in it: the work I cared most about — the architecture project — finally got real traction, but the writing that I claim is an equal priority saw nothing move. The mentoring thread surprised me; it didn't appear in any plan but showed up in the notes consistently enough to be real. The physical routine dipped without becoming a thing. The month felt productive in the ways I can measure and thin in the ways I can't.

## Forward signal

What does it look like to treat writing as infrastructure work — scheduled, protected, not squeezed into margins?

## Journal migration

1. Draft essay on systems thinking — migrate forward
2. Reply to old email re: collaboration — dropped (stale)
3. Set up weekly writing block on calendar — project-it
```

Plus a breadcrumb on today's daily note:
```
09:42 — Monthly review for May 2026 complete. Signal: treat writing as infrastructure work. #monthly-review
```

---

## WAT Mapping

| Layer | Implementation |
|---|---|
| **Workflow** | `SKILL.md` — the 10-phase interactive review pipeline |
| **Agent** | Claude (Opus recommended for the two-pass AI analysis; quality matters) |
| **Tools** | Claude Code's file read/write, directory listing, shell access |

---

## Tips

- **Opus for this one.** The theme extraction and devil's advocate passes involve nuanced editorial judgment across many files. A weaker model will miss self-corrections, over-promote weak signals, and write evaluative prompts despite the rules.
- **Let it take time.** The review is designed to be 45–90 minutes. Rushing produces shallow output. The gate questions are there to slow things down, not to be bypassed.
- **User-named priors matter.** When the agent presents the re-read table (Step 2) and asks if anything is jumping out, take it seriously. The themes you name before the AI runs its pass are the correction mechanism for off-record activity the AI can't see.
- **The forward signal is the deliverable.** Everything else in the review is context for this. If the forward signal is vague or feels like a restatement of a goal, push back and demand a real question or constraint.
