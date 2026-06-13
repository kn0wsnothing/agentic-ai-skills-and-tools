# Monthly Review

A structured reflective review skill for AI agents. Runs at month-end to re-read the window's daily reflections, extract themes and patterns, surface goals-vs-actual gaps, re-validate projects and areas at altitude, and close with a one-paragraph "how the month went" plus a forward signal for next month.

This is **reflection, not system validation.** Weekly reviews handle tactical loops. This skill owns the altitude above that.

---

## What It Does

1. **Re-reads the month** — scans daily notes in the review window, presents a compact table of daily reflections as a re-read aid
2. **Theme extraction** — two-pass AI analysis (extraction + devil's advocate) across six buckets: Continuing, Diminishing, Resolved, New, Weak signals, Absences
3. **Goals-vs-actual gap analysis** — surfaces gaps between stated goals and how time was actually spent; captures for quarterly, never edits goals here
4. **Project re-validation** — shape, altitude, and attention sweep for each active project; moves on explicit confirmation
5. **Areas sweep** — surfaces neglect, overload, and charter drift; observation-only, no charter edits
6. **Optional: paper journal migration** — walk unfinished items through a migration decision list
7. **Optional: public status update** — hook to your own publish workflow (see Phase H in SKILL.md)
8. **Outputs** — "how the month went" paragraph + one forward signal/theme written to next month's first daily note

---

## When to Trigger

- First day of the new month
- Triggers: "monthly review", "/monthly-review", "let's do the monthly", "first of the month review"

---

## Prerequisites

- `{workspace}/` — your root knowledge-base directory (Obsidian vault, Logseq graph, plain markdown directory, etc.)
- Daily notes in `{workspace}/05 Daily/YYYY-MM-DD.md` (or equivalent)
- A goals/vision file the agent can read for gap analysis
- An active projects list the agent can walk
- A monthly review note template (or adapt the one in SPEC.md)
- Episodic memory directory (optional but recommended — see memory structure below)

---

## Memory Structure

```
{workspace}/
├── 01 Projects/
│   ├── Active/          # Projects swept in Phase E
│   ├── On hold/
│   └── Someday-Maybe/
├── 02 Areas/            # Areas swept in Phase F
├── 03 Reference/
│   └── Monthly reviews/ # Review notes written here
├── 05 Daily/
│   └── YYYY-MM-DD.md    # Review window source material
└── 90 Memory/
    └── episodic/        # Optional: episodic logs in review window
```

Adapt paths to your own structure — see SPEC.md for customization guidance.

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic (any LLM agent) | [examples/generic.md](examples/generic.md) |

---

## Design Rationale

See [SPEC.md](SPEC.md) for the full altitude framework, phase structure rationale, and customization points.

---

## Pair With

- **weekly-review** — handles tactical loops (next actions, waiting-for, calendar prep). Monthly review must not duplicate this.
- **day-closure** — handles single-day reflection.
- **quarterly-review** — where goals get rewritten using three monthlies' gap data. Monthly captures; quarterly acts.
