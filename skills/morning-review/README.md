# Morning Review

An interactive daily review skill for AI agents. Drains inboxes, reconciles yesterday's open loops, and sets today's priorities — in one opinionated, guided session.

---

## What It Does

Four phases, run in sequence:

1. **Context** — reads yesterday's forward signal, today's calendar, and system health
2. **Collection** — batch-scans all sources: meeting notes, inbox, overdue items, open loops from a grounded mind sweep
3. **Clarify** — proposes dispositions for every collected item against the active project landscape; user confirms or overrides the whole batch
4. **Plan the day** — week look-ahead, waiting-for check, and Focus-setting with explicit user confirmation

The inbox empties. The Focus is set. The run either finishes or it hasn't finished.

---

## Key Principles

**Batch collect, then clarify.** The agent gathers everything before proposing any routing. No mid-collection routing decisions. The collection list is shown, the user can prune or add, then clarification happens in one pass.

**Opinionated.** The agent proposes specific dispositions — project routes, dates, energy levels — grounded in each project's stated outcome. The user confirms or overrides. The agent never asks "what do you want to do with this?".

**Inbox empties.** Every item collected gets a disposition. There is no "sit on this for now" exit. If something is genuinely unclear, the right disposition is a named project's Notes section, an area hub, or an archive parking lot — never the inbox.

**GTD naming.** Projects are named as outcomes (something with a clear "done" state). Tasks are verb-led. Topic labels are not next actions.

**No fabrication.** Every named entity in a proposal — person, project, deadline — must be paired with a verbatim source line. If the agent can't quote the source, it doesn't have it.

**Focus requires explicit confirmation.** The agent recommends Focus items but does not write them until the user names the list.

---

## Prerequisites

- **File read/write access** to your workspace
- **Calendar access** (optional but recommended for meeting-load awareness)
- **A structured workspace** with inboxes, project notes, and a daily note — see layout below

---

## Workspace Layout

The skill uses these paths, configurable via `{workspace}`:

```
{workspace}/
├── Inbox/                        # Unprocessed captures (items drain here)
├── Projects/Active/              # Outcome-bound projects (each has a main note)
├── Areas/                        # Ongoing responsibilities
├── Daily/YYYY-MM-DD.md          # Today's daily note (Focus, Notes, context)
├── Meetings/                     # Meeting notes (moved to Processed/ after review)
│   └── Processed/YYYY-MM/
└── System/Dashboards/            # System health dashboard (optional)
```

Adapt paths to your own structure — the skill references them as configurable in SKILL.md.

---

## Configuration

| Parameter | Description |
|-----------|-------------|
| `WORKSPACE` | Root path of your note workspace |
| `INBOX_FILE` | Path to your unprocessed inbox file |
| `PROJECTS_DIR` | Directory containing active projects |
| `AREAS_DIR` | Directory containing ongoing areas |
| `MEETINGS_DIR` | Directory for meeting notes |
| `DAILY_NOTE_PATTERN` | Path pattern for daily notes (`YYYY-MM-DD.md`) |
| `SYSTEM_HEALTH_PATH` | Optional system health dashboard |
| `CALENDAR` | Your calendar(s), configurable per platform |
| `TIMEZONE` | Your local timezone |

---

## Quick Start

1. Copy `SKILL.md` into your agent's skill directory
2. Set the paths in your agent config (CLAUDE.md, system prompt, or equivalent)
3. Trigger with: "morning review" / "/morning-review" / "let's do the review"

See the platform examples for setup specifics.

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic / other platforms | [examples/generic.md](examples/generic.md) |

---

## Design Rationale

See [SPEC.md](SPEC.md) for the full design rationale — including the batch-collect-then-clarify model, the GTD naming enforcement, the open-loops sweep design, and how this skill fits into a daily workflow cycle.

---

## Related Skills

This skill is designed as one part of a daily workflow loop:

- **work-closure** — end-of-session closure; writes the forward signal that morning review reads
- **day-closure** — end-of-day close; captures "how today went" and seeds tomorrow's context
- **weekly-review** — same clarification pattern at weekly cadence; deeper mind sweep, project-level triage
