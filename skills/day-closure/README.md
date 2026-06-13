# Day Closure

A short, opinionated end-of-day reflective close. Reads the day on its own terms, proposes a "How today went" entry plus a forward signal for tomorrow — done in two turns by default.

The design premise: a reflection ritual that doesn't run reflects nothing. Every step here is sized against that fact.

---

## What it does

1. **Reads the day** — one opinionated sentence or two about what the day was actually about, drawn from whatever record exists (notes, session log, work summary). Surfaces a thread if one has visible charge.
2. **Proposes the close** — a 2–5 sentence "how today went" entry, plus a forward signal for tomorrow, in a single gated message.
3. **Writes on approval** — one pass: the reflection, the forward signal, and a close line.

That's the short close. Two turns, one gate. When a day has a real thread worth staying with, a bounded deep-dialogue mode is available — entered only by the user's choice, never by default.

---

## Design principles

**Volume is not slippage.** A high-output day is not over-doing. When the day held — work got done, conduct was fine, nothing is unresolved — say so plainly and close short. Never manufacture a thread or interrogate a clean day for a hidden edge.

**Forward signal, not task list.** Tomorrow's signal is one thing: the thread, posture, or unfinished edge that matters. It's written from tomorrow's perspective so the morning reader sees it as live context, not a memo from last night.

**Reflection, not audit.** The purpose is to understand the day, not account for every hour. A clean day closes in two short paragraphs. A charged day earns more.

---

## What it is not for

- GTD clarification or inbox processing (that's work-closure's job)
- Weekly pattern review
- Re-doing work that a prior closure skill already handled

---

## Prerequisites

Your agent needs:

- **Read/write access** to a daily note or equivalent workspace file
- **The day's record** — some source of what actually happened (session log, work summary, notes)
- **Tomorrow's note** — or the ability to create one from a template

---

## Workspace structure

The skill uses placeholder paths you configure once:

```
{workspace}/
├── daily/
│   ├── YYYY-MM-DD.md       # Today's daily note (read + write)
│   └── ...
└── ...
```

Replace `{workspace}` with your actual path. The specific section names inside the daily note (Focus, Notes, Work closure, How today went, Today's context) are configurable — see SPEC.md.

---

## Optional: personal reflection hook

The skill has an opt-in step for a personal reflective practice — for example, checking whether a divination reading, a values prompt, a gratitude entry, or any other practice you keep had any bearing on the day. This step is entirely optional and off by default. See SPEC.md for how to wire it in.

---

## Quick start

1. **Copy `SKILL.md`** into your agent's persistent instructions.
2. **Configure the paths** — replace `{workspace}` with your actual workspace path in SKILL.md.
3. **Run manually first** — say "close the day" and review what it proposes before approving.
4. **Schedule or trigger** — works on demand, or wired to a work-closure skill that calls it at end of day.

---

## Platform examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic / any agent | [examples/generic.md](examples/generic.md) |

---

## Design rationale

See [SPEC.md](SPEC.md) for the full design rationale, including the short-close-by-default decision, the forward signal voicing rule, deep mode constraints, and the opt-in reflection hook.
