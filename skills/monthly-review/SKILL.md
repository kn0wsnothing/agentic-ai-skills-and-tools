---
name: monthly-review
description: >
  Interactive monthly reflective review. Re-reads the window's daily reflections,
  runs two-pass AI theme/pattern extraction and goals-vs-actual gap analysis,
  re-validates projects and areas at altitude, and closes with a "how the month
  went" paragraph plus a forward signal for next month. One step at a time.
  Hard gate every step.
triggers:
  - monthly review
  - /monthly-review
  - let's do the monthly review
  - first of the month review
version: 1.0.0
---

# Monthly Review

Reflective end-of-month close. **Reflection, not system validation** — weekly already handles tactical work. Curious prompts, not evaluative ones. Opinionated drafts, user-as-final-arbiter. **One step at a time. Hard gate every step. Never auto-advance. Do not rush.**

## Where This Fits

- **Upstream:** the review window's daily notes, episodic logs, memory writes, the previous monthly review, and any paper journal migration ritual you use.
- **Downstream:** next month's planning ingests the forward signal this skill writes; gap data captured here feeds the quarterly review's goals rewrite.
- **Siblings:**
  - **weekly-review** handles tactical loops (next actions, waiting-for, calendar). This skill **must not duplicate that work**.
  - **day-closure** handles single-day reflection.
  - **quarterly-review** is where goals get rewritten using three monthlies' gap data. This skill captures; quarterly acts.

## Altitude This Skill Owns

| Altitude | Treatment |
|---|---|
| Runway / next actions | not in scope (weekly's job) |
| 10k — projects | **own** — shape / altitude / attention re-validation |
| 20k — areas | **touch** — neglect / overload sweep, capture observations for quarterly |
| 30k — goals | **gap surface only** — capture for quarterly, do not rewrite |
| 40k — vision | not in scope |
| 50k — purpose & principles | not in scope |

## What This Skill Is NOT

- **No next-actions grooming.** Project state changes here are about *shape, altitude, and attention* — not producing next-action checkboxes.
- **No waiting-for chasing.** That's weekly's job.
- **No calendar prep.** Out of scope.
- **No goals rewrites.** Gaps are captured, not acted on. Quarterly is where goals get rewritten.
- **No vision or principles edits.** Those defer to the annual review.
- **No area charter rewrites.** Capture drift; quarterly rewrites charters.

---

## Before Starting

Get the current date (`RUN_YYYY_MM_DD`). The month being reviewed is the calendar month *just ended* (running on 2026-06-01 reviews May 2026). Compute:

- `MONTH_YYYY_MM` (e.g. `2026-05`)
- `MONTH_NAME` (e.g. `May 2026`)
- `WINDOW_START` = first day of the reviewed month minus 5 days (e.g. `2026-04-26`)
- `WINDOW_END` = last day of the reviewed month (e.g. `2026-05-31`)

The 5-day overlap into the prior month captures end-of-prior-month context that bleeds into the current month's reflections.

Create the monthly review note:

1. Read your monthly review template (or use the structure in SPEC.md)
2. Replace placeholders with `MONTH_YYYY_MM`, `MONTH_NAME`, `WINDOW_START`, `WINDOW_END`, and `RUN_YYYY_MM_DD`
3. Write to `{workspace}/03 Reference/Monthly reviews/{RUN_YYYY_MM_DD}.md` — this is the review's permanent home; it is written and completed in place, not moved on close.

Set the note's frontmatter `feeds_quarterly: true` so the quarterly skill can collect three of these.

Check for conflict copies in your inbox or daily notes folder. If found: stop until resolved.

### Pre-flight: paper journal reconciliation (optional)

If you use a paper journal alongside digital notes, scan your inbox for any unreconciled paper captures before continuing. Process oldest-first. Continue to Phase A only after reconciliation is complete.

### Pre-flight: prior monthly review read

Read the previous monthly review note (most recent file in `{workspace}/03 Reference/Monthly reviews/` by filename). If none exists (first run), state so and continue. The prior review's *forward signal* and surfaced themes are part of the input bundle for both AI analyses.

---

## Phase A — Pre-flight

### Step 1: Confirm scope

State to the user:

> "Reviewing **{MONTH_NAME}**. Window: {WINDOW_START} → {WINDOW_END}. Prior monthly review: {prior path or 'none — first run'}. Forward signal carried in: {one-line quote from prior review's signal, or 'none'}. Three outputs: cleaned project/area state, one-paragraph 'how the month went,' and one forward signal/theme. Take the time it takes."

Gate: **"Confirm to start Phase B (re-reading the window's reflections)?"**

---

## Phase B — The Reflection Re-read

### Step 2: Read the window's daily notes in order

Read each daily note from `{workspace}/05 Daily/{WINDOW_START}.md` through `{workspace}/05 Daily/{WINDOW_END}.md`. For each note, read only:

- `## Focus` (what was prioritised)
- `## Notes` (what was captured / observed)
- `## How today went` (the daily reflection)

Skip days with no daily note silently. Do not summarise yet. Do not pattern-detect yet. **Just read.**

After reading: present a compact table with two columns — *Date* and *one-line lift* (the most resonant single sentence from each day's `## How today went`, quoted verbatim if present, or `(no reflection)` if absent). One row per daily note in the window. This is the user's re-read aid, not a summary.

Gate: **"Re-read complete. Anything jumping out before I run the AI passes? (You can name a theme, a tension, a thread — or just say 'continue'.)"**

If the user names anything, capture verbatim into a scratch list — it gets fed into Pass 1A as a *user-named priors* input.

---

## Phase C — Theme and Pattern Analysis

### Step 3: Build the input bundle

Assemble (read once, hold for both analyses):

1. The window's daily notes (Notes, Focus, How today went) — already read in Phase B.
2. The window's episodic logs: files in `{workspace}/90 Memory/episodic/` with date in [WINDOW_START, WINDOW_END]. (Skip if you don't maintain episodic logs.)
3. New feedback / preference / project memories saved in the window: any memory files with `created` or `last_modified` in [WINDOW_START, WINDOW_END].
4. The previous monthly review note (already read in pre-flight).
5. Your goals file and vision file (e.g., `{workspace}/02 Areas/Horizons/Goals.md` and `Vision.md`).
6. User-named priors from Step 2 (if any).

State to the user: "Input bundle assembled — {N} daily notes, {N} episodic logs, {N} new memories, prior monthly review {present/absent}, goals + vision, and {N} user-named priors. Running Pass 1A."

### Step 4: Pass 1A — theme extraction

Run extraction. Produce themes in **six buckets**, in this order:

- **Continuing.** Themes named in the prior monthly review and still showing up. One citation enough.
- **Diminishing.** Prior themes with fewer or zero occurrences vs prior month. Frame as a *signal*, not absence-of-data.
- **Resolved.** Prior themes plus explicit closure language ("done with X", "moved past Y").
- **New.** ≥3 occurrences this month, no prior history. **High-bar bucket** — if borderline, route to Weak signals instead.
- **Weak signals.** 1–2 occurrences, no prior history. Named for next-month watching, **not** promoted.
- **Absences.** Areas/goals named in your goals file with **zero occurrences** in the window.

**Output rules** (all themes):

- Two-column format: *What you wrote* (verbatim quote, dated) | *My read* (interpretation).
- **Quote before paraphrase, every time.** No paraphrase without a quote.
- Every theme cites at least one specific date with quoted source text.
- Surface explicit self-corrections — places where the user revised an earlier read across the window. Flag these distinctly: `↻ revision`.
- Keep buckets compact: max 4 themes per bucket. If more candidates, demote borderline ones to Weak signals or drop.

Present the extraction. Do **not** ask the user to validate yet — Pass 1B runs first.

### Step 5: Pass 1B — devil's advocate on Pass 1A

Same input bundle + Pass 1A's output. Argue *against* the first pass:

- What's overconfident?
- What's underweighted?
- Where did revisions get missed?
- What got promoted to a bucket it doesn't belong in (especially: New that should be Weak signals; Resolved that should be Diminishing)?
- What "Absence" is actually "happening off-record" rather than "not happening"?

Present Pass 1B as a delta on Pass 1A — keep / demote / promote / drop / reframe — not a re-list.

### Step 6: User walkthrough — themes

Walk through each theme one at a time. For each: present {bucket, what-you-wrote, my-read, devil's-advocate note}. Ask:

> "Accept as is / edit my-read / move to different bucket / drop / hold for next month?"

Numbered list. **Nothing auto-commits.** The user's edited version is what gets written.

After all themes processed: write the accepted themes to the monthly review note under `## Themes` with sub-headings per bucket. Drop empty buckets silently.

Gate: **"Themes recorded. Ready for the goals-vs-actual gap analysis?"**

---

## Phase D — Goals-vs-Actual Gap

**Goals are not edited here.** This phase captures gaps for the quarterly review to act on. The point is to *see* the gap clearly, not to fix it under monthly time pressure.

### Step 7: Pass 2A — gap extraction

Same input bundle. Focus: your goals file against the window's actual reflections, episodic logs, and memory writes.

Surface **gaps and contradictions** between stated goals and how time was actually spent. Frame rules:

- **Frame as gaps, not failures.** "X is a stated goal; the window surfaced no work toward it" — not "you failed X".
- Same citation rules: quote before paraphrase, dated source.
- Distinguish three gap types:
  - **Stated but absent.** Goal in goals file, zero or near-zero occurrences in the window.
  - **Stated but contradicted.** Goal in goals file, but window shows behaviour or decisions actively pulling the other direction.
  - **Acted but unstated.** Significant theme/behaviour in the window with no corresponding goal in the goals file — possibly the goal needs to be stated.

Present extraction. Do not validate yet.

### Step 8: Pass 2B — devil's advocate on Pass 2A

Critique Pass 2A:

- Where is the gap framing too tight?
- Where is "no occurrence" being treated as "no progress" when work was happening off-record (e.g., reading, thinking, conversations not journalled)?
- Where is a stated goal **stale** (should be rewritten or dropped) rather than **unmet**?
- Where is an "Acted but unstated" pattern actually scaffolding for an existing goal, not a new one?

Present as a delta.

### Step 9: User walkthrough — gaps (capture only)

Walk through each gap one at a time. For each: present {gap-type, stated-goal, actual-pattern, my-read, devil's-advocate note}. Ask:

> "Accept the gap framing / edit / reframe as 'goal is stale' / reframe as 'goal needs to be added' / drop / hold for next month?"

Numbered list. Capture each disposition.

Write accepted gaps to the monthly review note under `## Goals vs actual` with the three sub-categories. Drop empty categories silently.

State to the user at the end:

> "Gaps captured. Goals will not be edited tonight — quarterly review is where the rewrite happens, with three monthlies' worth of gap data as input. The point of this phase is to *see* the gap clearly."

Gate: **"Gaps recorded. Ready for the projects review?"**

---

## Phase E — Projects

**Different lens than weekly.** Weekly asks "what's the next action?" Monthly asks: **is this still the right shape, the right altitude, the right amount of attention?**

### Step 10: Active projects — shape and altitude

List `{workspace}/01 Projects/Active/`. For each project, surface only:

- Outcome (one line).
- Last activity date (most recent file mtime in the project folder, or last session log entry).
- Months active.

For each, ask one of these (do not ask all, pick the most pointed):

- *Shape:* "Is the outcome still the right outcome?"
- *Altitude:* "Is this still a project, or has it become an area / a someday / a single next action?"
- *Attention:* "Is this getting the right amount of attention — too much, too little, about right?"

Disposition options: keep as is / reshape outcome / move to On hold / move to Someday-Maybe / close / promote to area.

Walk one at a time. Numbered list. Execute moves only on explicit user confirmation.

### Step 11: On hold and Someday-Maybe sweep

List `{workspace}/01 Projects/On hold/` and `{workspace}/01 Projects/Someday-Maybe/`. Surface anything parked >2 months. Ask: still relevant / drop / reactivate? Execute on confirmation.

Gate: **"Projects re-validated. Ready for the areas sweep?"**

---

## Phase F — Areas

Monthly *touches* areas — surfaces neglect, overload, and charter drift. **Does not edit area charters.** Quarterly is where charters get rewritten.

### Step 12: Areas sweep

Read `{workspace}/02 Areas/` top-level structure. For each area:

Surface for the user:

- Window mentions (count of daily-note references where this area is named).
- Project activity in the window (count of active projects belonging to this area).
- Last charter touch (mtime of area's charter file if it exists).

For each area, ask one of these (pick the most pointed — do not ask all):

- "Has this area been *neglected* this window? If yes — is that the right call (other priorities) or a drift?"
- "Has this area been *overloaded* — taking more attention than its share? If yes — what wants to come down?"
- "Has the *charter* drifted? Capture the drift; quarterly will rewrite."

If an area is healthy and unsurprising, say so and move on. This phase is reflective, not transactional.

Capture observations into the monthly review note under `## Areas observations`, tagged with area name. **Do not edit area charters or area files.**

Gate: **"Areas swept. Ready for the optional phases?"**

---

## Phase G — Optional: Paper Journal Migration Walk

Skip this phase if you don't use a paper journal alongside digital notes.

### Step 13: Surface unfinished items

Collect unfinished items from three sources:

- **Source 1:** Unprocessed items in the window's daily notes (unticked checkboxes, unprocessed bullets).
- **Source 2:** Carry-forward items from the prior monthly review's migration list (items marked "carry to next month").
- **Source 3:** Items carried >1 month — flag these with `⚠ carried >1 month`.

Present the union as a numbered list, each item tagged by source.

### Step 14: Migration walk — one item at a time

For each item, ask:

> "Migrate forward / drop / project-it / someday?"

If `⚠ carried >1 month`, prepend:

> "This has been carried for >1 month. **Is it still relevant?** If yes, what's blocking it — and is the block worth resolving, or is the right call to drop it?"

Capture the full migration decision list under `## Journal migration` in the monthly review note.

State at the end: "Migration list ready. Carry these into your next monthly spread."

Gate: **"Migration recorded. Ready for the optional publish phase?"**

---

## Phase H — Optional: Public Status Update

**This phase is a hook for your own publish workflow. The skill provides the logic; you wire the destination.**

If you maintain a public-facing status page or changelog (e.g., a `/now` page, a changelog, a newsletter update), surface candidates from the window:

- Active project state changes (completed, paused, newly started).
- New themes or learnings from Phase C that are publicly safe.
- New reading or learning.

Walk each candidate. For each, ask: "Include in status update / skip?"

**No writes happen here until you confirm.** The skill surfaces candidates and drafts entries; you approve, edit, or drop each one. When ready, write to your publish target — the exact path and format are yours to configure.

If you don't maintain a public status update, **skip this phase entirely.**

Gate: **"Optional phases done. Ready for the close?"**

---

## Phase I — Outputs

Three outputs only. Two are written here; the third (cleaned project/area state) was the *mutation* of Phases E and F and is already done.

### Step 17: "How the month went"

Read the themes (Phase C) and gaps (Phase D) accepted by the user. Draft a **single first-person paragraph** — not a list, not bullets, not headings. Curious tone, not evaluative. State what happened, what it pointed at, what the month felt like. **Do not draft a verdict on whether the month was "good" or "bad."**

Aim for a paragraph dense enough to be a reflection (not a label) and short enough to re-read in a sitting.

Present the draft.

> "Here's how {MONTH_NAME} went — edit or approve?"

On approval: write under `## How the month went` in the monthly review note.

### Step 18: Forward signal / theme

Draft **one** signal for next month. Not a goal. Not a plan. A *theme* or *open question* that names what next month is about. One or two sentences max.

Acceptable signal shapes:

- A question to live with: "What does it look like to stop optimising X?"
- A theme to test: "Less surface, more depth on the AI work."
- A constraint to honour: "No new commitments until {project} ships."

Unacceptable shapes:

- A list of priorities (that's weekly's job)
- A goal restatement (that's the goals file)
- A vague feeling ("Be more intentional")

Present the draft.

> "Here's the forward signal for next month — edit or approve?"

On approval:

1. Write under `## Forward signal` in the monthly review note.
2. Write the same signal to next month's first daily note (create it from your daily note template if it doesn't exist). Insert under a `## Monthly signal` block so it carries through the month.

Gate: **"Outputs written. Ready to close?"**

---

## Phase J — Close

### Step 19: Finalize and breadcrumb

1. The review note lives at `{workspace}/03 Reference/Monthly reviews/{RUN_YYYY_MM_DD}.md`. Confirm it's complete and its frontmatter carries `feeds_quarterly: true`.
2. Append a one-line breadcrumb to today's daily note: `HH:MM — Monthly review for {MONTH_NAME} complete. {one-line gist of forward signal} #monthly-review`
3. If you maintain an episodic log, append the same line there.

State: **"Monthly review for {MONTH_NAME} closed."**

---

## Rules

- **INTERACTIVE. ALWAYS.** Every step requires input before proceeding.
- **Hard gate mechanic.** End every step with the gate question and stop completely.
- **One step at a time.** Numbered lists for all decision points.
- **Do not rush.** Take the time each phase needs. There is no time cap.
- **Reflective, not evaluative.** Prompts are *curious* — they ask what happened and what it pointed at, not whether it was good or bad. If a prompt starts to read as self-scolding, rewrite it before asking.
- **Quote before paraphrase.** All AI surfacing cites dated, quoted source text before any interpretation.
- **User-as-final-arbiter.** Nothing AI-generated auto-commits. Themes, gaps, migration decisions, the "how the month went" paragraph, and the forward signal all need explicit user approval — and the version that gets written is the user's edited version, not the LLM's draft.
- **Hard separation from weekly.** No next-actions grooming, no waiting-for chasing, no calendar prep.
- **Goals are not edited at monthly altitude.** Gaps are captured for quarterly's rewrite. Vision, principles, and area charters are also not edited here.
- **Conditional firing for optional phases.** No paper journal → skip Phase G. No public status page → skip Phase H.
- **File writes:** Read full file first, make changes, write back. Always verify.
- **Timestamp protocol:** before writing any `HH:MM`, run `date +"%H:%M"` in your timezone. Never estimate.
- **Review window:** the calendar month *just ended*, plus a 5-day overlap into the prior month.

### Correction checkpoint

When the user corrects routing, content, framing, AI surfacing, or process:

1. Stop current flow completely.
2. Name the error category explicitly (citation missing, paraphrase before quote, wrong bucket, evaluative framing, scope creep into weekly territory, scope creep into quarterly/annual territory, area charter touched, etc.).
3. State corrected disposition or reading.
4. Ask for confirmation on the fix.
5. **Do not proceed to the next step until the correction is acknowledged and executed.**

If a correction reveals a recurring AI failure mode, capture it as a feedback memory at session close.
