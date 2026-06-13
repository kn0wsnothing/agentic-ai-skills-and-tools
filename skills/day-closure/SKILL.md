# Day closure

You are running a day closure — a short reflective close for the end of the day. Default shape: **short close** — one read of the day, one gated write, done in two turns. Deep mode exists for days that earn it, entered only by the user's choice.

The governing move: **understand the day → match the depth to the day → record.** Read the day for what it actually was. Many days simply held — the work got done, nothing is unresolved. When that's true, say so plainly and close short. **Volume is not slippage** — never read a high-output day as over-doing or loss of measure.

---

## Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| `WORKSPACE` | _(required)_ | Root path for your daily notes |
| `DAILY_NOTE_PATH` | `{WORKSPACE}/daily/YYYY-MM-DD.md` | Path pattern for daily notes |
| `FOCUS_SECTION` | `## Focus` | Section heading for today's planned work |
| `NOTES_SECTION` | `## Notes` | Section heading for freeform notes |
| `WORK_SUMMARY_SECTION` | `## Work closure` | Section heading for work summary (if upstream closure ran) |
| `REFLECTION_SECTION` | `## How today went` | Section heading for the day's reflection (written here) |
| `TOMORROW_SIGNAL_SECTION` | `## Today's context` | Section heading in tomorrow's note for the forward signal |
| `REFLECTION_HOOK` | `false` | Set `true` to enable the optional personal reflection step (see Step 2) |

---

## Before starting — one read

Read the **target day's** daily note in full: the Focus section, Notes section, Work closure section (if present), and the How today went section (to confirm it's empty).

No inbox scan. No backlog processing. If upstream work-closure ran, its summary is already in the Work closure section — don't re-derive it.

### Retroactive close (catch-up mode)

When closing a past day (e.g., yesterday, because yesterday's close was skipped):

- All writes go to the **target day's** note.
- The forward signal becomes a **carry line**: if anything from that day should shape today, append it to today's note under `TOMORROW_SIGNAL_SECTION` as one line. If nothing carries, skip the signal — it has no reader.
- Use the actual current timestamp, annotated: `- HH:MM — (Day closure for YYYY-MM-DD, done YYYY-MM-DD) ... #day-closure`. Never imply the entry was written on the target day.

---

## Step 1 — Read the day (one turn)

Form your read privately from the note. The user's own framing in the Notes section is the primary signal. The Work closure section carries the operational summary. Then send **one message** containing:

1. **Your read, 1–2 sentences.** "Reading the day: ..." — what it was actually about, how it went. Opinionated and testable, not a recap.
2. **If a thread has visible charge** — something unresolved, a real tension, a moment still carrying weight — name it: "The part of today that still seems to have charge is ___."
3. **If `REFLECTION_HOOK` is enabled and an open reflective-practice loop exists**, one clause: "your [practice name] loop from [date] is open — say the word if it came up today."
4. **If the day's notes contain story material** (a quotable scene, a striking moment, a notable exchange), one clause: "there's story material in today — want the capture scans after the close?"
5. **The question:** "Short close, or stay with [thread]?" — or, when no thread shows: "Anything still carrying charge, or is this a short close?"

Never open with "How was your day?" Never manufacture a thread or interrogate a clean day for a hidden edge.

- User names a thread or asks for depth → **Deep mode**.
- Otherwise → **Step 2**.

---

## Step 2 — Short close (one gated write)

Propose everything in a single message:

1. **How today went** — first person, 2–5 sentences, concrete, covering the whole day, not a chronological recap, not productivity-only. When the day held, an honest short read is the whole entry. Built from the day's record plus anything the user just said — never invent; only reflect what the record and the user actually contain.
2. **Forward signal** — per the voicing rule below.
3. **Reflection hook line** — only if `REFLECTION_HOOK` is `true` AND an open loop exists AND the user engaged it in Step 1. 1–3 sentences, first person, how the practice actually sat with the day. "It held," "didn't really come up," and "I lost the thread" are all complete, equal answers.

**Gate — ask:** "Write the close — approve or edit?" Do not write in the same turn as the gate call.

On approval, write all parts in one pass:

- The reflection to the target day's `REFLECTION_SECTION`.
- The signal to tomorrow's `TOMORROW_SIGNAL_SECTION`. If tomorrow's note doesn't exist, create it from your template — render any template placeholders into concrete values for the correct date.
- The reflection hook entry per your practice's field conventions (only if applicable).
- A close line to the target day's `NOTES_SECTION`: `- HH:MM — Day closed: [brief summary] #day-closure`

End with: **"Day closed."**

### Forward signal — voicing rule (non-negotiable)

- Prefix with the literal string `**Signal from yesterday:**` so tomorrow's reader sees immediately that this was written the night before.
- Write the body in **present tense from tomorrow's perspective** — as if it is already the next day. "Today's question is..." not "Tomorrow's question is..."; "You start today by..." not "You'll start tomorrow by..."
- The signal captures the one thing tomorrow should remember about today — the thread, posture, or unfinished edge that matters. It is **not** a task list, a vague aspiration, or a second summary. One signal, not three.

Good examples:
- "**Signal from yesterday:** Don't begin today in reaction. Choose one deliberate start before opening the flood."
- "**Signal from yesterday:** The issue today is not volume; it's avoidance of the higher-leverage conversation."

---

## Deep mode (entered only by the user's choice)

### Dialogue — 2–5 turns

Stay with the named thread long enough for insight to emerge. The purpose is to help the user discover what they think, not to extract a confession.

You may:
- Offer an interpretation the user can accept or reject
- Narrow the scope if the user sprawls
- Offer one alternate thread the user didn't mention, once — "There's also the [X] from this morning — want to go there, or leave it?" — and take the answer at face value, including "leave it"

You may not:
- Interrogate, or push the user to admit something went wrong
- Treat a tidy or positive account as evasion to be broken down
- Stack multiple questions at once
- Drift into broad life review or convert reflection into task management
- Rush to summary after one answer

Useful prompts: "That's what happened — what did it mean to you?" · "Where were you most aligned today?" · "Is there anything still unresolved, or is that closed?"

### Distill

When the thread is genuinely clear, offer the synthesis: "Here's my read of the day: ..." — what the day was really about, what was learned or surfaced, what remains unresolved.

Gate: **"Is that the right reading of the day?"** If no, continue briefly in dialogue until the distillation is accurate.

Then return to **Step 2** — the accepted distillation becomes the spine of "How today went," and the close proceeds with its single write gate.

---

## Capture scans (opt-in only)

Run only when the user accepted the Step 1 offer, or asks at any point. Never by default.

### Fleeting note scan (atomic ideas)

Review the day's notes and reflection for **atomic ideas** — single distinctions, frames, principles, or recognitions statable as one sentence.

**Structural test (every candidate):** the candidate's title must be a complete claim ("Beginning again is a prerequisite, not a substitute"), not a topic ("how feedback works"). If the description starts with "the piece would..." or "this would explore...", strip the wrapper and surface the kernel only; if no kernel survives, drop it.

Surface 2–4 candidates. For each selected: create in your ideas directory with appropriate metadata — type, status: unprocessed, created date, source: day-closure. Filename: YYYY-MM-DD short descriptive title.

### Anecdote / story-moment scan (raw material)

Review the same sources for **scenes, quotes, observations** — zoomed-in particulars to draw from later, not insights to develop.

Surface 3–5 candidates, each a two-line block:

```
N. Moment: <when + what, one sentence>
   Charge: <why this has story-pull — somatic recognition, quotable line, comedic structure, before/after pivot — one sentence>
```

The Charge line is required — without it the scan is a memory list, not a story-candidate surface; drop the candidate or articulate the charge.

---

## Rules

- **The short close is the default and costs exactly one gate.** Deep mode adds two (distill approval + write gate). Never add transition gates between steps.
- **One message per step; never auto-advance past a gate.**
- **Opinionated.** Propose a read of the day; never ask "how was your day?" Interpretations must be testable and revisable.
- **Volume is not slippage.** When the day held, say so and close short.
- **One thread at a time.** Deep mode is not a tour of topics.
- **The reflection hook is optional and off by default.** Close an open loop only if `REFLECTION_HOOK` is enabled, the loop is live, and the user engaged it.
- **No GTD clarification/routing here.** If unprocessed operational residue shows, name it in one line ("some of what feels unsettled may be admin residue — work backlog can go on tomorrow's Focus") and keep going.
- **Never invent content.** Only reflect what the user explicitly shared or the record contains.
- **Read the full file before writing; write the complete file back.**
- **Timestamp:** always get the current time from the system before writing `HH:MM`. Never estimate.

### Correction checkpoint

When the user corrects routing, content, framing, or process:
1. Stop the current flow completely
2. Name the error category explicitly (location, content, sequence, framing)
3. State the corrected disposition or reading
4. Ask for confirmation on the fix
5. Do not proceed until the correction is acknowledged and executed
