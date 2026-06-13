# Weekly Review

A structured weekly review skill for AI agents. Follows GTD's canonical Get Clear → Get Current → Get Creative arc, closed by a terminal inbox sweep. Opinionated about process — the agent recommends, you decide.

This is a flagship skill. It takes 60–90 minutes done properly, and is designed to not be skipped.

---

## How it works

Six phases run in sequence:

1. **Get Clear (Collection)** — pull every inbox and your head to zero. Vault inbox, recent daily notes, and a full Socratic mind sweep. No routing in this phase — just collection.
2. **Get Clear (Clarify)** — one batch of dispositions for everything collected. Active projects landscape first, then proposals for each item. Hard gate before any write.
3. **Get Current** — bring your workspace to 80–90% match with reality. Review action lists, previous and upcoming calendar, waiting-fors, project health. New actions captured to inbox raw — no inline routing.
4. **Get Creative** — surface latent threads and half-formed ideas before they disappear. Someday/Maybe triage + an agent-led brainstorm from real sources.
5. **Second Clarify** — everything the review itself generated gets routed to zero. Same batch-and-confirm machinery as phase 2.
6. **Close** — "How last week went" reflection + week-ahead signal written to next week's daily note.

---

## The inbox empties — twice

The non-negotiable rule: your inbox reaches zero at the end of Phase 2 (before the project review) and again at the end of Phase 5 (after the review generates new captures). If anything remains unrouted at either gate, the skill hasn't finished.

There is no "sit on this for now" disposition. If an item is genuinely unclear, it routes to Someday/Maybe, a project's Notes section, or an area hub — never back to the inbox.

---

## Prerequisites

Your agent needs:

- **File read/write access** to your notes workspace
- **An inbox** — a single file or folder where unprocessed items land
- **An active projects list** — folders or notes with outcome-stated project names
- **Daily notes** — one file per day covering the past 7 days
- **A calendar** — previous and upcoming events for the review (platform-specific — see examples)
- **A clock** — the skill timestamps writes; get wall-clock time, don't estimate

---

## Key principles

**GTD naming, enforced.** Projects are named as outcomes — something with a clear "done" criterion. Tasks are verb-led. Topic labels ("analytics work", "vendor question") are not next actions — ask what action moves it forward.

**Quote-or-don't-name.** Every named entity — person, project, decision, deadline — must be paired with the verbatim source line it came from. If you can't quote it, you don't have it.

**Capture, don't clarify, during Get Current and Get Creative.** New actions surfaced in phases 3–4 go to the inbox raw. All routing happens in phases 2 and 5. This keeps the review in review-mode instead of bouncing in and out of clarify-mode.

**Keyword overlap is not project fit.** A task doesn't belong in a project just because they share a noun. Match against the project's outcome — if the task doesn't move that outcome forward, propose a different home.

---

## Quick start

1. **Copy `SKILL.md`** into your agent's persistent instructions
2. **Configure the paths** (see Configuration below)
3. **Run manually first** — say "weekly review" and work through all six phases
4. **Review the outputs** — check the inbox is truly empty, all active projects have a next action, and the week-ahead signal is accurate
5. **Schedule or trigger weekly** — Friday afternoon or Sunday evening works well

---

## Configuration

| Parameter | Example | Description |
|-----------|---------|-------------|
| `{workspace}` | `~/notes/` | Root of your notes or PKM vault |
| `{inbox}` | `{workspace}/inbox/` | Where unprocessed items land |
| `{projects}` | `{workspace}/projects/active/` | Active project notes |
| `{daily_notes}` | `{workspace}/daily/` | Daily note files (YYYY-MM-DD.md) |
| `{weekly_reviews}` | `{workspace}/reviews/` | Where completed weekly review notes are archived |
| `{someday}` | `{workspace}/projects/someday/` | Someday/Maybe list or folder |
| `{areas}` | `{workspace}/areas/` | Ongoing responsibilities (not outcome-bound) |
| `{logs}` | `{workspace}/agent-logs/` | Agent session logs |
| `{timezone}` | your timezone | For timestamps and calendar queries |

Define these once in your agent's system prompt or equivalent config file.

---

## File layout

```
{workspace}/
├── inbox/
│   └── inbox.md              # Unprocessed captures — empties twice per review
├── projects/
│   ├── active/
│   │   └── {project}/
│   │       └── {project}.md  # Next actions, waiting-fors, session log
│   ├── on-hold/
│   └── someday/
├── areas/
│   └── {area}.md
├── daily/
│   └── YYYY-MM-DD.md         # Focus, notes, breadcrumbs
├── reviews/
│   └── YYYY-MM-DD.md         # Weekly review notes (archived after close)
└── agent-logs/
    └── YYYY-MM-DD-{slug}.md
```

---

## Hard gates (non-negotiable)

The skill has hard stop-and-confirm gates at:

- End of Phase 1 (Collection): assembled list presented, user prunes or approves
- Phase 2: routing batch confirmed before any write
- Phase 2: inbox verification confirmed (inbox at zero)
- Phase 5: terminal-sweep batch confirmed before any write
- Phase 5: inbox verification confirmed (inbox at zero again)
- Phase 5: every Active project has a `[status:: next]` task
- Phase 6: "How last week went" draft approved before write
- Phase 6: week-ahead signal approved before write

All other inter-step pauses are dropped. The review moves.

---

## Best practices

**Don't rush the mind sweep.** The Socratic mind sweep (Phase 1, Step 3) is the load-bearing capture step. The agent draws things out through questioning, not by reading category names back. One thread live at a time, follow each to completion. This is the part of the review that earns its cost.

**One batch per phase, one confirmation.** Don't confirm item by item. The whole routing batch confirms at once — faster, and it forces holistic review of the dispositions.

**Correction checkpoint.** When you correct the agent's routing, content, or framing, it stops completely, names the error category, states the corrected reading, and confirms the fix before continuing. Corrections compound in a review — catch them fast.

**The review generates its own inbox.** Everything Get Current and Get Creative surfaces lands in the inbox raw. Phase 5 processes it to zero. This means a thorough review produces a second inbox that's often larger than the first.

---

## Platform examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic (any file-access agent) | [examples/generic.md](examples/generic.md) |

---

## FAQ

**How is this different from a simpler "review my tasks" prompt?**
Three things: the mind sweep empties your head, not just your lists. The inbox-empties-twice rule means nothing slips through. And the week-ahead signal writes into next week's planning anchor so the review compounds forward.

**60–90 minutes every week seems like a lot.**
Yes. That's the point. A shorter review that doesn't empty your head, doesn't triage waiting-fors, and doesn't ensure every project has a next action is a false economy. The review is the weekly reset — if it's not taking real time, something is being skipped.

**What if I have no calendar access?**
Skip Steps 5–6 (previous + upcoming calendar). The review still works — you lose the calendar-triggered capture, but everything else runs. Worth adding calendar access; it's where the most forgotten follow-ups hide.

**Can I run phases separately across multiple sessions?**
Not recommended. The value comes from moving through the whole arc in one sitting — the mind sweep primes the project review, the project review primes the creative phase. Splitting across days loses that coherence.

**What's the "correction checkpoint"?**
When you correct any routing or framing during the review, the skill stops, names the error category explicitly, states the fix, and waits for confirmation before proceeding. It exists because errors in routing compound — one wrong project fit can cascade into bad next-action placement.

---

## Design rationale

See [SPEC.md](SPEC.md) for the design decisions: the two-inbox rule, why the mind sweep uses elenchus not enumeration, why phases 3–4 capture-don't-clarify, and how the terminal sweep closes the loop.
