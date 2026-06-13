# Day Closure — Design Spec

This document explains the design decisions behind the Day Closure skill.

---

## The problem

End-of-day reflection is easy to skip. Most implementations make it expensive: long forms, many prompts, mandatory sections, a review of every hour. The result is a ritual that doesn't run on tired evenings — which is precisely when reflection matters most.

The failure mode is predictable: a thoughtful design gets abandoned after a few days because the cost of doing it always exceeds the cost of skipping it. A ritual that doesn't run reflects nothing.

Day Closure is built against that failure mode. Every design decision can be traced to one question: **would this run on a hard evening, or would it get skipped?**

---

## Short close by default

The short close is the product. Two turns, one gate, done.

The agent reads the day, proposes a reflection entry plus a forward signal, and waits for approval. On approval, it writes everything in one pass. The user's total interaction is: read the read, say "approve", done.

Deep mode exists because some days earn it — a genuine thread, an unresolved tension, a moment that needs processing. But deep mode is opt-in, entered only when the user names a thread in response to Step 1. The agent never pushes for depth. It never treats a clean day as evasion.

This asymmetry is deliberate: the short close should be nearly free; depth should cost only what the user chooses to spend.

---

## Volume is not slippage

A high-output day (many sessions, a lot shipped, back-to-back decisions) should read as a full day, not as a warning sign. The agent must not interpret density as "loss of measure" or over-commitment.

Similarly, a quiet day — focused on one thing, not much logged — is not a failure. It may be exactly the right day.

The reflection's job is to read what the day was, not to assess whether it was correct. Productivity accounting is not reflection.

---

## Forward signal — voicing rule

The forward signal is the most important output. It is not a task list, a summary of today, or a second "how today went." It is one thing: the thread, posture, or edge that tomorrow should remember about today.

The voicing rule matters:

- Prefix `**Signal from yesterday:**` so the morning reader immediately knows this was written the night before.
- Body in present tense from tomorrow's perspective: "Today's question is..." not "Tomorrow's question is..."

The reason: a morning reader starts a fresh session. If the signal is written as a past-tense memo from a prior day, it reads as history. Written in the morning's present tense, it reads as alive context. That framing shift is small but meaningfully affects how it lands.

---

## Deep mode constraints

Deep mode is bounded deliberately:

- 2–5 dialogue turns, not open-ended
- One thread at a time; no topic tour
- One alternate thread offered once; the user's answer is taken at face value
- Never interrogating or pushing the user toward a particular reading
- Converges to a distillation the user approves, then returns to Step 2

The constraint that matters most: the agent may not treat a positive or tidy account as evasion. If the user says "it went well, nothing to unpack," that's the read. The agent does not push.

---

## What the agent reads

The primary input is the user's own record — whatever they wrote during the day. The agent's read is downstream of that; it proposes an interpretation of what the user said, not an independent assessment.

The rule "never invent content" follows directly: if the user didn't record it, the agent doesn't know it happened. The reflection must trace to something in the record or something the user said during the close, not to inference.

---

## Retroactive close

When yesterday's close was skipped, morning-review or a similar planning skill can offer a catch-up. The retroactive close follows the same shape as a same-day close, with two differences:

- All writes go to the target day's note (not today's).
- The forward signal becomes a carry line if anything from that day should shape today; otherwise it's skipped, because there's no tomorrow reader for yesterday's signal.

The timestamp annotation (`done YYYY-MM-DD morning`) is mandatory — the entry must not appear to have been written in real time.

---

## Optional reflection hook

Some practitioners keep a personal reflective practice — a divination tool, a values check, a gratitude prompt, a prayer, a kōan — that produces entries they want to resolve at day's end. The skill accommodates this without depending on it.

When `REFLECTION_HOOK` is `true`:

- The agent glances for an open loop during the pre-read (an entry made today, or a prior entry whose reflection deadline has arrived and is still empty).
- If a loop exists, one clause is appended to the Step 1 message: "your [practice name] loop is open — say the word if it came up today."
- If the user engages it, the close includes a brief reflection on how the practice bore on the day — not a second interpretation of the original entry, just a note on how it sat.

The hook is scoped narrowly:
- Never re-interpret the original entry unless the user explicitly asks.
- Never surface a loop whose reflection date is in the future.
- The practice is one optional thread, not the frame for the whole close.

What your practice is, what format it produces, and where it's stored are entirely yours to define. The SKILL.md uses `REFLECTION_HOOK: true/false` as the only configuration switch; the specifics of reading and writing to your practice's files are for you to fill in when you implement.

---

## Capture scans — opt-in only

Two optional scans — atomic ideas and story moments — are available after the close if the user opts in.

Both scans are opt-in for a practical reason: most days produce nothing worth capturing into a writing pipeline. Running empty scans at the end of every day is ceremony, not signal. The agent offers the scans in Step 1 only when the day's notes visibly contain story material; otherwise it doesn't mention them.

The structural test for fleeting notes (the title must be a complete claim, not a topic) exists to prevent the most common capture failure: collecting topics instead of ideas. An idea you can't state as a claim probably isn't an idea yet.

---

## What this skill is not for

- **GTD processing.** If unprocessed operational residue surfaces during the close, name it in one line and move on. Work-closure owns that scope.
- **Weekly synthesis.** Weekly-review handles multi-day patterns.
- **Catch-all journaling.** The scope is understanding the day that just happened.

---

## WAT mapping

| Layer | This skill |
|-------|-----------|
| **Workflow** | `SKILL.md` — the full step sequence, gates, and rules |
| **Agent** | Any capable model; Opus/Sonnet recommended for the interpretive read |
| **Tools** | File read/write; system clock for timestamps |
