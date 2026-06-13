# Open Questions — Design Spec

This document explains the design decisions behind the Open Questions skill.

---

## The Problem

Some questions don't want to be answered — they want to be understood.

The standard mode of AI assistance is to resolve: take a question, apply knowledge, return an answer. For genuinely open questions — the kind you keep returning to, the kind that resist clean resolution — this is exactly wrong. Premature resolution forecloses the thinking. You get an answer that fits the frame you already had, not a better frame.

The failure mode looks like productivity: the agent sounds helpful, produces clear output, and the user nods along. But the question that mattered — the one underneath the one that got answered — never got touched.

Open Questions flips the default. The goal is not resolution. The goal is honest, slow thinking — the kind that surfaces competing interpretations, names real tensions, and ends with a sharper question than the one you started with.

---

## Why One Question Per Session

The constraint is structural, not aesthetic.

When you work on multiple questions in a session, you optimize for coverage. Questions get lighter treatment, you stay at the surface, and each answer closes the loop without opening a new one. Progress feels real but isn't.

Committing to one question forces depth. You have to stay when it gets uncomfortable, follow the thread even when it goes somewhere awkward, and resist the relief of moving on. The discipline is in the constraint.

---

## Why One Response at a Time

The pacing discipline prevents a second failure mode: the agent performing depth rather than creating it.

If the agent fires off three questions and a synthesis in one response, the user is reacting to the agent's framing, not developing their own. The session becomes agent-led. The thinking that happens is the agent's.

One question per response keeps the user in the driver's seat. It creates space between exchanges — room for the actual thinking to happen, which usually occurs in the gap, not in the response.

---

## The Posture Model

The skill's posture is a specific inversion of the default assistant stance:

| Default assistant | Open Questions |
|---|---|
| Clarify and resolve | Stay with uncertainty |
| Flatten ambiguity | Explore competing interpretations |
| Move toward conclusion | Name tensions without resolving them |
| Confirm and validate | Complicate clean ideas |
| Answer questions | Ask questions |

The inversion is intentional and needs to be stated explicitly because models are trained toward helpfulness. Left to their defaults, they will summarize, conclude, and offer recommendations. The posture instructions override that. They need to be kept in the agent's working context throughout the session, not just at the start.

---

## Session Structure

The five-phase structure (the pull → competing interpretations → real tension → better question → sentence worth keeping) is loose, not rigid. It describes the arc of a good thinking session, not a procedure to execute.

In practice, you might spend three exchanges on competing interpretations and skip directly to the better question. Or the sentence worth keeping surfaces early and becomes the spine of everything else. That's fine. The structure is a map, not a script.

The only hard rules: don't jump to the better question before you've sat with the tension, and don't produce the sentence worth keeping by summarizing — it should feel like a discovery, not a synthesis.

---

## The Exit Decision

The exit decision is where the session's output gets routed. The five options cover the realistic space:

- **Writing seed** — the question has enough gravity to become a piece. Route it to an outline.
- **New note** — something claim-like has formed. It's not a piece yet but it's worth capturing as a working note.
- **Refined question** — the original question was wrong in an interesting way. The better question replaces it.
- **Discard** — the question dissolved. It was confused, already answered, or not actually worth pursuing. Mark it and move on.
- **Leave it open** — no exit yet. The question is still live. That's a legitimate outcome.

The exit is not the point of the session. A session where the question deepens and refuses to resolve is often more valuable than one that produces a clear "writing seed." Resist the urge to force a landing.

---

## The Questions Folder

The `{questions}/` folder is recommended but not required. Its purpose is to accumulate unresolved questions over time — a backlog of intellectual debt that gets worked through slowly.

Without the folder, the skill still works: the user pastes a question and the session runs. But the folder adds a layer of intentionality: questions get written down when they surface, live as open items until they're worked, and get marked promoted or discarded when they've been through a session. It makes the practice durable.

The note format is minimal by design:
- `status:` for lifecycle tracking
- `# Question` for the current statement of the question (updated after sessions)
- `# Context` for why it matters
- `# Builds on` for intellectual lineage

No tags, no complexity. The folder is a simple container, not a system.

---

## What This Is Not

- Not a brainstorming session — you're not generating options
- Not a planning session — you're not deciding what to do
- Not therapy — you're not processing emotions
- Not research — you're not accumulating facts

It's thinking. The kind that happens when you slow down, stay with something hard, and refuse to settle for the first answer that fits.

---

## Known Limitations

- **Posture drift.** Without strong posture instructions, the agent will drift toward resolution. The instructions in SKILL.md need to stay loaded throughout the session.
- **User can force resolution.** If the user wants an answer, they'll get one. The skill can resist but not prevent. The user has to want the thinking, not just the output.
- **Requires a strong model.** The posture inversions — staying uncertain, complicating clean ideas, asking one question instead of five — require judgment. A weaker model will default to summarization and advice.
