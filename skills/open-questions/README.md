# Open Questions

A thinking-partner skill for sitting with hard questions. The goal is not to answer the question — it's to think more honestly and deeply about it.

One question per session. One exchange at a time.

---

## What It Does

You bring a question you can't stop thinking about. The agent doesn't try to answer it. Instead it helps you explore the competing interpretations, name the real tension underneath, and surface a sharper version of the question than the one you started with.

A session ends not with an answer but with a clearer question, a sentence worth keeping, and a decision about where it goes next.

---

## How It Works

1. You say "let's work on a question" — or paste one directly
2. The agent lists your open questions (if you have a folder of them) and asks which one to sit with
3. You pick one, or paste a new question
4. The session runs as a slow back-and-forth: exploring, complicating, following threads
5. When it feels complete, you decide where the question goes — newsletter seed, new note, refined, or left open

---

## The Discipline

The agent operates under a strict posture:

- Does not summarize
- Does not conclude
- Does not optimize for clarity or usefulness
- Does not push toward resolution
- Asks one question per response — never a list

If your thinking sounds too clean, it will complicate it. If you're borrowing a familiar frame, it will challenge it. If a metaphor appears, it will test it rather than extend it.

---

## Prerequisites

- A folder of open questions (optional but recommended): `{questions}/`
- Each question note has a `status:` field (`open`, `promoted`, `discarded`) and a `# Question` section

Without the folder, you can paste any question directly to start.

---

## Quick Start

1. **Copy `SKILL.md`** into your agent's persistent instructions
2. **Create a questions folder** at `{questions}/` — or point it at whatever folder you keep questions in
3. **Say** "let's work on a question" or paste one directly

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic / other | [examples/generic.md](examples/generic.md) |

---

## Design Rationale

See [SPEC.md](SPEC.md) for the full rationale, including the session structure, posture model, and exit decision logic.
