# Morning Review — Design Spec

This document explains the design decisions behind the Morning Review skill.

---

## The Problem

Most people have more than one place where work accumulates. Email, a task manager, sticky notes, a voice memo app, a "quick capture" inbox, yesterday's unfinished list. Before you can plan today, you need to drain all of it — and the draining itself is a decision-making process, not just a mechanical sweep.

Without a structured review, the day tends to start from whatever floated to the top of memory. The important-but-not-urgent item that got captured on Tuesday gets buried. The meeting note from yesterday with action items sits unprocessed. The thing you meant to do last week carries silently.

The morning review solves this by forcing a complete collection pass before any planning happens, then routing everything in one confirmed batch, then setting Focus with full context.

---

## Why "Batch Collect, Then Clarify"

The natural failure mode of a morning review is early-commitment: the agent sees a meeting note, immediately proposes routing it to a project, the user accepts, and then a different project would have been more appropriate once the full landscape was visible. Or the agent asks "where should this go?" for each item individually, turning a 10-minute review into a 45-minute conversation.

The batch model separates collection from routing:
- Phase B is parallelized and largely silent — collect everything first
- Phase C opens with a view of the full active-project landscape, then proposes dispositions for the whole batch
- The user confirms or overrides in one pass

This is more efficient and produces better routing decisions because the full picture is visible before any item is committed.

The gate at the end of Phase B is structural, not ceremonial. The agent shows the list and asks "ready to clarify?" — this prevents the agent from sneaking routing decisions into the collection phase.

---

## The Open-Loops Sweep Design

The system surfaces what's captured. There is always a gap between what's in the system and what's live in the user's head — especially after time off, or on days when captures didn't happen.

The sweep closes that gap. But a flat "what's on your mind?" is a low-quality question. It puts the burden on the user to inventory their own state, which is exactly the thing they're struggling with when loops are open.

The better design: prime from evidence. Before entering the conversation, the agent reviews what's already loaded — yesterday's forward signal, the calendar, the overdue scan, carried Focus items. From this, it identifies 2–3 candidate threads that might be live: the item that's been carried for three days, the commitment phrased as undone in recent notes, the signal from last night that the day might have shifted.

These aren't read back as a list. They're fuel for grounded questions: "Last night's signal said X was urgent — is that still true?" The user doesn't have to inventory; they just have to react to a specific probe.

This is the daily, lightweight version of the weekly mind sweep. It's not a territory map across eight life areas. It's a focused catch: what's live today, overnight, or after time off.

---

## GTD Naming Enforcement

Two naming rules that are enforced throughout:

**Projects are outcomes.** A project is not a topic or a domain — it's a commitment to a specific deliverable or state change. "Tech desk pilot" is not a project. "Run the 90-day tech desk pilot to working v1" is a project. The distinction matters because outcome-stated names tell the agent how to route tasks (does this task move the outcome forward?) and tell the user what they're actually committed to.

When the agent can't phrase a proposed project as an outcome, it asks — because that usually means the user hasn't yet decided what "done" looks like. That's a valuable clarification to surface before creating a folder.

**Tasks are verb-led.** A next action is an atomic physical step: "Draft response to vendor", "Review Q3 data summary", "Call the events team". "QR findings hunt" is a label for unclarified work, not an action. The agent refuses to route topic labels as tasks and asks instead: what specific action moves this forward?

---

## The Anti-Fabrication Constraint

The morning review processes meeting notes that often include automatically transcribed content. Speaker attribution in these transcripts is unreliable — transcription models diarize speech into turns but frequently misattribute turns between similar-sounding speakers, or hallucinate a confident attribution from tone alone.

A fabricated attribution that makes it into a task ("X promised to send the report") becomes a real commitment. The agent then sends a follow-up. The person never made the promise. The relationship takes a hit.

The constraint is strict: every named entity in any extraction must be paired with a verbatim source line. If the agent can't quote the source, it doesn't have it — it flags the meeting as "needs human review" instead of guessing.

For attributed actions from meeting notes, the safe form is "Follow up with [person] about [topic]" rather than "[person] committed to [action]" unless the content makes the commitment unambiguous.

---

## The "Inbox Empties" Rule

This is the hardest rule to enforce and the most important one. Partial inbox processing is not inbox processing — it's selecting which items to deal with and deferring the rest, which rebuilds exactly the pile the review was meant to clear.

The rule is absolute: every item in every inbox gets a disposition in Phase C, and every processed item is removed from the inbox at execution time. The inbox verification step forces a final check before Phase D begins.

The one acceptable gray area is the "Unclear → propose new project / ask user" disposition. This acknowledges that some items don't map cleanly to any existing outcome and that forcing them into the nearest project would be worse than asking. But even this requires a decision — the item doesn't stay in the inbox, it gets explicitly parked in a project's Notes section or an area hub with a specific reason.

---

## Focus Requires Explicit Confirmation

The agent recommends Focus items but does not write them until the user explicitly names the list. The recommendation accounts for meeting load, overdue items, and Phase C routings. But the user owns the Focus decision.

This matters because:
- The agent doesn't know everything about the user's day — a conversation that happened offline, an energy state, a commitment that wasn't captured
- Focus is the most consequential write of the morning review — it shapes how the whole day is spent
- Having the user name the items (rather than just approve a list) creates active buy-in

The capacity check against meeting load is context, not a limit. Heavy meeting days get a suggestion to pull Focus down to 1–2 items, but the user can override. The point is that the recommendation should be grounded in the actual day, not a default count.

---

## What Belongs in a Morning Review vs. a Weekly Review

The morning review is a daily catch, not a weekly sweep. It processes what accumulated overnight and sets today's Focus. It does not:
- Re-evaluate which projects are still worth doing
- Run a full territory map across all life areas
- Review horizons (purpose, vision, goals)
- Resurface items that were deliberately deferred to future weeks

The weekly review does all of that. The morning review's open-loops sweep is the daily lightweight sibling — one or two grounded probes, not a comprehensive inventory.

Running a weekly-depth review every morning is a known failure mode. It takes too long, surfaces too much, and creates decision fatigue before the actual day has started.

---

## Known Limitations

- **Calendar integration is platform-specific.** SKILL.md describes calendar access in abstract terms. Platform examples show how to wire it up per environment.
- **System health dashboard is optional.** Workspaces without one fall back to the overdue scan and waiting-for check as the health signal.
- **Meeting transcript quality varies.** The anti-fabrication constraint is strong but cannot catch a well-written fabrication. The human review flag on meeting notes helps, but is not a substitute for the user reading the note.
- **The open-loops sweep is conversation-quality-dependent.** The priming is only as good as the context loaded in Phase A and the Phase B scans. On a day with thin context (no forward signal, no overdue items), the sweep has less to work with.
