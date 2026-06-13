# Voice Development

Long-term writing voice development coaching. Mirror and analyst role only — the agent never generates prose for publication.

---

## What it does

This skill coaches writing voice development over a 12-month arc through five sequenced exercises:

1. **Voice archaeology** — surface patterns already in your writing you can't see yourself
2. **Imitation and departure** — use a writer's specific moves to discover what you do differently
3. **Constraint design** — force yourself off default patterns to reveal options you didn't know you had
4. **Socratic interrogation** — strip hedging to find the conviction underneath
5. **Style codification** — extract a personal style guide from accumulated exercise data

Plus freewriting (ongoing), weekly check-ins, and a status snapshot command.

---

## The one rule

**The agent is a mirror, not a writer.**

It analyses, reflects, quotes back, and asks questions. It never writes prose you would publish or claim as your own voice. If you ask it to rewrite something, it redirects to the Socratic exercise instead.

When you paste writing, the agent's job is to show you what it's doing — not to improve it.

---

## Triggers

| Command | What it does |
|---|---|
| `/freewrite` | 10-minute prompt + analysis of your output |
| `/exercise archaeology` | Voice archaeology exercise |
| `/exercise imitation [writer]` | Imitation and departure exercise |
| `/exercise constraints` | Constraint design exercise |
| `/exercise socratic [topic]` | Socratic interrogation |
| `/exercise codification` | Style guide extraction |
| `/check-in` | Weekly 5-10 minute check-in |
| `/voice-status` | Current phase, recent work, what's next |

---

## Setup

See `examples/claude-code.md` or `examples/generic.md` for platform-specific setup.

You'll need:
- A voice development folder (`{voice_dir}`) with subfolders for freewriting, exercises, and a progress tracker
- 3 starter files in `{voice_dir}`: `Voice development progress.md`, `Voice profile.md`, `Personal writing style guide.md`

The skill reads these at session start and writes exercise outputs back to the folder tree.

---

## WAT mapping

| Layer | Implementation |
|---|---|
| **Workflow** | `SKILL.md` — five-exercise arc + session types + cadence |
| **Agent** | Any capable LLM (Opus recommended for archaeology + codification) |
| **Tools** | File read/write for exercise outputs and progress tracking |

---

## Design notes

See `SPEC.md` for the full rationale: why the mirror constraint, the exercise sequencing logic, the freewriting prompt design, the suggested writers list, and what this skill is not.
