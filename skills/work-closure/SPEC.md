# Work Closure — Design Spec

This document explains the design decisions behind the Work Closure skill.

---

## The Problem

End-of-day GTD shutdown is the most skipped step in a knowledge worker's system. The reasons are predictable: by the end of the day you're tired, the inbox feels like a cliff, and the friction of routing decisions is high enough that "I'll deal with it tomorrow" wins. Tomorrow's agent (or tomorrow's you) starts with no clean slate.

This skill exists to lower that friction to near-zero by delegating the scanning, proposing, and writing to an agent — you only confirm.

The secondary problem: open loops you never wrote down. Source scans surface what got captured. The explicit open-loops sweep (Step 3b) surfaces what didn't. Without both, closure is incomplete.

---

## Why GTD Naming Is Non-Negotiable

Two specific failure modes drove the hard naming rules:

**The "nearest project" failure.** An agent sees a task mentioning "data" and routes it to the nearest project with "data" in the name — regardless of whether that project's outcome has anything to do with the task. The fix: route against the project's stated outcome, not its name.

**The "topic label" failure.** An agent captures "QR code findings hunt" as a task. It's not a task — it's a label for unclarified work. A real task would be "Investigate QR code open rate drop in August data". The fix: if the verb isn't nameable, the item isn't clarified. Ask before creating.

Both failures produce a project system that looks complete but contains noise — tasks that never get done because they're either in the wrong project or aren't actually actionable.

---

## Why the Inbox-Empties Rule Is Absolute

"Sit on this for now" is a disposition that allows the inbox to persist. Once the inbox is allowed to persist, it grows. Once it grows, it becomes a place where things go to die rather than a capture point that gets processed.

The rule is simple: the inbox is a temporary holding point, not storage. Every item has a destination — a project, an area, the archive, or trash. If the destination isn't clear, the right response is to create a parking note in a project's notes section (or `{archive}/Inbox/`) with an explicit note about what's unclear. That's still a destination.

The skill enforces this by treating an uncleared inbox as an incomplete run. The inbox verification gate exists specifically to surface any stragglers before close.

---

## The Batch Model

The skill collects everything before routing anything. This is a deliberate departure from the natural instinct to route-as-you-go.

Batch collection has two advantages:

1. **Holistic routing.** Seeing all items together surfaces conflicts — two tasks that are really the same thing, an item that's already tracked, a set of tasks that together imply a new project. You can't see these conflicts if you're routing one item at a time.

2. **One confirmation.** Routing-as-you-go means a confirmation call for every item. Batching means one confirmation for everything. For a typical day's inbox (5–15 items), the difference in friction is significant.

The cost is a longer Phase A — but Phase A is largely silent (source scans run in parallel, subagents handle verbose reads), so the user experience is: short wait, one big list, one confirmation.

---

## The Overwhelm Tripwire

The triage gate (Step 4a) exists because workday-close is the moment when overwhelm is most likely to be visible (it's in your notes) and most easily acted on (you're in a structured workflow that can immediately execute a disposition).

The failure mode without this gate: you write "I'm drowning" in your notes, the skill ignores it, the workday closes, and tomorrow morning-review inherits an unaddressed overwhelm signal. The overwhelm compounds.

The gate doesn't require a full triage of the plate. It requires exactly one item to get one concrete move. That's the minimum necessary to break the accumulation cycle.

"Waved off" is an explicit outcome — the user may have named overwhelm rhetorically rather than literally. But it must be explicit, so the signal isn't silently discarded.

---

## The Absorbed Line

The "Absorbed" line in the workday summary is telemetry, not judgment. The distinction matters.

A high-volume day where you absorbed three unplanned requests and nothing slipped is a strong day. A day where you absorbed one request and two focus items slipped is a different story. The absorbed line doesn't evaluate — it records. The weekly review's allocation read is where patterns become visible.

"Displaced: nothing (scope growth)" is an important variant: it means the absorbed work got carried *in addition to* the original plan, expanding total scope. Repeated entries of this type are a workload signal for the weekly read.

---

## The Phase Structure and Where Day-Closure Fits

Work closure is explicitly the *operational* shutdown. It processes the GTD system. Day-closure is the reflective shutdown — it reads the work-closure summary and asks what the day meant.

These are different jobs and they require different cognitive modes. Mixing them degrades both: GTD processing gets contaminated by reflection, and reflection gets contaminated by routing decisions.

The skill offers day-closure as a one-line follow-on (Step 4c). The offer is made once and never repeated. The marginal cost is small because day-closure reads the summary that was just written — it doesn't need to re-scan anything.

---

## The Open-Loops Sweep vs. Source Scans

Source scans (Steps 1–3) surface everything that was written down. The open-loops sweep (Step 3b) surfaces what wasn't.

The sweep is grounded in evidence already in hand — focus items that slipped, meetings that ended on unresolved asks, notes that gestured at something without landing a next action. The agent doesn't ask "anything else?"; it asks specific questions derived from what it just read.

The distinction between a *loop* (something needing an action or decision) and a *thread* (something needing meaning or reflection) is important here. Threads belong to day-closure. A loop captured in work-closure that turns out to be a thread gets routed to day-closure's domain — it shouldn't be forced into a next action.

---

## File Move Protocol

The copy → verify → delete protocol for file moves is not paranoia. Moves have produced empty or partially truncated destination files in practice — particularly with files that are large or have non-ASCII content. The verification step catches this before the original is removed. The extra cost (a byte-size check + diff) is negligible against the cost of losing a meeting note.

---

## Integration with the Daily Rhythm

```
Morning review
    ↓ reads yesterday's work-closure summary, sets today's focus
Day's work
    ↓ focus items get checked, notes accumulate, inbox fills
Work closure (this skill)
    ↓ empties inbox, routes open loops, writes workday summary
Day closure (optional)
    ↓ reads workday summary, reflection, forward signaling
Next morning-review
    ↓ reads day-closure output, inherits clean slate
```

Work closure is the pivot between execution and reflection. It doesn't do reflection — it makes reflection possible by clearing the operational residue first.
