# Salon

The single conversational thinking skill. One mirror, several lanes — an open question, a book or film you just encountered, a half-formed idea, or an open-ended "let's talk about" opening. It detects which kind of session this is from your opening line and never asks you to pick a mode.

Where most AI assistance defaults to resolving — take the input, apply knowledge, return an answer — this skill defaults to staying with the question. It works when your thinking moves, including when it moves because the agent got out of the way.

---

## What it does

You open a conversation — a question, a book, a film, an idea, or nothing in particular. The skill reads the opening and silently routes to one of five lanes, then holds one consistent posture across all of them: stay with uncertainty, one move per response, never rush to resolve or produce.

A session closes with two automatic housekeeping writes (a session log and a daily-note breadcrumb, if you keep those), then a **translation offer** — a chance to turn what surfaced into a durable artifact: a sharpened open question, a fleeting note for your writing pipeline, or a personal anecdote worth keeping as raw material. You can take any, all, or none.

---

## The five lanes

| Opening signal | Lane | What happens |
|---|---|---|
| "let's work on a question", references a question note | **Open question** | The deepest lane — a four-beat arc (the pull → competing interpretations → the real tension → the better question) |
| Names a book | **Book** | Pulls your reference/highlight notes on it silently, discusses without reciting them back |
| Names a film, show, or other watched media | **Film / media** | Pure discussion — you're the only source |
| A concept or half-formed thought with no source text | **Idea** | Optional one-shot search across your notes for adjacent material, then discussion |
| Anything else | **Anything** (default) | Loose conversation, no forced structure |

Lane detection is silent and happens once, at the start. If it guesses wrong, the conversation self-corrects — the skill never interrupts to ask which mode you meant.

---

## The discipline

The agent operates under a strict posture, held throughout every lane:

- Does not summarize, conclude, or push toward resolution
- Does not flatten ambiguity or turn the session into advice, a list, or a lesson
- Offers **ideas** freely (its own reads, take-it-or-leave-it) but never states a **read of you** (a claim about your motives or history) without a source — an unsourced read is a question, not a statement
- One move per response — never a wall of questions
- "No move" is a valid turn — reaching for something to say when you haven't got one is the failure this skill is most prone to
- Watches for its own worst tell: the construction "it's not that A, it's that B," which sounds like insight and often isn't

---

## Prerequisites

Your agent needs:

- **File read/write access** to your notes directory
- **An open-questions folder** (optional but recommended) — see [open-questions](../open-questions/) for the note format this lane shares
- **A fleeting-notes folder** (optional) — where the translation offer's "claim forming" artifacts land
- **An anecdotes folder** (optional) — where lived scenes worth keeping land
- **A semantic search tool** (optional) — used for a one-shot background query in the open-question and idea lanes; skipped cleanly if you don't have one

Every folder is optional independently. Without any of them, the skill still runs as pure conversation — it just has nothing to read at session start and offers nothing to write at session close beyond the plain writing seed.

---

## Quick start

1. **Set the paths** in your agent's system prompt or config file (see Configuration in `SKILL.md`)
2. **Copy `SKILL.md`** into your agent's persistent instructions
3. **Trigger it:**
   - "Let's work on a question"
   - "I've been thinking about [x]"
   - "I just read [book] — [x]"
   - Any open-ended "let's talk about" opening

---

## Platform examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic (any file-access agent) | [examples/generic.md](examples/generic.md) |

---

## Design rationale

See [SPEC.md](SPEC.md) for why the skill merges what used to be several separate thinking modes into one, why "ideas" and "reads" are held to different evidentiary standards, and why the translation offer never auto-writes.
