---
name: quarterly-review
description: >
  Interactive quarterly reflective + strategic review, run at the start of a new quarter after the monthly review that closes the quarter. Uses the quarter's three monthly reviews as primary input. Owns the area-charter rewrites and the goals-document rewrite (using three monthlies' accumulated gap data); touches a longer-horizon vision document read-only, capturing tension for a future annual-level review if you run one. Also runs an optional standing-commitment checkpoint and ingests an operating-audit report if you use that skill. Runs two AI passes (cross-monthly synthesis + devil's advocate) and closes with a "how the quarter went" paragraph plus a forward theme that propagates into the weekly review's forward-signal step. Use this skill whenever the user says "quarterly review", "/quarterly-review", "let's do the quarterly", or "first of the quarter review". One step at a time — the user decides everything. Decision steps hard-gate; pure phase-transitions announce-and-continue.
triggers:
  - quarterly review
  - /quarterly-review
  - let's do the quarterly
  - first of the quarter review
version: 1.0.0
---

# Quarterly review

Strategic end-of-quarter close. **Where the accumulated monthly signal becomes action** — three monthlies captured gaps and area drift without editing anything above project altitude; quarterly is where the goals document and area charters actually get rewritten. Reflective at the top, decisive at the altitude it owns. Opinionated drafts, user-as-final-arbiter. **One step at a time. Every decision this skill owns — the charter rewrites, the goals-document rewrite line-by-line, the standing-commitment disposition, the vision tension, the reflection + forward theme, and every outward write — hard-gates and never auto-advances. Pure between-phase transitions ("ready for the next phase?") announce-and-continue. Do not rush.**

## Model

This is one of the highest-stakes reflective skills in this system — it rewrites your goals document and your area charters. Run it on your most capable reasoning model, not a fast/lightweight tier. If the session is already on one, say so and continue. If it isn't, propose the switch and wait:

> "Quarterly review rewrites your goals document and area charters — it deserves your strongest available reasoning model. Switch models before we start, then say go."

Do not begin Phase A until the user confirms.

## Where this fits

- **Upstream:** the quarter's three monthly reviews (marked to feed the quarterly review, however you track that), the previous quarterly review, your goals document, your vision document, the charter block in each area's hub note, the quarter's operating-audit report (if you run that skill), and a standing-commitment project and its decision log, if you keep one.
- **Downstream:** next quarter's first monthly review reads the rewritten goals document as the baseline for its gap analysis. A future annual-level review, if you run one, uses four quarterlies (including their captured vision-tension lines) as primary input. The forward theme written here propagates down: your weekly review's forward-signal step reads it, and any daily pre-review brief you run can carry it as standing intent.
- **Siblings:**
  - **monthly-review** surfaces goal/actual gaps and area observations but deliberately does **not** rewrite the goals document or area charters. Monthly captures; quarterly acts.
  - **A longer-horizon annual review**, if you run one, owns the full vision and purpose rewrites. Quarterly *touches* vision (does the quarter's lived life still match?) but never rewrites it — it only captures a `vision_tension` line for that review to act on.
  - **operating-audit**, if you use it, runs on the same quarterly cadence; quarterly's pre-flight checks that it ran in-window and ingests its report.

## Altitude this skill owns

| Altitude | Treatment |
|---|---|
| Runway / next actions | not in scope (weekly's job) |
| Projects | not in scope (monthly's job) — no portfolio session, no next-actions grooming |
| Areas | **rewrite** the charter block per area; rebalance attention across areas |
| Goals (1–2 year horizon) | **rewrite** the goals document, using the three monthlies' accumulated gap data |
| Vision (longer horizon) | **touch** (read-only check; capture tension as `vision_tension` for a future annual-level review) |
| Purpose & principles | not in scope (an annual review's job, if you run one) |
| Rituals (daily habits) | **touch** — reads the quarter's three monthly ritual records; asks, per ritual, whether it's still the right one — never rates it (that's monthly's job), never recommends dropping it |

## What this skill is NOT

Hard separation from monthly-review:

- **Not a long monthly.** No daily-note re-reads, no theme extraction from raw days, no journal migration, no project portfolio review, no public-status propagation. Those are monthly's job and were already done in the monthly that closed the quarter. Quarterly reads the *three finished monthlies*, not the raw days.
- **Not an annual.** Vision and purpose are not rewritten here. Vision is read-only; purpose is untouched.
- **Not a project-portfolio strategy session.** Project shape, altitude, and attention are monthly's job. The one project-adjacent move quarterly makes is the standing-commitment checkpoint (Phase E) — a strategic disposition, not a portfolio sweep.
- **Not next-actions grooming or waiting-for chasing.** Those are weekly's.
- **Not a rituals rate check.** Weekly counts, monthly reads the shape across weeks; quarterly only asks whether each ritual still belongs.

---

## Before starting

Get the current date (`RUN_YYYY_MM_DD`). The quarter being reviewed is the calendar quarter *just ended* (running on the first of a new quarter reviews the three months just completed). Compute:

- `QUARTER` (e.g. `2026-Q2`)
- `QUARTER_MONTHS` — the three calendar months in the quarter
- The three monthly review notes that cover those months. If your monthly reviews are run-dated on the first of the *following* month, match by each note's stored month field, not by filename arithmetic.

Create the quarterly review note:

1. Read your quarterly review template, or use the section structure implied by this skill (`## Quarter themes`, `## Areas / charter changes`, `## Goals rewrite record`, `## Venture checkpoint` or equivalent, `## Vision tension`, `## Audit commitments`, `## How the quarter went`, `## Forward theme`).
2. Replace placeholders with `QUARTER`, `QUARTER_MONTHS`, and `RUN_YYYY_MM_DD`.
3. Write to `{workspace}/03 Reference/Quarterly reviews/{RUN_YYYY_MM_DD}.md` (e.g. `2026-07-01.md`). This is the review's permanent home; it is written and completed in place, never moved or archived — a future annual review needs four quarterlies on hand.

Set the note's frontmatter to mark it as feeding a future annual review (if you run one), and `vision_tension: ""` (filled in Phase F, left empty if none).

Check for sync-conflict copies in your reference and daily-notes folders. If found: stop until resolved.

---

## Phase A — Pre-flight

### Step 1: Verify the quarter is closable

Two hard preconditions. Check both before anything else:

1. **The closing monthly ran.** The monthly review covering the quarter's last month must exist. If it is missing, **stop**:

   > "The monthly review for {last month of quarter} hasn't run yet. Quarterly reads three finished monthlies — run the monthly review first, then come back. Stopping here."

2. **The operating audit ran in-window** (only if you run the operating-audit skill; skip this precondition entirely if you don't). An operating-audit report must exist dated within the trigger window (the first two weeks of the new quarter). If none in-window, **stop**:

   > "No operating audit has run in the first two weeks of {next quarter}. The quarterly review ingests the audit as an input. Run the operating audit first as its own session, then come back. Stopping here."

Do not offer to run either inline — they are their own sessions. Stop and let the user sequence them.

### Step 2: Assemble the input bundle and confirm scope

Read once, hold for the synthesis passes:

1. The quarter's three monthly reviews — each note's themes, goals-vs-actual, and areas-observations sections, plus its forward signal.
2. The previous quarterly review (most recent file by filename). None on first run — state so.
3. Your goals document (current state, including anything flagged as needing updating).
4. Your vision document (read-only reference for Phase F).
5. The charter block in each area's hub note (may not exist yet — first run bootstraps them).
6. The quarter's operating-audit report, if you use that skill.
7. Any attention-allocation history you track (how attention actually distributed across areas this quarter) — used in Phase C's rebalance.
8. Your standing-commitment project and its decision log, if you keep one — held for Phase E.
9. **Optional concept-search pass**, if you have a semantic search tool over your notes: compose 2–4 queries from the quarter's dominant themes to surface concept-linked tensions that span the whole quarter, threads a month-by-month reading misses. Feeds Step 3's Pass 1 synthesis as *candidate* cross-monthly tensions only — each must still trace to a monthly-sourced, quoted theme before it earns a place in the synthesis, never promoted on the search hit alone. Skip this step entirely if you have no such tool.

State to the user:

> "Reviewing **{QUARTER}**. Three monthlies: {run-dates}. Prior quarterly: {path or 'none — first run'}. Operating audit: {date, in-window, or 'not used'}. Outputs: rewritten area charters, a rewritten goals document, a standing-commitment disposition (if applicable), a captured vision tension, a 'how the quarter went' paragraph, and a forward theme that carries into next quarter's weekly reviews. This is strategic, not tactical — no daily re-reads, no journal migration, no project portfolio (monthly already did those). Take the time it takes."

**Advance (announce-and-continue):** state "Scope set — moving to Phase B, cross-monthly synthesis." and continue in the same turn. (The model-switch confirmation above still stands — do not start until the model is confirmed.)

---

## Phase B — Cross-monthly synthesis

### Step 3: Pass 1 — synthesis across the three monthlies

Run synthesis over the three monthlies' themes, goals-vs-actual, and areas-observations. Produce, in this order:

- **Quarter themes.** Themes that persisted or escalated across two or three of the monthlies — not a re-list of each month's themes, but what held across the quarter. Every theme cites **which monthly said it** (e.g. "surfaced month two and three, absent month one") with the monthly's own quoted read.
- **Aggregated goal gaps.** Collect every entry from the three monthlies' goals-vs-actual sections, deduplicated and grouped by the three gap types (stated-but-absent, stated-but-contradicted, acted-but-unstated). A gap that recurred across two or three monthlies is a **standing gap** — flag it distinctly; those are the ones the goals rewrite most needs to answer. A gap that appeared once and then resolved is noted but demoted.
- **Area observations rollup.** Collect the three monthlies' areas-observations, grouped by area. Note which areas were flagged neglected or overloaded and how consistently. This feeds the Phase C charter rewrite.

**Output rules:**

- **Source before synthesis, every time.** Every claim traces to which monthly (or monthlies) carried it, with the monthly's quoted text. No synthesized claim without a monthly source.
- A theme or gap that appears in only one monthly is a *weak* signal at quarter altitude — name it as such, do not promote it to a standing theme.
- Keep it compact: the three monthlies already did the heavy reflective lifting. This pass is aggregation and escalation-detection, not fresh theme extraction from raw material.
- **Readout budget: roughly 500 words for the whole pass** across the three sections, one or two sentences of synthesis per item. Format anchor, not a limit on what you read.

Present the synthesis. Do **not** ask the user to validate yet — Pass 2 runs first.

### Step 4: Pass 2 — devil's advocate

Same input bundle + Pass 1's output. Argue against the synthesis:

- What got called a "standing" theme/gap on thin evidence (present in two monthlies but one was a near-miss)?
- What single-monthly signal is being underweighted because it only appeared once — but appeared *late* and may be the real trajectory?
- Where is an "aggregated gap" actually already resolving on its own, so a goals-document rewrite would be fixing a closed problem?
- Where is the area rollup treating "less attention" as "neglect" when the quarter's priorities made that the right call?

Present Pass 2 as a delta on Pass 1 — keep / demote / promote / reframe / drop — not a re-list. **Readout budget: roughly 300 words**, numbered list, one or two sentences each.

### Step 5: User walkthrough — synthesis

Walk through each item one at a time (themes, then aggregated gaps, then area rollup). For each: present {source monthlies with quotes, your read, the devil's-advocate note}. Ask:

> "Accept as is / edit my read / reframe / demote to weak signal / drop / hold?"

Numbered list. **Nothing auto-commits.** The user's edited version is what carries into the rewrites.

After the walkthrough: write the accepted synthesis to the quarterly note under `## Quarter themes` (themes) and hold the accepted gaps and area rollup for Phases C and D.

### Rituals — still the right ones?

**Read-only touch, not a rate check.** Weekly already surfaces the per-week counts and monthly already reads the shape across a month's weeks; this skill's job is narrower and different. Read the ritual record from each of the quarter's three monthly reviews and, per ritual, note the pattern across the three: held steady, never really took, or faded partway through — with which monthly showed which, same source-before-synthesis rule as the rest of this phase.

Ask the touch question only where three months of reading is actually informative — a ritual that held steady and unremarkable across all three doesn't need it forced on it:

> "{Ritual} has read as {pattern} across the quarter — is this still the right one to be running, or does it want to change?"

This is a question, never a recommendation — no "consider dropping," no framing a low rate as a discipline problem. Capture whatever the user says under `## Rituals` in the quarterly note, per ritual asked. This phase does not drop, add, or restructure a ritual itself; that disposition, if any, is the user's to carry forward.

**Advance (announce-and-continue):** state "Synthesis recorded — moving to the area-charter rewrites." and continue in the same turn. (Each charter still gates per-area in Step 6.)

---

## Phase C — Areas charter rewrite

This is a foundational-document rewrite. **Gate before every write.** Monthly captured charter drift against nothing; quarterly is where the charter block actually gets established and maintained.

The charter block lives in each area's hub note and has this shape (a few lines, not an essay):

```
## Charter

**Purpose.** Why this area exists — the standing outcome it protects. One or two sentences.

**Scope.** What's in and what's explicitly out. A short list.

**Current attention intent.** How much of your attention this area should get this quarter, and why — stated as a deliberate call, not a description of what happened.

*Last rewritten: {RUN_YYYY_MM_DD} (quarterly review)*
```

### Step 6: Charter rewrite — one area at a time

List your top-level areas (the ones with hub notes). For each area, in turn:

1. Present the current charter block (or "no charter yet — this run bootstraps it").
2. Present the area's rollup from Phase B (neglect/overload observations across the three monthlies, with sources) and its actual attention allocation this quarter if available.
3. Propose the rewritten charter block — purpose, scope, current attention intent — grounded in what the quarter's monthlies observed. On a bootstrap run, propose the whole block from scratch.

Ask:

> "Accept / edit / rewrite from my draft / leave charter as is (capture drift only) / skip this area?"

Wait for the user. **The user's edited version is what gets written**, not the drafted one.

On accept: write the charter block into the area's hub note (create the section if absent; replace it if present). Update the "last rewritten" line to `RUN_YYYY_MM_DD`.

Record each change under `## Areas / charter changes` in the quarterly note: area name, one-line summary of what changed (or "bootstrapped" / "left as is").

### Step 7: Attention rebalance across areas

Having rewritten the individual charters, step back: across all areas, is the attention balance right for next quarter? Surface where the "current attention intent" lines collectively over- or under-commit. This is a single reflective prompt, not a per-area walk:

> "Looking at all the charters together — does the attention balance across areas hold for next quarter, or does something need to come down so something else can come up?"

Capture the answer under `## Areas / charter changes` as a closing note. No file writes here beyond what Step 6 already wrote.

**Advance (announce-and-continue):** state "Charters rewritten — moving to the goals-document rewrite." and continue in the same turn. (The goals rewrite is arbitrated line-by-line and applied under its own hard gate in Steps 9–10.)

---

## Phase D — Goals rewrite

This is the reason the monthlies captured gaps without acting. **Gate before the write.** The goals document is the single most load-bearing document this skill touches.

### Step 8: Propose the goals-document rewrite

1. Present the current goals document in full.
2. Present the aggregated goal gaps accepted in Phase B, standing gaps flagged first.
3. For each current goal, propose a disposition informed by the gaps:
   - **Keep as is** — the quarter's behaviour matched it.
   - **Rewrite** — the goal is stale or mis-stated (a standing "stated but contradicted" gap points here); propose the new wording.
   - **Add** — a standing "acted but unstated" gap means a real goal was operating without being written; propose stating it.
   - **Drop** — a goal that no longer reflects where the user is going; propose removing it, with the gap evidence.

Present the full proposed document as a before/after.

### Step 9: Line-by-line arbitration

Walk each proposed change one at a time. For each: present {current line, proposed change, gap evidence with monthly sources, your read}. Ask:

> "Accept / edit the wording / keep the original / different disposition (keep/rewrite/add/drop)?"

Numbered list. **The user arbitrates every line.** Nothing auto-commits.

The "explain the why in 30 seconds" test applies: a goal the user can't say out loud why it belongs gets softened to a question or dropped, not written as settled.

### Step 10: Write the goals document

Assemble the final document from the arbitrated dispositions (the user's edited wording, never the drafted one). Present the complete final file as a diff against the current one.

Gate: **"Here's the rewritten goals document. Apply / edit / abort?"**

On apply: write the file. Record the before/after and the reasoning under `## Goals rewrite record` in the quarterly note.

**Advance (announce-and-continue):** state "Goals document rewritten — moving to the standing-commitment checkpoint." and continue in the same turn.

---

## Phase E — Standing-commitment checkpoint (optional)

Skip this phase entirely if you don't have a long-running side commitment that competes with day-to-day work for attention. If you do — a venture, a significant personal project, a major lifestyle change — it's worth checking at more than one altitude so day-to-day urgency doesn't crowd it out permanently: weekly (an ordinary checkpoint inside your regular weekly review), quarterly (here — trajectory against lived allocation), and annually (against your longer-horizon vision, if you run that review). This phase is a **strategic disposition, not a project-portfolio session** — no next-actions, no reshape of the commitment's task list.

### Step 11: Trajectory against the quarter's lived allocation

1. Read the commitment's project note and its decision log, if you keep one.
2. Surface the re-evaluation triggers named in that decision (the conditions under which a hold/pause call gets revisited).
3. Check each trigger against the quarter's actual allocation — how much attention the commitment actually got, and whether any trigger fired.

Present: {each re-evaluation trigger, did it fire this quarter, evidence}. Then ask:

> "Given the quarter's lived allocation — does this stay on hold, re-activate, or get adjusted? This is a disposition to capture, not a plan to build."

Capture the disposition (and the reasoning) under a checkpoint section in the quarterly note. If the disposition is "re-activate" or "adjust," note it as an input for the next weekly/monthly review to action — **do not** restructure the project here.

**Advance (announce-and-continue):** state "Checkpoint captured — moving to the vision touch." and continue in the same turn.

---

## Phase F — Vision touch

**Read-only. Vision is never rewritten here** — that's a future annual review's job, if you run one. This phase captures whether the quarter's lived life still matches vision, as an input that review will act on.

### Step 12: Does the lived quarter still match vision?

1. Present your vision document.
2. Present the quarter's accepted themes (Phase B) and the rewritten goals (Phase D) beside it.
3. Ask a single reflective question:

> "Reading the quarter you actually lived against your vision — where do they still line up, and where has a gap opened? Name the tension; a future annual-level review is where vision itself gets edited, not here."

Capture the tension as a concise line (or "none — vision still holds") in the quarterly note's `vision_tension` field and under `## Vision tension`. This is the line a future annual review, if you run one, collects across four quarterlies.

**Advance (announce-and-continue):** state "Vision tension captured — moving to the audit-commitments review." and continue in the same turn. (Skip straight to Phase H if you don't use the operating-audit skill.)

---

## Phase G — Audit commitments

Skip this phase entirely if you don't run the operating-audit skill.

### Step 13: Review the operating-audit accepted changes

1. Read the quarter's operating-audit report (assembled in Phase A).
2. Surface the changes the audit accepted or committed to.
3. For each, check whether it was actually adopted this quarter (evidence from your daily/monthly record).

Walk one at a time. For each: {committed change, adopted? evidence}. Ask:

> "Adopted / carry forward to next quarter / drop explicitly (it's no longer worth doing)?"

Numbered list. Capture dispositions under `## Audit commitments` in the quarterly note. Carried-forward items are named so next quarter's audit pre-flight can check them.

**Advance (announce-and-continue):** state "Audit commitments reviewed — moving to the outputs." and continue in the same turn.

---

## Phase H — Outputs

Two written outputs. The rewrites (charters, goals document) were the *mutations* of Phases C and D and are already done.

### Step 14: "How the quarter went"

Read the accepted themes (Phase B), the goals rewrite (Phase D), the standing-commitment disposition (Phase E, if run), and the vision tension (Phase F). Draft a first-person "how the quarter went" — usually a single paragraph, but a fragmentary shape is fine if that's the honest form. Strategic and curious in tone, not evaluative. State what the quarter was about, what moved at the altitude this skill owns, and what the rewrites were answering. **Do not draft a verdict on whether the quarter was "good" or "bad."**

Present the draft.

> "Here's how {QUARTER} went — edit or approve?"

On approval: write under `## How the quarter went` in the quarterly note.

### Step 15: Forward theme

Name the forward theme for next quarter, if one is genuinely there. Not a goal (that's the goals document), not a weekly priority list, not a plan. A *strategic theme* that names what next quarter is about at the level this skill owns — the thing your weekly review's forward-looking step should orient toward. Usually one or two sentences. Sometimes the honest answer is that no single theme has formed yet and next quarter carries two or three live threads — say that plainly rather than forcing one.

Shapes that tend to work — a strategic bet to test ("This quarter proves whether the side project earns real weekend hours or stays a someday."), a rebalance to hold ("Bring health up from the floor it hit last quarter without letting the core work cadence slip."), or a constraint to honour at quarter scale ("No new area commitments until the charter rebalance settles."). What it isn't: a list of priorities (that's the weekly review's job), a goal restatement (the goals document), or a month-scale signal (monthly's forward signal).

Present the draft.

> "Here's the forward theme for {next quarter} — edit or approve?"

On approval: write under `## Forward theme` in the quarterly note. This is the line your weekly review's forward-signal step reads as standing intent — it does not need to be copied into a daily note if your weekly review can pull it live from the latest quarterly note.

**Advance (announce-and-continue):** state "Outputs written — moving to close." and continue in the same turn.

---

## Phase I — Close

### Step 16: Finalize and breadcrumb

1. The review note already lives in its permanent home. Confirm it's complete and its frontmatter carries the future-review marker (if applicable) and the filled `vision_tension`.
2. Write a one-line breadcrumb to today's daily note: `- HH:MM — Quarterly review for {QUARTER} complete. Goals document + {N} charters rewritten. Forward theme: {one-line gist} #quarterly-review`
3. If you keep a distilled decisions/episodic log, write a one-line entry there: the quarter's shape, what the goals rewrite answered, and the standing-commitment disposition if run. Write the quarter's shape, not a task log.
4. If you track recurring obligations in a checklist, mark this quarter's review complete there.

State: **"Quarterly review for {QUARTER} closed. The forward theme is now standing intent for the weekly review and any pre-review brief you run."**

---

## Rules

- **Interactive on decisions, always.** Every theme/gap/area-rollup adjudication, each charter rewrite, every goals-document line, the goals-document apply, the standing-commitment disposition, the vision tension, the audit-commitment dispositions, the "how the quarter went" paragraph, and the forward theme requires explicit input before it is committed. Pure between-phase transitions ("ready for the next phase?") are announce-and-continue, not gates. The model-switch confirmation stays a hard gate.
- **Hard gate mechanic (decision steps).** End every *decision* step with its gate question and stop completely. Do **not** gate a bare phase-transition — announce it in one line and continue; the user interrupts if they want to pause.
- **One step at a time.** Numbered lists for all decision points.
- **Do not rush.** Take the time each phase needs. There is no time cap.
- **Reflective at the top, decisive at the altitude it owns.** Themes and the vision touch are curious; the charter and goals rewrites are decisions the user arbitrates line by line.
- **Source before synthesis.** Every quarter-level claim traces to which monthly (or monthlies) carried it, with the monthly's quoted text. No synthesized claim without a monthly source.
- **User-as-final-arbiter.** Nothing AI-generated auto-commits. Themes, charter rewrites, the goals-document rewrite, the standing-commitment disposition, the vision tension, the "how the quarter went" paragraph, and the forward theme all need explicit user approval — and the version that gets written is the user's edited version, not the drafted one.
- **Hard separation from monthly.** No daily-note re-reads, no theme extraction from raw days, no journal migration, no project portfolio, no public-status propagation. Quarterly reads the three finished monthlies.
- **Vision is read-only.** Capture the tension line; never edit the vision document. Purpose and principles are out of scope entirely.
- **The goals rewrite is the point.** The three monthlies captured gaps precisely so this rewrite has data. Do not skip it or defer it "to next time" — if a gap is genuinely not ready to answer, that disposition is captured as a known unknown, not a silent skip.
- **Conditional phases fire only when configured.** Skip Phase E if you have no standing commitment to check. Skip Phase G (and the Step 1 audit precondition) entirely if you don't run the operating-audit skill.
- **File writes:** read the full file first, make changes, write back. Always verify.
- **Timestamp protocol:** before writing any `HH:MM`, get the current wall-clock time. Never estimate.
- **Review window:** the calendar quarter *just ended*. Running on the first day of the new quarter, the three monthlies are the ones reviewing that quarter's three months.

### Correction checkpoint

When the user corrects routing, content, framing, AI surfacing, or process:

1. Stop the current flow completely.
2. Name the error category explicitly (source missing, synthesized-without-monthly-evidence, scope creep into monthly territory, scope creep into annual territory — vision or purpose edited, charter rewritten without gate, a goals-document line committed without arbitration, evaluative framing, etc.).
3. State the corrected disposition or reading.
4. Ask for confirmation on the fix.
5. **Do not proceed to the next step until the correction is acknowledged and executed.**

If a correction reveals a recurring AI failure mode, capture it as a note for yourself at session close.
