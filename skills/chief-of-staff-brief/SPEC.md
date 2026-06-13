# Chief-of-Staff Brief — Design Spec

This document explains the design decisions behind the Chief-of-Staff Brief skill.

---

## The Problem

Weekly reviews fail in a predictable way: the human arrives at the review having done no preparatory sweep, so the first 20 minutes is collection — pulling up logs, scanning dashboards, remembering what was in flight. By the time there's enough context to make decisions, the available time is gone and the review reduces to triage.

The chief-of-staff brief solves this by front-loading the sweep work. An agent runs before the human sits down, reads the same signals the human would have read, and produces a one-page brief with queued decisions and pre-drafted actions. The human arrives at decisions, not collection.

---

## Why "Pre-Draft" Not "Execute"

The skill's central design choice: it pre-drafts everything but executes nothing.

A nudge to a stale waiting-for goes in the brief as a ready-to-send one-liner — the human sees it, decides to send or drop, and acts. The agent never sends it directly. A project that should be parked goes in the brief as a park question — the human confirms. A decision that's been deferred too long goes in "decisions queued" — the human disposes.

This matters for two reasons:

**Trust calibration.** Most decisions the brief surfaces involve other people, external timing, or context the agent can't fully see. Pre-drafting costs very little. Erroneous execution is expensive — a nudge sent on a stale context assumption, a project parked prematurely. The asymmetry favors pre-draft over execute.

**Legibility.** A brief the human reads before acting creates a shared model of the system's state. Both the human and the agent end up oriented to the same picture. The human can correct the agent's read ("that item is resolved — I handled it offline"), and the agent's next brief improves.

---

## The One-Page Constraint

The brief is hard-capped at one page. This is not an aesthetic preference — it's a forcing function.

If the agent is allowed to write a multi-page brief, it will. And a long brief is almost as bad as no brief: the human skims, misses the sharp items, and the pre-drafting labor is wasted. The one-page cap forces the agent to make editorial judgments: of everything it found, what changes a decision?

If something doesn't fit, it's either not important enough for the brief (handled later in review) or the brief has too many low-value items. Cut toward the sharpest items, not toward compression.

---

## The Five Sections

### Decisions queued

The most important section. These are the items the review must dispose of — "dispose of" meaning reach a concrete decision: do it, defer it, kill it, assign it. Not status updates, not housekeeping.

The cap of 3–6 items is intentional. If you surface 15 decisions, the human is back in collection mode. If you surface 2, the brief is underperforming. 3–6 is the right granularity for a weekly review.

### Nudges pre-drafted

Stale waiting-fors are the most common form of passive loop drain — items that block progress but never get chased because chasing requires low-urgency action that always loses to higher-urgency work. A pre-drafted nudge removes the friction: the human sees the item, reads the ready-to-send message, clicks send or drop.

The nudge must be in the owner's register. "Just checking in" is not a message anyone should send. The agent should write the actual message the human would send — plain, direct, specific about what's being waited for.

### Park questions

This section only appears when specifically triggered: a project checkpoint showing consecutive misses, a zombie item on the dashboard. It is not a general "things to think about" section.

When it does appear, the question must be stated plainly. The point is to surface something the human may be avoiding — softening the question defeats the purpose.

### Allocation — actual vs intent

This section does one specific thing: compares what the logs and closures show actually happened against what the standing intent said the week was for. It is not a performance assessment.

The framing matters: volume is not slippage. A week where a lot happened is not better or worse than a quiet week. The read is about routing — did the margin that was earmarked for a specific goal actually reach that goal, or did it get absorbed elsewhere?

Two or three sentences is the right length. Long enough to be specific, short enough to resist editorializing.

### Biggest unclosed loop

One sentence. Singular. The brief should identify the loop that has carried longest or costs most by staying open — the thing that, if closed, would change the most downstream.

This is a forcing function for the agent: of all the open items in the system, which one actually matters most? If the agent can't answer that, the brief isn't ready.

---

## Evidence Constraint

Every claim in the brief must trace to a source the agent actually read in that run — a health file row, a log entry, a closure line. No vibes, no pattern-matching from prior context.

This constraint has real teeth. Without it, agents confidently surface "issues" based on accumulated priors rather than the current state. The health file says a project is active but the agent remembers it being stalled last month, so it surfaces a stale concern. The evidence constraint prevents this: if the current run's sources don't say it, it doesn't go in the brief.

---

## Headless-Safe Requirement

The brief runs on a schedule, before the human sits down. It must never block — no "should I include X?" questions when no one is there to answer.

The headless-safe requirement means: when uncertain, make a best-judgment call and flag the uncertainty inline. "Health file appears stale (last updated 38h ago) — items below reflect last known state." This is more useful than silence or an unanswered question.

Interactive runs (the human directly asks for the brief) may ask at most one clarifying question. This preserves the fast, non-interactive character of the skill while allowing for ambiguity resolution when the human is present.

---

## What the Skill Is Not

**Not a status report.** The brief is not a summary of everything that happened last week. It is a pre-digest of what needs action this week.

**Not a performance review.** The allocation read is not an assessment of whether last week was good or bad. It's a routing check.

**Not a task manager.** The brief never creates, updates, or completes tasks. It surfaces items for human decision.

**Not a sender.** Nudges are pre-drafted, not sent. The human controls all external communication.

---

## The Dogfood Clause

The skill includes an explicit survival criterion: if it consistently fails to change real decisions, it should be deleted.

The criterion matters because scheduled skills tend to persist through inertia. They run, they produce output, the output gets filed away unread. The dogfood clause forces an honest assessment: is this brief actually changing what happens in the weekly review? If the answer is no after a sustained trial period, the right call is deletion, not refinement. A brief that nobody acts on is worse than no brief — it adds noise and creates a false sense that the sweep work is being done.

---

## Design Tradeoffs

### Pre-draft depth vs. brevity
The brief pre-drafts nudges at high fidelity (ready-to-send messages in the owner's register) but keeps every other section short. This asymmetry is deliberate: nudges are the section where pre-drafting saves the most human time, so they warrant the extra work.

### Breadth vs. sharpness
The brief favors fewer, sharper items over comprehensive coverage. This trades recall (some real items won't appear) for precision (the items that do appear are worth the human's attention). The weekly review covers the rest.

### Automation vs. legibility
The brief is designed to be legible to a human who hasn't read the source material — no jargon, no internal references that require context to parse. A brief the human can't quickly read defeats its own purpose.
