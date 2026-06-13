# Process Notes — Design Spec

This document explains the design decisions behind the Process Notes skill.

---

## The Problem

Raw notes are easy to capture and hard to develop. Most systems lose ideas in one of two ways: either nothing is captured at all, or ideas accumulate as half-formed snippets that never get developed into something useful. The first problem is a capture problem. The second is a development problem.

Process Notes solves the development problem. It's for the moment after capture — when you have an idea but not yet the thinking that would make it a real, standalone note.

---

## Why Socratic, Not Extractive

An extractive approach would read the fleeting note and produce a draft main note from the available material. That's fast but it's wrong — it produces a polished version of what was already captured, not new thinking. The value of note development isn't transcription; it's the reasoning that wasn't in the fleeting note yet.

The Socratic loop forces the thinking. Each question asks for something specific — not "tell me about this idea" but "what's the strongest objection to your thesis?". The agent writes each answer into the note as it's given, shows it back, and waits for confirmation. The author is building the note in real time, not reviewing a draft.

This is slower than extraction by design. A 20-minute session that produces 500 words of genuine reasoning is more valuable than a 2-minute extraction that produces 800 words of paraphrase.

---

## One Section at a Time

The skill processes sections sequentially, not all at once. This matters for two reasons:

1. **Cognitive load.** Seeing all questions at once fragments attention. One question focuses it.
2. **Verification loop.** After each section is written, the author confirms it captures their thinking. Mid-session corrections are cheap. End-of-session rewrites are expensive.

The smart-start logic for continue mode follows the same principle: scan which sections are done, confirm them briefly, jump to the first gap. Don't re-ask what the author already worked through.

---

## The Seed Brainstorm

Every thinking session generates more ideas than belong in the note being developed. The seed brainstorm makes these ideas visible before they disappear.

The step is mandatory — even when "Questions it opens" is thin — because the absence of spin-off ideas is itself information worth making explicit. If the session returns nothing seedable, the author knows: this idea developed cleanly without spawning new threads. That's different from not asking.

The brainstorm doesn't auto-create notes. It presents candidates and waits for selection. Pre-creating seeds would clutter the notes directory with speculative seeds the author didn't consciously choose to start.

---

## The Question Quality Gate

Questions accumulate during a thinking session. Most aren't worth preserving — they're instrumental questions the session answered, or directions ruled out, or momentary rabbit holes. Saving them all would pollute the open questions store with noise.

The gate imposes editorial judgment: the author selects at most 1-2 questions that feel genuinely worth pursuing. The rest are discarded. A small, curated open questions store is more useful than a large, exhaustive one.

---

## Moving the Fleeting Note

When a fleeting note is fully developed, it moves to `{processed}/` with updated frontmatter (`status: processed`, `developed_into:` link). This preserves the provenance chain — you can trace any main note back to its source — while keeping the active fleeting notes directory clean.

The move uses copy → verify → delete, not a rename, to protect against partial failures. The fleeting note is only deleted after the copy is confirmed. If anything goes wrong mid-move, the source survives.

---

## Guiding Questions as a Compass

A standing list of the author's guiding questions — the themes and problems they're working through over months or years — acts as a compass during note development. Most ideas connect to one or two of these questions in ways the author doesn't see until someone names it.

This is optional but meaningful. The compass doesn't constrain the session; it reveals threads. If a note genuinely doesn't connect to any guiding question, that's worth surfacing too: is this exploring new territory, or is the connection obscured?

Force-fitting is wrong. Some ideas are genuinely exploratory. The compass is a lens, not a filter.

---

## Voice Standards

Note sections carry the author's voice, not the agent's. The voice standards are explicit about what to avoid — hollow intensifiers, AI signature vocabulary, undue significance — because models default to these patterns. The standards exist to produce notes that read like the author wrote them, not like a summary generated on their behalf.

The author's rough phrasing is often the right phrasing. Capturing it that way preserves the texture of the thinking. Polishing it loses the signal.

---

## Design Tradeoffs

### Speed vs. depth
The one-section-at-a-time loop with confirmation gates is slower than extraction. The tradeoff is intentional: depth over speed. A session that produces genuine new reasoning is the point. If the author needs a fast draft, this isn't the right skill.

### Flexibility vs. prescription
The skill specifies required sections but adapts to your template. The Socratic loop applies to whatever sections your system requires. This makes the skill portable across different note-taking methodologies while preserving the core interaction model.

### Mandatory seed brainstorm
Making the brainstorm mandatory means it adds time even when it returns nothing. This is acceptable — the cost of running an empty brainstorm is a few seconds; the cost of missing a genuinely good spin-off idea is losing it entirely. The asymmetry justifies the requirement.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the Socratic thinking loop, seed brainstorm, question gate, handoffs |
| **Agent** | Any LLM with file read/write access |
| **Tools** | File read/write; optionally a notes search tool for MOC linking and suggest mode |
