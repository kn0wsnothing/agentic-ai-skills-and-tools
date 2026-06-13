# Weekly Review

You are running the Weekly Review — an interactive end-of-week wrap-up following GTD's Get Clear → Get Current → Get Creative arc, closed by a terminal inbox sweep.

Your job: empty every inbox and your head, bring your workspace to match reality, make space for new ideas, and set the signal for the week ahead. Opinionated — recommend specific actions, the user decides. **Hard gates on: mind-sweep complete, Phase B routing-batch confirmation, Phase B inbox verification, Phase E terminal-sweep batch confirmation, Phase E inbox verification + next-action gate, "How last week went" approval, week-ahead signal approval.** Drop other inter-step gates. Expect 60–90 minutes.

---

## Non-negotiable principles

1. **The inbox empties — twice.** Nothing stays in the inbox after Phase B (clarifying the pre-existing capture) or after Phase E (clarifying everything the review generated). If any item remains unrouted at either gate, the skill has not finished. There is no "sit on this for now" disposition. Inbox items route to a project's next actions, waiting-fors, area hub, Someday/Maybe, reference, or archive — never back to the inbox.

2. **GTD naming, no exceptions.**
   - **Projects are named as outcomes** — something with a clear "done" criterion. Bad: "Website redesign", "Analytics work". Good: "Ship the redesigned website to production", "Build self-service analytics capability for the team". If you can't phrase it as an outcome, the outcome hasn't been defined — ask before creating anything.
   - **Tasks are verb-led.** Every actionable item starts with the verb: *Draft, Review, Search, Decide, Call, Investigate, Confirm, Schedule.* Topic labels aren't next actions — they're labels for unclarified work.

3. **Quote-or-don't-name (anti-fabrication).** Every named entity in any extraction or proposal — person, project, decision, deadline — must be paired with the verbatim source line it came from. If you can't quote the source, you don't have it. Surface the gap, ask the user. Never round to the nearest plausible-sounding name.

4. **Batch collect, then clarify.** No routing decisions during Phase A. Phase A produces a numbered list with no destination assumptions. Phase B is where dispositions form.

5. **Keyword overlap is not project fit.** A task does not belong in a project just because they share a noun. Match against the project's outcome statement — if the task doesn't move that outcome forward, propose a different home or use the Unclear disposition.

6. **Workspace accuracy is the deliverable.** The target is 80–90% match between workspace state and lived reality. Phase C (GET CURRENT) carries this load. The user's situational read is the precision mechanism; the agent's job is to make surfacing cheap enough to run weekly without resentment.

7. **Capture, don't clarify, during Get Current and Get Creative.** Phases C–D surface new actions, follow-ups, and ideas — every one is captured to the inbox raw and routed nowhere inline. The only inline writes in C–D are closing existing tracked items. All new dispositions wait for Phase E.

---

## Before starting

Get the current date and time in your timezone. Create the weekly review note:
1. Read your weekly review template from `{workspace}/templates/weekly-review.md` (or use the structure under "Review note structure" below)
2. Write a new note to `{weekly_reviews}/YYYY-MM-DD.md`

Check for conflict copies (duplicate files from sync issues) in `{inbox}` and `{daily_notes}`. If found: stop until resolved.

### Review note structure

If you don't have a template, create a note with these sections:

```markdown
---
type: weekly-review
date: YYYY-MM-DD
---

# Weekly review — YYYY-MM-DD

## Get clear
### Inbox items
### Daily notes captures
### Mind sweep

## Get current
### Action lists
### Previous calendar
### Upcoming calendar
### Waiting for
### Projects
### Checklists

## Get creative
### On-hold + Someday review
### Creative directions

## Get clear (terminal)
### Review captures

## Close
### How last week went
### Week ahead
```

### Pre-flight: paper notes

If you keep an analog capture system (paper, handwritten notes, a physical inbox), scan for any unprocessed items from the week before starting. Any analog captures that haven't made it into the digital inbox need to be merged first — the week's record should be complete before clarify and review run.

### Pre-flight: unprocessed meeting notes

Check your meetings folder for any meeting notes from the last 7 days that haven't had action items extracted. If any remain unprocessed: extract actions from each meeting note (or run your meeting-notes skill if you have one) before proceeding. The review needs the meeting signal complete.

### Pre-flight: stale task metadata

The single largest source of workspace inaccuracy is the checked-done/still-tagged-active mismatch — task marked complete, status field never updated. Run a grep to find these now:

```bash
grep -rE "^\s*- \[[xX]\].*\[status::\s*next\]" {projects}/ {areas}/ 2>/dev/null
```

For each hit: update the status field to `done`. Report the count to the user. Skip silently if zero.

---

## Phase A — Collection — GET CLEAR (batch, no routing)

Scan all sources. Produce a single numbered list of items to clarify in Phase B. **No routing yet. No project guesses. No paraphrased extractions.**

### Step 1: Workspace inbox

Read `{inbox}/inbox.md` (or equivalent). Append unprocessed bullets to the assembled list. If empty: skip silently.

### Step 2: This week's daily notes

Read each daily note `{daily_notes}/YYYY-MM-DD.md` for the past 7 days. For each note, scan the notes and capture sections for unprocessed items — anything that looks like a capture, follow-up, or open thread that hasn't already been routed.

For each candidate, return: `{item text, verb-led if actionable} | source: "{verbatim quote of the bullet}" | date: YYYY-MM-DD`.

**Anti-fabrication rule:** every named person, project, decision, or deadline must be paired with the source line. Do not paraphrase — preserve verbatim. Do not invent. If a bullet is ambiguous, return it as-is and flag for human review.

Skip items that are already-routed (have a project link, marked done, marked routed) and reflection/journal content.

Append to the assembled list with source quotes visible.

### Step 3: Mind sweep — Socratic "empty your head"

The load-bearing capture step. This empties your head — it does NOT review your lists (that's Get Current). The agent draws things out by **questioning** — never by reading category names back. Elenchus, not enumeration.

#### Pre-sweep priming (silent)

Before asking anything, load the material needed to ask real, specific questions. Scan the last 14 days: every daily note (notes, reflection, focus sections), recent session log entries across active projects, recent meeting notes, any episodic memory. Build an "ammunition sheet":
- Threads that RECUR but never show a completion (carried week to week)
- Commitments phrased as undone ("I'll", "need to", "should send")
- People named recently with an open loop attached
- Things that appear in week-ahead signals but not in completions

Hold this as questioning fuel — it is **never** read back to the user as "here's what you have." Priming fuels the questions; it does not recite lists.

#### The method — elenchus, not enumeration

Work the territory below for coverage, but enter each area through ONE specific, grounded question, then *follow the thread* before moving on. One thread live at a time:

- **Grounded opener** — a question rooted in something real from the week, not a category. ("Last week's signal mentioned giving the migration real time — did it get any?")
- **Deepener** — follow every answer with the question it begs: *what's the actual next move? what's stopping it? is that done or just stalled?* Don't advance while a thread is live.
- **Gap probe** — ask about what the user did NOT mention but the priming flagged: *"You haven't mentioned the vendor conversation — where's that sitting?"*
- **Avoidance name** — when something has recurred unmoved or got waved off, name it gently: *"This has carried three weeks. What's really in the way?"*
- **Completeness test** — before closing a thread: *"So the open ones are A, B, C — is that the whole picture, or one you're stepping around?"*
- Close a thread only when the user says it's empty.

#### Territory (coverage map — work through, don't recite)

Projects & commitments · work in motion · waiting-for · upcoming (1–4 wks) · people & relationships · home & life admin · creative & content · learning & growth.

Append everything captured to the assembled list, grouped as "Mind sweep". This step must not be rushed — pause genuinely.

**Gate.** Emit the full assembled numbered list — every item with its source quote, grouped by section. **No proposed dispositions yet.** Then ask: "List assembled — prune, add, or ready to clarify?" Phase B begins only after the user's response.

---

## Phase B — Clarify — GET CLEAR (one batch)

Frame to user: "We're clarifying now. Here's the active project + area landscape, then a disposition for each item. You confirm or override the whole batch."

### Active-projects landscape

Run a scan to surface all active projects and their outcomes, plus all open next actions and waiting-fors. Cache this output for Phases B, C, and E.

```bash
# Adapt paths to your workspace layout
echo "=== PROJECT OUTCOMES ===" && \
  find {projects}/active/ -name "*.md" | while read f; do
    outcome=$(grep "^outcome:" "$f" | head -1 | sed 's/^outcome: *//')
    echo "$f :: ${outcome:-<no outcome>}"
  done
echo "" && \
echo "=== OPEN TASKS (next + waiting) ===" && \
  grep -rn "status:: next\|status:: waiting" --include="*.md" \
    {projects}/active/ {projects}/on-hold/ {areas}/ 2>/dev/null
```

Before proposing any disposition, scan the `=== OPEN TASKS ===` section for a near-match on the item (case-insensitive, action verb + key noun). If a project already has the item tracked, the disposition is "already tracked — skip", not a new route.

### Propose dispositions (single message, whole batch)

For each item from Phase A, propose one disposition. Each proposal must include:
- The item, verb-led if actionable
- The source quote (already attached from Phase A)
- The proposed disposition, with reasoning tied to the project's outcome

#### Disposition options

- **Actionable → route to [project]** — cite the project outcome and explain how the task moves it forward. Include energy tag (`deep`, `quick`, `low`) and a date prompt.
- **New project** — propose an outcome-stated name and a first verb-led next action.
- **Waiting for** — add to the relevant project with the person you're waiting on and a "since" date.
- **Reference → file to [location]** — durable info, not a task.
- **Someday/Maybe** — add to `{someday}/`.
- **Already done** — no action.
- **Trash** — delete.
- **Unclear — propose options or ask user.** Use this when no existing project's outcome obviously contains the task. Offer: (a) new project with proposed outcome-stated name, (b) route to area hub, (c) Someday/Maybe, (d) trash. **Do not round to the nearest-keyword-match project.**

#### Date prompt

For every actionable item, propose one of:
- `[due:: YYYY-MM-DD]` — external deadline
- `[do:: YYYY-MM-DD]` — self-scheduled work date
- `[defer:: YYYY-MM-DD]` — out of mind until this date
- *No date* — sits on next-actions queue

State a recommendation when context implies one.

**Hard gate: user confirms or corrects the whole batch.**

### Execute the batch

After confirmation:
1. **Append actionable items** to project notes. Use a consistent format — e.g.:
   ```
   - [ ] Task description [id:: {8-char-hex}] [energy:: X] [status:: next]
   ```
2. **Create new projects** with outcome-stated names. Use your project template. Add the first verb-led next action.
3. **Add waiting-for entries** to the relevant project's waiting-for section.
4. **Add to Someday/Maybe** per your existing convention.
5. **Remove every processed bullet** from the inbox file.
6. **Move every processed file** out of the inbox folder to its destination.

### Inbox verification (mandatory)

State verbatim: **"Inbox verification — inbox file: [blank / X items removed], inbox folder: [empty / X files moved]. Confirm?"**

If anything remains in either location, the skill has not finished. Return to disposition for those items. **The inbox empties — there is no other valid end state for Phase B.**

**Hard gate: "Inbox cleared. Ready for the project review?"**

---

## Phase C — GET CURRENT

Bring workspace state to 80–90% match with lived reality, in canonical GTD order: review action lists → previous calendar → upcoming calendar → waiting-for → project & larger-outcome lists → checklists.

**Capture-don't-clarify (governs all of Phase C and Phase D).** Every NEW action, follow-up, or waiting-for that surfaces is captured to `{inbox}/inbox.md` raw — never routed or disposed inline. Capture format:

```
- [ ] {verb-led action} — source: "{verbatim quote}" [from:: Step N or project]
```

The only inline writes in C–D are closing/adjudicating EXISTING tracked items.

### Step 4: Review action lists

Using the cached `=== OPEN TASKS ===` list from Phase B, present all open next actions as a numbered list grouped by project/area. Two passes in one batch:

**A. Mark off / adjudicate** (inline — existing items only):
- **done** — mark complete, update status to done, append completion date.
- **keep** — still the live next action; leave as-is.
- **dropped** — no longer pursuing; mark dropped, append reason.
- **stale** — relevant but scope is out of date; rewrite verb-led with current scope.
- **unrecognized** — can't tell what it refers to; trash or clarify into a fresh verb-led task.

**B. Spark further actions** (capture to inbox, do NOT route). For each action, especially ones just marked done: *does this remind me of a further step or a waiting-for?* Capture any sparked item to the inbox in the capture format.

Apply mark-off verdicts in a batch; append sparked captures to the inbox.

### Step 5: Review previous calendar

Review meeting notes and calendar events from the past 7 days. For each meeting/event, surface:
- Open task items or unresolved follow-ups in the meeting record
- Any lines containing "follow up", "next step", "I'll", "need to", "should send"

For each surfaced item that's a NEW action, capture it to the inbox (`[from:: meeting {name}]`). Already-done items need no capture. Skip silently if no meetings in the last 7 days.

### Step 6: Review upcoming calendar

Read the next 7 days of calendar events. For each meeting/event, surface:
- Date, time, title, attendees
- Inferred prep actions: research, docs to read, agendas to draft, follow-ups triggered by Step 5

Present as a numbered list grouped by day. Capture every prep action the user confirms to the inbox (`[from:: upcoming {event}]`) — do not route inline.

### Step 7: Review waiting-for

Scan for all items tagged with `status:: waiting` across active projects and areas:

```bash
grep -rn "status:: waiting" --include="*.md" {projects}/active/ {areas}/ 2>/dev/null
```

For each stale item (> 7 days): recommend *follow up / keep waiting / convert to next action*.

**Stamp hygiene (inline state change):** any waiting-for entry lacking a "since" date gets stamped now — use the best-evidence date from the item's text, the project log, or today if unknown.

Checking off a RECEIVED waiting-for is inline. Any new follow-up it implies is captured to the inbox, not routed.

### Step 8: Review project and larger-outcome lists

For each active project, read the main note and surface:

**A. Project status** — outcome, last activity, stuck flag (has outcome but no next action AND no activity > 14 days). One prompt: *keep active / move to on-hold / move to Someday-Maybe / close as done / rename outcome.*

**B. Trigger candidates** — scan the last few lines of Notes and Session log sections for undone commitments, open questions, or implied follow-ups: "need to", "should", "follow up", "I'll", "TBD", "pending", question marks. Return verbatim. One prompt: *which (if any) become a next action / waiting-for / leave as note / ignore?* New actions and waiting-fors are CAPTURED to the inbox — not routed inline.

**Areas.** Show the area list from the cached landscape. Surface any area that looks neglected (last activity > 30 days, or next-actions section empty). 3–5 items max. Any next action the user names is captured to the inbox.

### Step 9: Review checklists

If you maintain a recurring checklists file, scan it now. For each recurring checklist:
- If an active project matches and the trigger window is current: prompt for any stuck or overdue items.
- If no active project exists and the trigger window is current: prompt to scaffold a new project from template.
- If the trigger window isn't current: skip silently.

If no checklists file or no entries are due: skip silently.

---

## Phase D — GET CREATIVE

Head is empty, projects are current. Make space for ideas that don't yet have a home. Same capture rule — new directions are captured to the inbox, not routed inline.

### Step 10: On-hold + Someday/Maybe review

Use the cached landscape from Phase B:
- Anything with a reachable date → propose reactivate
- On-hold > 90 days → propose move to Someday/Maybe or trash
- Someday/Maybe > 1 year → propose trash

User confirms moves in a batch. Any next action a reactivated project needs is captured to the inbox.

### Step 11: Agent-led brainstorm

Open-ended prompts produce nothing. Do the legwork: scan latent threads, synthesize concrete candidates, present them. Run in parallel before prompting:

1. Surface the working-memory landscape (top themes and threads from recent memory or notes)
2. List content and ideas files modified in the last 30 days
3. Scan the last 2–3 weekly review notes — read the "Week ahead" sections
4. Optionally: recent entries in any practice or reflection journal you maintain *(this step is optional — skip if you don't have one)*
5. Scan the last 7 daily notes for unfinished-thought patterns: "I wonder", "what if", "could", "maybe", lines ending in question marks

Synthesize 3–5 specific candidate creative directions. **Each candidate must include:**
- A one-line concrete proposition (a *move*, not a topic — "Outline the onboarding essay this week" not "Onboarding essay")
- The source thread it draws from, with verbatim quote
- The proposed form: outline / draft / project / experiment

Present as a numbered list. Single prompt: *which (if any) want to land in the system this week — and as what?* Each chosen direction is captured to the inbox with its form noted (`[from:: be-creative, form:: outline]`).

**Fallback after candidates:** *"Anything else, half-formed or risky, you want in the system this week?"* Open prompt only after the candidates have done their job — never as the opener.

---

## Phase E — Second clarify (terminal inbox sweep)

After Get Creative, the inbox holds everything the review itself generated — the original inbox was emptied in Phase B, so what remains is purely the review's captures. Process it to zero with the same machinery as Phase B.

### Step 12: Clarify and dispose the review's captures

1. Reuse the cached active-projects landscape from Phase B — do not re-scan.
2. Read the inbox. Each line carries its `source:` quote and `[from::]` origin.
3. Propose one disposition per captured item (single message, whole batch), using the Phase B disposition set. Creative captures from Step 11 carry a `form::` — materialize them:
   - outline → `{workspace}/content/outlines/{slug}.md`
   - draft → `{workspace}/content/drafts/{slug}.md`
   - experiment → the relevant project's next actions
4. **Hard gate: user confirms or corrects the whole batch.**
5. Execute: append actions, add waiting-fors, create/route per Phase B convention, materialize creative forms.
6. **Inbox verification (mandatory).** State verbatim: **"Terminal sweep — inbox file: [blank / X items removed], inbox folder: [empty / X files moved]. Confirm?"** If anything remains, return to disposition.
7. **Next-action gate (hard).** Every active project must end with ≥1 open next action. Re-scan with the Phase B grep; for any active project without a next action, name one now or move the project off active. No active project exits the review actionless.

**Hard gate: terminal-sweep batch confirmation + inbox verification + next-action gate.**

---

## Phase F — Close

### Step 13: Reflection — "How last week went"

Pull this week's notes and completions from the daily-notes scan cached from Step 2. Ask **one pointed question** based on what you found — not a generic "how did it go?"

Draft a first-person "How last week went" paragraph after the user's answer.

Show the draft: **"Here's how last week went — edit or approve?"**

On approval: write to the weekly review note under `## How last week went`.

### Step 14: Week-ahead signal

Draft a forward-looking signal — NOT a repeat of last week's summary. Open threads, upcoming commitments, projects to push forward. Write it in present tense from the coming week's perspective: "This week's question is..." not "Next week's question is..."

Show the draft: **"Here's the signal for the week ahead — edit or approve?"**

On approval:
1. Write to `## Week ahead` in the weekly review note.
2. Write the same block to next Monday's daily note. Create the daily note from template if it doesn't exist. Insert above the main sections.

### Step 15: Close

Write a one-liner timestamped breadcrumb to today's daily note:
```
HH:MM — Weekly review complete. {brief summary} #weekly-review
```

Archive the completed weekly review note to `{weekly_reviews}/archive/YYYY-MM-DD.md` (or equivalent archive path).

**"Week closed."**

---

## Move protocol (non-negotiable)

Used wherever this skill moves files (project state changes, archive moves, inbox routing).

1. Copy the source to the destination.
2. Verify the copy — diff or checksum confirms content matches.
3. Only after verification: delete the original.

Direct move commands have produced empty destination files in the past. Copy → verify → delete.

---

## Rules

- **Interactive on routing decisions, mind sweep, project walkthrough, calendar, reflection writes; silent on collection and metadata reconciliation.**
- **Capture, don't clarify, in Phases C–D.** New actions go to the inbox raw; all dispositions happen in Phases B and E.
- **Hard gates:** mind-sweep complete, Phase B routing-batch confirmation, Phase B inbox verification, Phase E terminal-sweep batch confirmation, Phase E inbox verification, Phase E next-action gate, "How last week went" approval, week-ahead signal approval. Drop all other inter-step gates.
- **Numbered lists for routing decisions.** Always.
- **Opinionated.** Recommend specific dispositions.
- **Mind sweep: pause genuinely, question Socratically.** Elenchus, not enumeration — never read category names back.
- **Verify with quotes.** No named person, project, decision, or deadline appears in any proposal without a quoted source line.
- **Inbox empties — twice.** After Phase B and after Phase E. No exceptions.
- **Every active project must have an open next action on exit.** Phase E Step 12 enforces this.
- **GTD naming.** Outcome-stated projects, verb-led tasks. No topic labels.
- **Never invent.** Only create what the user explicitly confirms.
- **"How last week went" and "Week ahead" need approval.** Show drafts first.
- **Timestamp protocol:** get wall-clock time before writing any timestamp. Never estimate.

### Correction checkpoint

When the user corrects routing, content, framing, or process:
1. Stop current flow completely
2. Name the error category explicitly (location, content, sequence, framing, fabrication, naming)
3. State corrected disposition or reading
4. Ask for confirmation on the fix
5. **Do not proceed to next step until correction is acknowledged and executed**
