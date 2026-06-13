# Operating Audit — Design Spec

This document explains the design decisions behind the Operating Audit skill.

---

## The Problem

Most "how am I doing" prompts produce validation. The agent scans your notes, reports a summary, and ends with encouragement. The user learns nothing they didn't already know or suspected. Nothing changes.

A behavioral audit should do three things a retrospective prompt doesn't: extract evidence before forming any view, evaluate the governance layer by its own standard, and name the one thing actually being avoided. Those are meaningfully different operations, and conflating them with "give me a summary of my work" is what makes most retrospectives feel like a waste of time.

---

## Why Evidence Extraction Comes Before Analysis

The failure mode in most agent-assisted audits is subtle: the agent forms a view from the conversation's context window, then selects evidence to support it. This looks like analysis but is actually post-hoc rationalization. The agent finds what it expects to find.

The fix is a strict separation: Step 1 (evidence extraction) runs to completion before Step 2 (diagnosis) begins. The extraction is deterministic — daily notes, session logs, project files, governance components. The analysis only runs on what was extracted, not on memory of the window.

This matters most for findings about volume and pacing. An agent with a "busy = stressed" prior will mis-read a productive build week as a friction signal. Forcing the extract first surfaces the actual distribution: was intent met? Did builds cash out?

---

## Volume Is Not Slippage

This principle needs a spec entry because it's the most common audit error and the most institutionalized one. Many productivity frameworks treat high system usage, heavy build output, and metawork-heavy weeks as warning signs. For someone whose primary work is building systems, this is backwards.

The auditable questions are routing and payback:
- **Routing:** did discretionary margin actually go where stated intent earmarked it? If the week-ahead signal says "give the migration real time" and the logs show the migration got nothing, that's a routing failure worth naming.
- **Payback:** do the builds' stated benefits materialize? A build claiming "unblocks weekly review" should show up in review quality or speed over the following weeks. If it doesn't, that's a payback failure.

Pathologizing volume itself — "you worked a lot on the system this week" as a finding — produces the opposite of useful signal. It trains the user to hide system work or feel guilty about it. The audit rejects this frame entirely.

---

## The Dogfood Clause

The governance layer — dashboards, briefs, triage gates, activity ledgers — exists to change decisions. That's the only thing that justifies its cost. A governance component that produces reports nobody reads, nudges nobody acts on, or gates that never actually stop anything is pure overhead.

The dogfood clause applies the same standard the governance layer claims to apply to everything else. Find a concrete instance where it changed a decision. If you can't find one across a 12-week window, you have a governance layer that creates the appearance of rigor without the substance.

Two consecutive audits is the threshold before a deletion recommendation. One missed window might be a slow quarter. Two in a row is a pattern.

This is the most uncomfortable part of the audit for users who have invested in building governance. The discomfort is the point: governance that survives only because its cost is invisible isn't governing anything.

---

## The One Thing Avoided

Most audits end with 5–8 "areas for improvement." The user reads them, nods, and changes nothing because there's no forcing function — everything is equally important and therefore nothing is.

The "one thing avoided" close is the forcing function. It names exactly one tension — the one with the most recurring evidence and the least acknowledgment in the notes and logs. Not a category ("time management"). A specific pattern tied to specific dates.

The rule is: exactly one. Not a list. Not "one main thing and a few related areas." One. The constraint forces the audit to do the hardest editorial work: ranking the findings and calling the sharpest one explicitly.

"Not softened" is also non-negotiable. The avoidance language in audits ("you might want to consider whether...") is a courtesy that costs the user the finding's utility. Name it as a tension, state the evidence, stop.

---

## Why the Log Extraction Uses Targeted Sections

Naive log audits try to read every log in full. On any system with months of logs, this approach dies — it runs out of context, times out, or produces an incoherent summary of too much material.

The targeted extraction (open items / next actions / pending sections only, head-25 per file) scales to large log stores because it extracts only the signal most relevant to the audit's questions: what was surfaced but not closed? The full narrative of each session is not needed for the audit; the threads that didn't close are.

This is a meaningful tradeoff: you might miss context from narrative sections that explains why something didn't close. The fix is in the daily-notes extraction, which captures "How today went" quotes and avoidance statements verbatim. The two sources complement each other.

---

## Conduct-Practice Boundary

The audit owns what the plate contains and where time went. It does not evaluate personal development practices, reflection journals, or spiritual practices — not because these don't matter, but because they're the wrong instrument for that evaluation. The practices own how days feel and what conduct held. The audit reads their outputs as evidence (a reflection that surfaces avoidance, a practice note that flags a recurring tension) but never grades the practices themselves.

This boundary prevents a specific failure mode: an audit that pathologizes the absence of visible output from practices that don't produce visible output. Meditation, journaling, and reflection work on a different time horizon than project execution. Treating their absence from the logs as a finding is the wrong lens.

---

## Known Limitations

**Evidence quality depends on note quality.** An audit over sparse notes produces sparse findings. The fix is better capture hygiene during the window, not a better audit algorithm.

**The dogfood clause requires memory across audits.** The "two consecutive audits" threshold requires knowing what the previous audit found. If audit notes aren't saved, this degrades to single-window evaluation. Save the deliverable.

**Parallel log extraction requires file-system access.** The targeted section extraction is a bash loop. Agents without shell access need to replicate it with sequential file reads. Equivalent results, more context cost.

**No automated baseline.** The audit compares against stated intent (Focus items, week-ahead signals, stated goals) — not against an external benchmark. If stated intent is systematically vague, the routing check loses resolution. The fix is sharper intent statements, not a better audit.
