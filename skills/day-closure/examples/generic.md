# Day Closure — Generic Implementation

This guide covers the platform-agnostic implementation pattern. Use it if you're integrating day-closure into a custom agent framework, a non-Claude Code environment, or building your own orchestrator.

---

## What you need

- An agent with read/write access to your notes filesystem
- A daily note file for today (readable)
- A daily note file for tomorrow (writable; create from template if absent)
- System clock access (for accurate timestamps)
- A trigger mechanism — manual command, a hook from another skill, or a scheduled job

---

## Trigger phrases

Wire your agent to activate the day-closure skill on:

- "close the day"
- "day closure"
- "/day-closure"
- "close yesterday" (retroactive mode)

---

## Minimal daily note structure

The skill reads from and writes to a daily note. You control the section names via SKILL.md configuration. A minimal structure that works:

```markdown
# 2025-03-25

## Today's context
<!-- tomorrow's signal lands here -->

## Notes
<!-- freeform notes from the day -->

## Work closure
<!-- upstream work-summary section, if you have one -->

## How today went
<!-- day-closure writes here -->
```

If your daily note uses different headings, update the Configuration table in `SKILL.md` to match.

---

## What the skill does at each step

### Step 1 — The read (one agent turn)

The agent reads today's note in full. It forms a private read, then sends one message:

1. One or two sentences on what the day was actually about — opinionated, not a recap.
2. If a thread has charge, it names the thread.
3. The question: "Short close, or stay with [thread]?" (or "Anything still carrying, or short close?" when nothing shows).

The user either names a thread (→ deep mode) or signals short close (→ Step 2).

### Step 2 — The proposal and write (one agent turn + one gate)

The agent proposes in one message:

1. "How today went" — 2–5 sentences, first person, concrete.
2. Forward signal — see voicing rule below.
3. Reflection hook line — only if configured and the user engaged it.

Then asks: "Write the close — approve or edit?"

On approval: writes the reflection, the forward signal to tomorrow's note, and a close timestamp to today's Notes section. Done.

---

## Forward signal voicing rule

The signal must be:

- Prefixed with `**Signal from yesterday:**`
- Written in present tense from tomorrow's perspective ("Today's question is...", not "Tomorrow's question is...")
- One thing only — not a task list

Examples:
```
**Signal from yesterday:** Don't begin today in reaction. Choose one deliberate start before opening the flood.
```
```
**Signal from yesterday:** The issue today is not volume; it's avoidance of the higher-leverage conversation.
```

---

## Retroactive close (yesterday)

When closing a day that has already passed:

- All writes go to the **target day's** note.
- The forward signal becomes a carry line: if anything from that day should shape today, append it to today's `Today's context` section; if nothing carries, skip the signal.
- Timestamp annotation: `- HH:MM — (Day closure for YYYY-MM-DD, done YYYY-MM-DD) ... #day-closure`

---

## Deep mode

Available but not default. The agent enters deep mode only when the user names a thread in response to Step 1. Deep mode is 2–5 dialogue turns, converges to a distillation the user approves, then returns to the Step 2 write gate.

The key constraint: the agent never pushes for depth. If the user says the day held and nothing needs unpacking, that's the read.

---

## Optional: personal reflection hook

If you keep a reflective practice — a daily prompt, a values check, a divination tool, a gratitude entry — you can wire an optional step to close open loops at day's end.

Set `REFLECTION_HOOK: true` in your SKILL.md configuration. Then add platform-specific notes telling the agent:

- Where your practice journal lives
- What an "open loop" looks like in your specific format (e.g., an entry with an empty "Evening review" field)
- Where to write the closing entry

When enabled, the agent glances for open loops during its pre-read and, if one exists, offers a single clause in the Step 1 message. If the user engages it, the close includes a brief reflection on how the practice bore on the day. The practice is one optional thread, not the frame for the whole close.

---

## Capture scans (opt-in, post-close)

After the close, the user can ask for:

- **Fleeting note scan**: atomic ideas worth capturing — complete claims, not topics. 2–4 candidates, user picks.
- **Story-moment scan**: scenes, quotes, or observations worth keeping as raw material. 3–5 candidates, each with a "Charge" line explaining why it has pull.

Neither scan runs by default. The agent offers them in Step 1 only if the day's notes visibly contain story material.

---

## Scheduling

Day closure works on demand. If you want to automate it:

- **End-of-day hook**: trigger from a work-closure skill after the work summary is written
- **Cron / scheduled job**: run in the evening, e.g., 9–10pm local time
- **Morning catch-up**: morning-review or equivalent can offer a retroactive close when yesterday's reflection section is empty

If running automated: the agent still follows the two-turn pattern and gate. In a fully automated pipeline where no human is present, you'd need to either accept the proposal automatically (remove the gate) or design a notification path for the approval step. For fully headless operation, that tradeoff is yours to make.

---

## WAT mapping

| Layer | This skill |
|-------|-----------|
| **Workflow** | `SKILL.md` — step sequence, gates, rules |
| **Agent** | Any capable model with file access |
| **Tools** | File read/write; system clock |
