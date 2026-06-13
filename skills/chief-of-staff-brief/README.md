# Chief-of-Staff Brief

A scheduled sense-and-pre-draft skill for AI agents. Runs before your weekly review, reads the state of the system, and writes a one-page brief with queued decisions, pre-drafted nudges, and an allocation read — so review starts at decisions, not collection.

The brief **proposes**. You **dispose**. Nothing here sends a message, moves a project, or mutates anything outside the agent's own workspace.

---

## What It Does

The skill reads four signal sources and distills them into a structured one-page brief:

1. **System health signals** — the state of your open loops, waiting-fors, stalled items
2. **Last week's session logs** — what actually happened, classified by type
3. **Last week's work closures** — absorbed work, triage items, in-flight carry-overs
4. **Standing intent** — your top priorities and what the week was earmarked for

The output is a single brief file with five sections:

- **Decisions queued** — the 3–6 things review must dispose of, sharpest first
- **Nudges pre-drafted** — one per stale waiting-for (>7d), ready to send in your register
- **Park questions** — only when triggered: checkpoint misses, zombie items, things that need a hard look
- **Allocation — actual vs intent** — did last week's margin go where it was earmarked?
- **Biggest unclosed loop** — one sentence; the thing that costs most by staying open

---

## Prerequisites

Your agent needs:

- **File read/write access** to a workspace directory
- **A system health file** or equivalent status dashboard — a markdown file regenerated regularly with open loops, stale items, and waiting-fors
- **Session logs** — dated markdown files capturing what happened in agent sessions
- **Work closure records** — end-of-day or end-of-session logs noting what was absorbed, what's in-flight, what's triaged
- **A standing intent record** — a file noting your current top priorities (weekly "Big 3" equivalent, monthly theme)
- **A scheduling mechanism** — cron, Task Scheduler, platform-native scheduling, or manual trigger

---

## Output Structure

Writes to `{workspace}/briefs/YYYY-MM-DD-chief-of-staff.md`. One page hard cap. Structure:

```markdown
---
type: chief-of-staff-brief
date: YYYY-MM-DD
generated_by: chief-of-staff-brief skill
---

# Chief-of-staff brief — YYYY-MM-DD

## Decisions queued
{3–6 items, one line each, sharpest first.}

## Nudges pre-drafted
{One per stale waiting-for. Item, age, ready-to-send one-liner in your register.}

## Park questions
{Only if triggered: checkpoint misses or zombie items. State the question plainly.}

## Allocation — actual vs intent
{Two or three sentences. Did margin go where it was earmarked?}

## Biggest unclosed loop
{One sentence.}
```

---

## Quick Start

1. **Set up the inputs** — system health file, session logs, work closure logs, standing intent record
2. **Copy `SKILL.md`** into your agent's persistent instructions
3. **Configure the paths** in the Configuration table (see SKILL.md)
4. **Run manually first** — trigger it before your next weekly review and inspect the output
5. **Schedule it** — run early Monday (or whatever day your weekly review falls on), before you sit down

---

## Operating Principles

**Sense and pre-draft only.** The brief does not send messages, park projects, mutate tasks, or write outside its designated output directory. Every action lands as a queued decision for you to act on.

**Evidence-cited.** Every claim traces to a health signal, a log entry, or a closure line. No vibes, no guessing.

**One page.** If it doesn't fit, cut. The brief is what changes a decision, not a status report.

**Headless-safe.** When run on schedule with no human present, it never blocks on a question. It writes the brief with best-judgment content and flags uncertainties inline.

**Dogfood clause.** If the brief consistently fails to change real decisions, it should be deleted. Its only claim: review starts at decisions, not collection; stale loops get chased weekly instead of drifting.

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic scheduling | [examples/cron-schedule.md](examples/cron-schedule.md) |

---

## Design Rationale

See [SPEC.md](SPEC.md) for the full design rationale, including the pre-draft model, the evidence constraint, and the headless-safe requirement.
