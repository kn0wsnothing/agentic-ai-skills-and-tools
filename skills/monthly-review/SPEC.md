# Monthly Review — Design Spec

This document explains the design decisions behind the Monthly Review skill — who it's for, what problem it solves, how the phases were structured, and how to adapt it to your own system.

---

## The Problem

Most productivity systems have good tooling at the weekly level (next actions, waiting-for, calendar prep) and at the annual level (goals, vision). The monthly level is often skipped or merged into an extended weekly review — which means it inherits the wrong lens. Weekly asks "what's next?" Monthly should ask "is this still the right thing to be doing, and at what altitude?"

Without a dedicated monthly review, patterns accumulate invisibly. A goal drifts for three months before anyone notices. A project stays "active" long after it should have been parked. The gap between what you said you'd focus on and what you actually did never gets examined.

This skill makes that examination structured and friction-low.

---

## The Altitude Framework

The skill uses a horizon-based altitude model, borrowed from GTD:

| Altitude | Name | Treatment |
|---|---|---|
| Runway | Next actions | Not in scope — weekly's job |
| 10k ft | Projects | **Owned** — shape, altitude, attention re-validation |
| 20k ft | Areas | **Touched** — neglect/overload sweep, observations for quarterly |
| 30k ft | Goals | **Gap surface only** — captured for quarterly, never rewritten |
| 40k ft | Vision | Not in scope |
| 50k ft | Purpose & principles | Not in scope |

**Why this allocation?** Each altitude has the right review cadence. Next actions need weekly attention; vision and purpose need annual. Monthly owns the middle layer: projects and areas. Goals are surfaced but not acted on — you need three months of gap data before rewriting a goal has enough signal to be worth doing.

---

## Phase Structure Rationale

### Why re-read before analysis (Phase B before C)?

The re-read phase (Phase B) is not redundant with the AI analysis in Phase C. It serves a different purpose: giving the user their own re-contact with the month before the AI runs its pass. The compact table of daily reflections is a *re-read aid*, not a summary. The user often names themes or tensions that the AI would miss or underweight — these become "user-named priors" that feed into the AI analysis as a corrective.

### Why two AI passes with a devil's advocate?

Single-pass extraction is overconfident. The first pass will tend to over-promote (calling "New" what should be "Weak signal"), miss self-corrections within the window, and treat absence-of-mention as absence-of-activity. The devil's advocate pass (1B, 2B) argues against the first pass before the user sees either — so the user walks through a pre-critiqued set of themes rather than having to generate the critique themselves.

### Why "capture, not act" for goals?

Monthly reviews operate under time pressure — there's a month of material to process and a life to get back to. Rewriting goals under this pressure produces hasty, reactive changes. The quarterly review has the right conditions: three months of gap data, enough distance to see patterns rather than recency bias. Monthly captures; quarterly acts. This is a hard constraint, not a suggestion.

### Why the user-as-final-arbiter rule?

Every AI-surfaced theme, gap, migration decision, and output paragraph needs explicit user approval, and the version that gets written is the user's edited version — not the LLM's draft. This is because:

1. The AI doesn't have access to off-record context (conversations, thinking, reading that wasn't journalled).
2. The AI will construct confident narratives from incomplete signals.
3. The user's own voice in the "how the month went" paragraph matters — it's a record they'll re-read, not a report someone else wrote about them.

---

## The Three Hard Boundaries

1. **Monthly/Weekly boundary.** No next-actions grooming, no waiting-for chasing, no calendar prep. If it belongs in a weekly review, it doesn't belong here.

2. **Monthly/Quarterly boundary.** Goals, area charters, vision, and principles are not edited at monthly altitude. These documents get rewritten at quarterly with three months of input. Monthly's job is to surface the gap, not close it.

3. **Monthly/Annual boundary.** Vision and purpose-level documents defer to the annual review. Monthly doesn't touch them.

These boundaries exist because mixing altitudes in a single review produces confusion about what's being decided and at what timescale.

---

## The Forward Signal

The forward signal is the most important output of the review. It's not a goal, not a plan, and not a list of priorities. It's a theme or question that names what next month is *about* — written in one or two sentences, in the user's own voice.

The signal gets written to two places: the monthly review note (permanent record) and next month's first daily note (so it carries through the month as a standing context item).

Acceptable shapes:
- A question to live with: "What does it look like to stop optimising for output?"
- A theme to test: "Less surface, more depth."
- A constraint to honour: "No new commitments until X ships."

Unacceptable shapes:
- A list of priorities (that's weekly)
- A goal restatement (that's the goals file)
- A vague sentiment ("Be more intentional") — too diffuse to act as a navigational signal

---

## Customization Points

### Path structure

The skill uses a specific directory layout as defaults. You can adapt all paths:

| Default path | What it is | How to change |
|---|---|---|
| `{workspace}/05 Daily/YYYY-MM-DD.md` | Daily notes | Match your daily note naming convention |
| `{workspace}/03 Reference/Monthly reviews/` | Review note output | Any directory you want to archive monthly reviews |
| `{workspace}/02 Areas/Horizons/Goals.md` | Goals file | Any file that lists your current goals |
| `{workspace}/02 Areas/Horizons/Vision.md` | Vision file | Any file that holds your longer-horizon intent |
| `{workspace}/01 Projects/Active/` | Active projects | Match your project directory |
| `{workspace}/90 Memory/episodic/` | Episodic logs | Optional — skip if you don't maintain these |

### Review window

Default: the calendar month just ended, plus a 5-day overlap into the prior month. The overlap captures end-of-month context that bleeds into the current month's reflections. You can shrink or eliminate the overlap if your daily notes are consistent.

### Optional phases

**Phase G (paper journal migration)** only applies if you maintain a paper journal alongside digital notes. Skip it if you don't.

**Phase H (public status update)** only applies if you maintain a public-facing status page, changelog, or similar. Skip it if you don't. If you do maintain one, you'll need to configure the target path and entry format for your own system — the skill surfaces candidates and drafts entries, but does not prescribe the destination.

### Optional personal reflection hook

After the forward signal is approved (Step 18), you can add a personal reflection hook — a practice, ritual, or prompt you use to mark the month's close. This is intentionally left as a blank space in the skill: the skill closes the *system*, the practice closes the *self*, and the two are different enough that one shouldn't be prescribed for the other.

### Monthly review note template

The skill expects a template with placeholders for `MONTH_YYYY_MM`, `MONTH_NAME`, `WINDOW_START`, `WINDOW_END`, and `RUN_YYYY_MM_DD`. A minimal structure:

```markdown
---
type: monthly-review
month: MONTH_YYYY_MM
window_start: WINDOW_START
window_end: WINDOW_END
run_date: RUN_YYYY_MM_DD
feeds_quarterly: true
---

# Monthly review — MONTH_NAME

## Themes

## Goals vs actual

## Areas observations

## How the month went

## Forward signal

## Journal migration

```

---

## Integration with Other Reviews

```
Daily notes (daily)
    ↓
Weekly review (weekly) — next actions, waiting-for, calendar
    ↓
Monthly review (monthly, first of the month) — this skill
    ↓ feeds gap data
Quarterly review (quarterly) — rewrites goals, area charters
    ↓ feeds vision inputs
Annual review (annually) — rewrites vision, purpose, principles
```

Each level consumes outputs from the level below it. Monthly review's outputs — gap data (`feeds_quarterly: true`) and the forward signal — are explicitly designed to be consumed by the quarterly review and next month's planning respectively.

---

## Known Limitations

- **Off-record activity.** The AI analysis only sees what was written in daily notes and logs. Reading, thinking, conversations that weren't journalled are invisible to it. The devil's advocate pass and the user-named priors mechanism partially compensate, but the fundamental limitation remains.
- **Mtime as proxy for activity.** The projects sweep uses file modification times to estimate last activity. This is a proxy — a project can be actively worked without generating file changes, or files can be touched by automated processes without real activity. Treat mtime as a signal, not a ground truth.
- **No deduplication across months.** If the same theme appears in two consecutive monthly reviews, it will show up as "Continuing" correctly. But the AI won't automatically detect when a theme has been "Continuing" for six months and should be escalated to a goals rewrite — that judgment requires the quarterly review.
