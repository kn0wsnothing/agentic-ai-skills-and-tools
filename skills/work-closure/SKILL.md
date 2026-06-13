---
name: work-closure
description: >
  Interactive workday shutdown. Batch-collects open items from today's focus, daily notes, vault inbox, and a quick open-loops sweep of what the day left live. Clarifies the batch and routes each item to its correct destination. Marks the workday closed. Use when you're done with work for the day — whether that's 5pm, 7pm, or whenever you step away. One step at a time — you decide everything.
---

# Work Closure

Interactive end-of-workday wrap-up. **Context-driven** — search your workspace before proposing destinations. Numbered lists for routing decisions. **Gates at routing-batch confirmation, inbox verification, and the work-closure summary are enforced by explicit user confirmation calls — not prose. No disposition, write, or next-phase action may share a turn with a gate call. Drop all ceremonial inter-step gates.**

This is the *operational* shutdown — GTD processing only. Reflection and forward-signaling happen in day-closure.

---

## Where this fits

- **Upstream:** the day's work (focus items, daily notes, meeting notes, inbox captures accumulated since morning)
- **Downstream:** day-closure reads the episodic log and names any residue this skill didn't clear
- **Siblings:** none — this is the sole operational shutdown

---

## Non-negotiable principles

1. **The inbox empties or the run failed.** Nothing stays in `{inbox}/Inbox.md` or the `{inbox}/` folder after Phase B. If any item remains unrouted, the skill has not finished — return to Phase B. There is no "sit on this for now" disposition. If an item genuinely needs more thought overnight, the destination is a project's notes section, an area hub, or `{archive}/Inbox/` with a parking note — never the inbox.

2. **GTD naming, no exceptions.**
   - **Projects are named as outcomes** — something with a clear "done" criterion. Bad: "data analytics hire". Good: "Bring on a junior analytics hire to support me". If you can't phrase it as an outcome, the project isn't defined yet — ask before creating the folder.
   - **Tasks are verb-led.** Every actionable item starts with the verb: *Draft, Review, Search, Decide, Call, Confirm, Schedule.* Topic labels ("QR findings hunt", "Hormuz tracker Q") are not next actions — they're unclarified work. If the verb isn't nameable, ask what action moves it forward.

3. **Quote-or-don't-name (anti-fabrication).** Every named entity in any extraction or proposal — person, project, decision, deadline, meeting outcome — must be paired with the verbatim source line it came from. If you can't quote the source, you don't have it. Surface the gap, ask the user. Never round to the nearest plausible-sounding name.

4. **Batch collect, then clarify — enforced by gate call.** No routing decisions during Phase A. Phase A produces a numbered list with no destination assumptions. The Phase A→B boundary is enforced by an explicit gate call; you may not emit a disposition in the same turn as the collection list. Phase B is where dispositions form, and ends with another gate call before any write executes.

5. **Keyword overlap is not project fit.** A task does not belong in a project just because they share a noun. Match against the project's outcome — if the task doesn't move that outcome forward, propose a different home or use the **Unclear** disposition. Nearest-keyword-match routing is a failure mode.

---

## Before starting

Read today's daily note: `{daily_notes}/YYYY-MM-DD.md`

### Pre-flight: unprocessed meeting notes

Scan `{meetings}/` for any meeting notes that haven't had action items extracted yet. If unprocessed notes exist for today's window, run your meeting-notes extraction skill (if installed) before proceeding. After extraction, continue from Phase A.

If none found: continue.

---

## Phase A — Collection (batch, parallelized, no routing)

Scan all sources in parallel where possible. Produce a single numbered list of items to clarify in Phase B. **No routing yet. No project guesses. No paraphrased extractions.**

### Step 1: Focus sync + open items

Read today's `## Focus` section.

**Auto-sync (no confirmation needed):**

For each completed focus item linked to a project task (e.g., via an ID or wikilink), find the matching task in its project note and mark it done. Report what was synced.

**Needs decision (numbered list):** Present remaining focus items — unchecked or completed-but-unlinked. "These are open or unlinked from today's Focus — which carry forward, which are done, which drop?"

### Step 2: Today's notes

Read today's `## Notes` section. List unprocessed items in the same numbered list. (No gate.)

### Step 3: Inbox + meeting notes

**In-session parallel reads:** Read `{inbox}/Inbox.md`. List all files in `{inbox}/` folder and read them in parallel.

**Meeting notes extraction — partition by status:**

List every `.md` file under `{meetings}/` that is **not** under `{meetings}/Processed/`. For each, check whether action items have already been extracted (e.g., via a frontmatter marker or prior processing run):

- **Already extracted:** read the action items section directly. Drop them into the assembled numbered list, preserving any inline source quotes.
- **Not yet extracted:** delegate to a subagent (if available) or read and extract in-session. For each file, extract verb-led action items with verbatim source quotes. Flag any item you can't quote as "needs human review".

Append meeting items to the assembled numbered list, keeping source quotes visible.

### Step 3b: Open loops — what today left live

Source scans surface what got written down. This step surfaces what's still in your head from the day and never made it to a note. **Capture, not reflection:** surface open *loops* (things needing a next action or a decision), not open *threads* (things needing meaning — that's day-closure's lane).

**Priming.** Use what's already in hand: today's focus items (what got done, what slipped), today's notes, any meeting thread cues, and the morning's context signal. From these, privately note 1–2 candidate loose ends — a focus item that slipped without a decision, a meeting that ended on an unresolved ask, a reply put off. Hold as questioning fuel; do not recite.

Then ask **one or two grounded questions**, not a flat "anything else?":
- *"You didn't get to [slipped focus item] today — does that carry, drop, or is it waiting on someone?"*
- *"[Meeting] ended on [open ask] — is there a next move owed, or is it off your plate now?"*
- *"Anything from today still open that the scan didn't catch — a reply you decided not to send, a thread you left hanging?"*

Capture each named loop to the assembled numbered list, grouped as "Open loops". Collection only — no routing yet.

If the source scans **and** this sweep surface nothing: state "Nothing open" and skip to Phase C. A clean inbox alone is not an empty head — run the sweep before taking the fast path.

**Gate.** Emit the full assembled numbered list — every item with its source quote, grouped by section (Focus open items / today's Notes / inbox bullets + files / meeting items / open loops). **No proposed dispositions yet.** Then ask the user: "List assembled — prune, add, or ready to clarify?" and wait for response before proceeding. Phase B begins only after confirmation.

---

## Phase B — Clarify (one batch)

Frame to user: "We're clarifying now. Here's the active project + area landscape, then a disposition for each item. Confirm or correct the whole batch."

### Active-projects landscape

Scan your project notes to build a dedupe surface before routing. For each active project, read the project's stated outcome. For each area hub, note its domain.

Cache this for all of Phase B. Before proposing a disposition for any item, check whether the project already has a near-match task open. If it does, the disposition is "already tracked, skip" — not a new route.

```bash
# Example bash scan (adapt paths to your setup):
for f in {projects}/Active/*/*.md; do
  outcome=$(awk '/^outcome:/{sub(/^outcome:[ ]*/,""); print; exit}' "$f")
  echo "$f :: ${outcome:-<no outcome>}"
done
```

### Propose dispositions (single message, whole batch)

For each item from Phase A, propose one disposition. Each proposal must include:
- The item, **verb-led if actionable**
- The source quote (already attached from Phase A)
- The proposed disposition, with reasoning tied to the project's outcome

#### Disposition options

- **Actionable → route to [project]** — cite the project's outcome and explain how the task moves it forward. Include energy and collaboration tags if your system uses them, and a date if timing is implied.
- **Carry to tomorrow** — pull into tomorrow's focus.
- **Defer to [date]** — out of mind until then.
- **Reference → file to [location]** — durable info, not a task.
- **Done** — already finished.
- **Trash** — delete.
- **Unclear — propose new project / ask user.** Use when no existing project's outcome obviously contains the task. State what's unclear and offer options: (a) new project with a proposed outcome-stated name, (b) route to area hub as ongoing, (c) trash. Do not round to the nearest-keyword-match project.

#### Date prompt

For every actionable item, propose one of:
- **Due date** — external deadline (call, meeting prep, hard commitment)
- **Do date** — self-scheduled work date
- **Defer date** — out of mind until this date
- **No date** — sits on the next-actions queue, surfaces in weekly review

State a recommendation when context implies one. Always offer the choice; don't silently leave dates off when timing is implied.

**Gate — ask for confirmation.** After emitting all proposals in a single message, ask: "Confirm or correct the batch?" and wait. Do NOT execute any write in the same turn. If the user corrects, apply corrections and re-ask on the updated batch.

### Execute the batch

After confirmation:

1. **Append actionable items to project notes.** Format to your system's conventions (e.g., `- [ ] Task description [status:: next]`).

2. **Create new projects** when the user confirmed the Unclear → new project disposition. Use the outcome-stated name. Scaffold folder + main note from your project template. Add the first action.

3. **Move processed meeting notes** to `{meetings}/Processed/YYYY-MM/` (year-month subfolder, create if missing).

   **Move protocol — copy → verify → delete (non-negotiable):**
   1. Copy the source to the destination.
   2. Verify the copy is intact (byte size matches, diff returns clean).
   3. Only after verification: remove the original.

4. **Remove every processed bullet** from `{inbox}/Inbox.md`.

5. **Move every processed file** out of `{inbox}/` to its destination.

### Inbox verification (mandatory)

State to user: **"Inbox verification — `{inbox}/Inbox.md`: [blank / X items removed], `{inbox}/` folder: [empty / X files moved]. Confirm?"**

If anything remains in either location, the skill has not finished. Return to disposition for those items.

**Gate — ask for confirmation.** "Inbox cleared. Ready to close the workday?" and wait. Do NOT begin Phase C in the same turn.

---

## Phase C — Workday close

### Step 4a: Triage gate (overwhelm tripwire)

Scan today's notes (already read in Step 2) for overload language — "overwhelmed", "too much", "no time to triage", "drowning", "can't keep up", or a `#triage` tag.

**No hit:** skip silently to Step 4b.

**Hit:** the closure does not complete until one named item gets one of three dispositions. Quote the line that fired, then put the decision:

> *"You named overwhelm today: '[quote]'. Before we close, one item gets picked — drop it, defer it past this month, or renegotiate it with the person who put it there. Which item, and which move?"*

- **Drop** — remove/mark the item in its source file now.
- **Defer** — re-date it past the month boundary; state the date explicitly so it's a decision, not a slip.
- **Renegotiate** — create a next action (route to your messaging workflow) with the recipient named. The renegotiation must exist as a draftable artifact before close.

Record the outcome in the work closure line as `triage-resolved: [item] → [move]`. If the user explicitly declines ("today was fine, the line was noise"), record `triage-resolved: waved off` — a legitimate outcome, but it must be explicit.

### Step 4b: Write work closure summary

Draft a brief summary of what was accomplished and what's in flight:

```
Completed: [brief list]
In flight: [brief list]
Blocked/waiting: [brief list]
Absorbed: [what landed today that wasn't on the morning focus — and what it displaced, if anything]
```

The Absorbed line is telemetry, not judgment. A high-volume build day is not slippage — it tracks unplanned intake: work that arrived mid-day and got carried. "Displaced: nothing (scope growth)" is a valid and useful entry. Omit the line entirely on days with no absorbed work.

**Show the draft and ask for confirmation.** "Here's the work closure summary — edit or approve?" and wait. Do NOT write to the daily note in the same turn.

On approval: write to today's `## Work closure` section (create if needed):

```markdown
## Work closure

- HH:MM — Workday closed. [summary] #work-closure
```

**Workday closed.**

### Step 4c: Offer the day close (once, one line)

After the workday close, offer exactly once: *"Close the day too? Two minutes."* If yes, run `day-closure`. If no: done — never re-ask.

---

## Rules

- **Interactive on routing decisions and the open-loops sweep; silent on source collection.** Phase A scans are silent; Step 3b is the one interactive collection step. Phase B is where routing lives.
- **Open-loops sweep is capture, not reflection.** Step 3b surfaces operational loose ends grounded in the day's evidence; it never drifts into meaning-making — that's day-closure's lane.
- **Gates are explicit user confirmation calls, not prose** — routing-batch confirmation, inbox verification, work-closure summary approval. No disposition / write / next-phase action may share a turn with a gate call.
- **Numbered lists for routing decisions.** Always.
- **Context first.** Search before proposing. Never default-route on keyword overlap.
- **Verify with quotes.** No named person, project, decision, or deadline appears in any proposal without a quoted source line.
- **Inbox empties.** No exceptions.
- **GTD naming.** Outcome-stated projects, verb-led tasks. No topic labels.
- **Never invent.** Only create what the user explicitly confirms.
- **File writes:** Read full file first, make changes, write back. Always verify.
- **Timestamp protocol:** before writing any HH:MM, check the current time in your local timezone. Never estimate.
- **Waiting-for stamps:** any new waiting-for entry gets a `since:` date so age can be computed in future reviews.

### Correction checkpoint

When the user corrects routing, content, framing, or process:
1. Stop current flow completely
2. Name the error category (location, content, sequence, framing, fabrication, naming)
3. State the corrected disposition or reading
4. Ask for confirmation on the fix
5. Do not proceed to the next step until correction is acknowledged and executed
