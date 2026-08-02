# Quarterly review — design spec

This document explains the design decisions behind the Quarterly Review skill — who it's for, what problem it solves, how the phases were structured, and how to adapt it to your own system.

---

## The problem

A monthly review is good at *seeing* drift — a goal that's gone quiet, an area getting overloaded, a theme that's persisted for weeks. It is deliberately bad at *acting* on what it sees, because a month of material and a life to get back to is the wrong condition under which to rewrite something as load-bearing as your goals or your area charters. Rewriting under that pressure produces hasty, reactive changes that get re-rewritten next month anyway.

Quarterly review exists to hold the acting until there's enough evidence to act well. Three monthlies' worth of gap data is enough distance to see a pattern rather than react to a mood. This skill is where that data finally gets spent.

---

## The altitude framework

The skill uses the same horizon-based altitude model as its monthly sibling, borrowed from GTD, but owns a different slice of it:

| Altitude | Name | Treatment |
|---|---|---|
| Runway | Next actions | Not in scope — weekly's job |
| Projects | 10k ft | Not in scope — monthly's job |
| Areas | 20k ft | **Owned** — charter rewrite, attention rebalance |
| Goals | 30k ft | **Owned** — full rewrite using three monthlies' gap data |
| Vision | 40k ft | **Touched** — read-only, tension captured for annual |
| Purpose & principles | 50k ft | Not in scope — annual's job, if you run one |

**Why this allocation?** Monthly owns the middle layer (projects, areas as observation); quarterly owns the layer above where actual rewrites belong (areas as charter, goals). Vision and purpose sit one level higher still and are deliberately left for an annual cadence — a quarter is not enough distance to responsibly rewrite a 3–5 year horizon, but it is enough to notice when the two have started to diverge, which is exactly what the Vision-touch phase captures.

---

## Why charters get rewritten here, not monthly

An area charter — purpose, scope, current attention intent — is a foundational document in the same category as the goals document, just smaller. It shouldn't be edited on a monthly cadence for the same reason goals shouldn't: not enough evidence, too much reactivity. Monthly's areas sweep exists specifically to accumulate *drift observations* without touching the charter itself, so that by the time quarterly runs, there are three months of "neglected" or "overloaded" signal to draft from — or, on a first run, nothing at all, which is why charter bootstrap is built into Phase C rather than treated as a separate setup step.

## Why the goals rewrite is arbitrated line by line

A wholesale "here's your new goals document, approve or reject" gate hides the actual decisions inside a single yes/no. Each goal's disposition — keep, rewrite, add, drop — has different evidence behind it and deserves its own moment of judgment. The "explain the why in 30 seconds" test exists because a goal that survives a rewrite only because no one looked closely at it isn't a real goal; it's inertia wearing one.

## Why two AI passes, not four

Monthly review runs four passes (extraction + devil's advocate, twice) because it's working from raw daily material that hasn't been synthesized yet. Quarterly's input — three already-synthesized monthly reviews — has already been through that heavier lift. Quarterly's two passes (cross-monthly synthesis + devil's advocate) are aggregation and escalation-detection: which signals showed up more than once, which got promoted on thin evidence, which single-month signal is actually the leading edge of something real. A lighter pass structure matches the lighter-lift input.

## Why the standing-commitment checkpoint is optional and disposition-only

Not everyone runs a long-term side commitment that needs protecting from day-to-day urgency — but for anyone who does (a venture, a significant personal project, a major life change), checking it at only one altitude is a trap: checking it weekly means daily urgency always wins, checking it only annually means a full year can pass on an assumption that stopped being true in month two. Three altitudes (weekly, quarterly, annual) with quarterly reading the *actual lived allocation* against the commitment's own stated re-evaluation triggers gives it a fair hearing without turning quarterly into a project-planning session — the phase produces a disposition (stay, re-activate, adjust), never a rebuilt task list.

## Why vision is read-only here

Vision editing needs the same distance argument taken one step further — a full year's accumulated quarterlies, not one quarter's. What quarterly can responsibly do is notice and name a *tension*: does the life actually being lived still point at the vision that was written down? Naming that tension four times a year, without acting on it, is what makes a future annual review's vision rewrite (if you run one) evidence-based rather than a blank-page exercise.

---

## The three hard boundaries

1. **Quarterly/Monthly boundary.** No daily-note re-reads, no theme extraction from raw days, no journal migration, no project portfolio review. Quarterly reads the three *finished* monthlies — if something needs raw-day evidence, that's monthly's job, already done.

2. **Quarterly/Annual boundary.** Vision and purpose are not rewritten here. Quarterly only captures a tension line for an annual-level review, if the user runs one, to act on with a full year's worth of these lines.

3. **Quarterly/Weekly boundary.** No next-actions grooming, no waiting-for chasing. The forward theme this skill produces is deliberately not a list of priorities — that's the weekly review's job to turn into one.

---

## The forward theme

Like monthly's forward signal, this is not a goal, a plan, or a priority list — it's a strategic theme naming what the *next quarter* is about, at the altitude this skill owns. The distinction from monthly's signal is scale: a monthly signal names what a month is about; a quarterly theme names what a quarter is about, and should be durable enough to still make sense in month three of the quarter, not just week one.

Acceptable shapes:
- A strategic bet to test: "This quarter proves whether the side project earns real weekend hours or stays a someday."
- A rebalance to hold: "Bring health up from the floor it hit last quarter without letting the core work cadence slip."
- A constraint to honour at quarter scale: "No new area commitments until the charter rebalance settles."

Unacceptable shapes: a list of priorities (weekly's job), a goal restatement (the goals document's job), or a month-scale signal dressed up as a quarter-scale one.

---

## Customization points

### Path structure

| Default path | What it is | How to change |
|---|---|---|
| `{workspace}/03 Reference/Quarterly reviews/` | Review note output | Any directory you want to keep quarterlies in permanently — they should never be archived, since a future annual review needs four on hand |
| `{workspace}/02 Areas/Horizons/Goals.md` | Goals document | Any file that lists your current goals |
| `{workspace}/02 Areas/Horizons/Vision.md` | Vision document | Any file that holds your longer-horizon intent |
| `{workspace}/02 Areas/{Area}/{Area}.md` | Area hub notes | Match your own area-note structure; the charter block can live anywhere consistent per area |
| `{workspace}/03 Reference/Monthly reviews/` | Monthly review input | Wherever your monthly-review skill writes its notes |

### Conditional phases

**Phase E (standing-commitment checkpoint)** only applies if you separately track a long-running commitment that competes with day-to-day work. Skip it entirely otherwise — there's no partial version of this phase worth running without a real commitment and decision log behind it.

**Phase G (audit commitments)**, and the corresponding precondition in Phase A, only apply if you run the operating-audit skill. Skip both entirely if you don't.

### Quarterly review note template

The skill expects a template with placeholders for `QUARTER`, `QUARTER_MONTHS`, and `RUN_YYYY_MM_DD`. A minimal structure:

```markdown
---
type: quarterly-review
quarter: QUARTER
run_date: RUN_YYYY_MM_DD
feeds_annual: true
vision_tension: ""
---

# Quarterly review — QUARTER

## Quarter themes

## Rituals

## Areas / charter changes

## Goals rewrite record

## Venture checkpoint

## Vision tension

## Audit commitments

## How the quarter went

## Forward theme

```

Rename "Venture checkpoint" to whatever your own standing commitment is called, or drop the section if Phase E doesn't apply to you.

---

## Integration with other reviews

```
Daily notes (daily)
    ↓
Weekly review (weekly) — next actions, waiting-for, calendar
    ↓
Monthly review (monthly, first of the month) — captures gaps, doesn't act
    ↓ feeds three months of gap data
Quarterly review (quarterly, first of the quarter) — this skill: rewrites goals, area charters
    ↓ feeds vision-tension lines
Annual review (annually, if you run one) — rewrites vision, purpose, principles
```

Each level consumes outputs from the level below it. This skill's outputs — the rewritten goals document, rewritten charters, and the forward theme — feed next quarter's monthly reviews and weekly reviews directly; the captured vision tension is the one output that waits an entire year to be spent.

---

## Known limitations

- **Model dependency.** The charter and goals rewrites require real judgment about what evidence is strong versus thin. A lightweight model will tend to accept synthesized claims at face value rather than push back in the devil's-advocate pass — this is why the skill hard-gates on model choice before starting.
- **Garbage in, garbage out from monthly.** This skill's synthesis is only as good as the three monthly reviews feeding it. If a monthly review skipped its own devil's-advocate pass or the user rubber-stamped it, quarterly inherits that weak signal without a way to independently verify it.
- **The standing-commitment checkpoint assumes a decision log exists.** Without one, Phase E has no re-evaluation triggers to check against and degenerates into a vaguer "how do you feel about this" prompt. Worth keeping a lightweight decision log for anything you want checked this way.
- **Read-only vision touch can defer real tension indefinitely.** Because vision is never rewritten here, a genuine, recurring mismatch between lived life and stated vision can get captured as a `vision_tension` line every quarter for a year without anything actually changing until the annual review runs. If you don't run an annual-level review, build in some other backstop so these lines don't just accumulate unread.
