# Weekly Review — Design Spec

This document explains the design decisions behind the Weekly Review skill.

---

## The Problem

Most "review my tasks" prompts produce a shallow list walkthrough. The agent skims open items, the user says "keep most of them", and nothing actually changes. The inbox doesn't empty. Projects without next actions stay that way. The week-ahead signal doesn't get written, so Monday starts cold.

A weekly review should do three things: empty your head completely, bring your workspace to match reality, and set the signal for next week. Those are meaningfully different operations, and conflating them is what makes most reviews feel like a waste of time.

---

## Why Three Phases (Not One)

GTD's three-move structure — Get Clear, Get Current, Get Creative — exists because each phase requires a different mental mode:

**Get Clear** is about capture, not judgment. You're emptying, not deciding. The moment you start routing items during collection, you stop capturing — your attention shifts to fit-assessment and you miss what's in your head. Batch collect first, then clarify.

**Get Current** is about confronting reality. This is where you mark things done, drop what's no longer relevant, and ensure every project has a live next action. It requires honesty, not creativity — you're bringing the system back to truth, not generating new ideas.

**Get Creative** only works when the other two are done. If your head isn't empty and your system isn't current, "be creative" is just anxiety — you're generating ideas on top of an unreviewed pile. Clear the ground first.

The phases aren't optional. Skipping Get Clear and jumping to Get Current produces a shallow project walkthrough. Skipping Get Current and jumping to Get Creative produces ideas that have no home in the system.

---

## The Two-Inbox Rule

The inbox empties twice. This is the single most important structural decision in the skill.

The first empty (end of Phase B) processes everything that existed before the review started.

The second empty (end of Phase E) processes everything the review itself generated. This is the move that closes the loop — Get Current and Get Creative reliably produce new captures (a triggered next action, a calendar-prep task, an idea from the brainstorm). Without a second clarify, those captures sit in the inbox and the review hasn't actually finished.

The rule is binary: either the inbox is empty or the review isn't done. There's no partial credit.

---

## Why the Mind Sweep Uses Elenchus

The Socratic mind sweep (Phase A, Step 3) is the hardest step to implement well. The naive version asks category prompts: "What about people? What about projects? What about home?" This produces shallow answers because it gives the user something to react to rather than something to generate from.

Elenchus works differently. The agent asks one grounded question rooted in something real from the week — something from the daily notes scan, a recurring thread, an open commitment from meeting notes — and follows that specific thread to completion before moving to the next. The thread structure forces genuine retrieval. The gap probe catches things the user didn't volunteer. The avoidance naming calls out the item that's been carried three weeks — gently, but directly.

The pre-sweep priming step (silent) is what makes specific questions possible. Without it, every question is generic. With it, the agent knows that a specific vendor call was mentioned twice last week and never followed up, that a commitment to a colleague appeared in a daily note on Tuesday, that the week-ahead signal mentioned a project that had zero work done on it. Those specific facts are what generates real questions.

The priming material is never read back to the user. It fuels questions, not reports.

---

## Capture-Don't-Clarify in Phases C–D

During Get Current and Get Creative, new actions surface constantly. A project's session log mentions a follow-up that was never tracked. A completed task reminds you of the next step. The brainstorm produces an idea that should become a project.

The temptation is to route these inline — it feels efficient to just "quickly add this to the right project" as you go. But inline routing breaks the review. The moment you switch from review-mode to clarify-mode, you've left the review. You start context-switching between "what's the state of this project" and "where does this new task go", and the project review loses its coherence.

The discipline is: capture raw to the inbox during Get Current and Get Creative. Route nothing inline. Phase E processes the whole batch after the review is complete. This keeps each phase's mode intact.

---

## Keyword Overlap Is Not Project Fit

One of the most common agent failure modes in routing: a task about "analytics" gets routed to the analytics project because the noun matches. But the task might be about a data request for a completely different project, or a personal learning goal, or a vendor evaluation — none of which move the analytics project's outcome forward.

The rule is to match against the project's outcome statement, not its name. "Does this task move the outcome 'Build self-service analytics capability for the team' forward?" If no, it doesn't belong there. Propose a different home or use the Unclear disposition.

This matters most in a weekly review because the review is when most new projects are born. Misrouting during project creation cascades — the project ends up with tasks that aren't scoped to its outcome, and the outcome becomes noise.

---

## The Correction Checkpoint

Reviews fail gracefully or they fail badly. A bad routing decision early in Phase B can cascade: the task goes to the wrong project, the wrong project gets marked as having a next action, the actual project ends up actionless. By the time you notice, you've confirmed a dozen more dispositions on top of the error.

The correction checkpoint stops this cascade. When a correction happens, the skill names the error category explicitly (not just "sorry, I'll fix it" — that doesn't help you understand what went wrong), states the corrected reading, and confirms before proceeding. No continuation until the fix is acknowledged. No "yes, and..." that moves forward while the correction is still live.

The explicit error-category naming is load-bearing. "That's a fabrication error — I attributed a deadline that wasn't in the source" is more useful than a silent fix because it flags the failure mode for this run and future ones.

---

## Why the Week-Ahead Signal Goes to Monday's Daily Note

The week-ahead signal is written to the weekly review note AND to next Monday's daily note. The weekly review note is the record. Monday's daily note is the delivery mechanism.

Without the Monday write, the signal exists in an archive. The next session that opens on Monday doesn't see it unless someone actively retrieves the weekly review — which rarely happens. Writing it to Monday's daily note means the signal is waiting when the week starts, without any retrieval step.

The voicing rule (present tense from the coming week's perspective) matters for the same reason. "This week's question is..." is immediately usable. "Next week's question is..." reads as deferred — it already sounds like something from last week.

---

## Why 60–90 Minutes

The review is designed to take real time. Not because the process is inefficient — because the work requires it.

The mind sweep takes as long as it takes. You're not reading a list; you're retrieving from memory. A Socratic sweep that's actually working will surface things that feel surprising. That takes more than five minutes.

The project walkthrough (Step 8) reviews every active project. For a 10–15 project load, that's meaningful time even at a few minutes per project.

The terminal inbox sweep (Phase E) processes everything the review generated. For a thorough review, that's often 10–20 new captures.

A 20-minute "weekly review" that skips the mind sweep, skips the project walkthrough, and doesn't empty the inbox isn't a weekly review — it's a shallow task skim. The 60–90 minute estimate is the honest one.

---

## Known Limitations

**Calendar integration is platform-specific.** The skill describes what to do with calendar data (previous week review, upcoming week prep) but not how to retrieve it. The platform examples show concrete implementations.

**Someday/Maybe aging is approximate.** The skill flags items in Someday/Maybe older than 1 year for potential trash. That threshold is a heuristic. Adjust based on your own sense of what "too old to be relevant" means for your context.

**The brainstorm quality depends on source quality.** The agent-led brainstorm (Step 11) is only as good as the source material — daily notes, recent meeting notes, previous week-ahead signals. If these are sparse or absent, the brainstorm falls back to generic. The fix is better capture hygiene during the week, not a better brainstorm algorithm.

**No deduplication across the two sweeps.** An item can appear in both Phase B (from the inbox before the review) and Phase E (if it surfaces again during Get Current). The skill doesn't automatically detect this — the user's Phase B confirmation serves as the dedup signal.
