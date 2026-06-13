# Breadcrumb — Design spec

Design decisions behind the Breadcrumb skill and why they're made the way they are.

---

## The problem

Sessions end. Context disappears. The agent did good work; you walked away. Tomorrow's session starts cold: the agent asks what it already answered, you re-explain what was decided, project notes don't reflect what shipped.

Breadcrumb closes this gap by making session-end writes a structured ritual rather than an afterthought. Not a backup or a transcript — a deliberate record of what completed, what moved, and what's worth keeping.

---

## Why a gate at step 5

The session log writes to `{logs}/` immediately — that's scratch, no gate needed. Everything else (daily note, project notes, memory, agent context) touches the canonical record.

The gate exists because the proposal step is where errors get caught. A six-step session might have resolved three tasks, updated two project notes, and generated one durable memory fact. Getting those wrong — ticking the wrong task, over-promoting an ephemeral detail — is harder to undo than to prevent.

The gate is also fast in practice. The proposal is paste-ready; APPLY takes one word.

---

## Why one bullet in the daily note

The daily note is a navigable index of the day, not a transcript. One bullet per session. If a session warrants more than one sentence, that detail goes in the session log file — which the breadcrumb links to.

The one-bullet constraint emerged from a real failure mode: agents that wrote multi-paragraph session_text to the daily note made the note illegible. The constraint is non-negotiable.

---

## Why topic-slugs (never numbers)

A logs directory full of `2026-04-17-claude-1.md`, `2026-04-17-claude-2.md` is unusable. What was session 1 about? You have to open it to know. A directory full of `2026-04-17-claude-schema-debug.md`, `2026-04-17-claude-newsletter-draft.md` is immediately navigable.

The slug is always derived from the session's topic or outcome, never from a counter. Sub-descriptors (`-morning`, `-followup`) handle same-topic collisions without reintroducing numbers.

---

## Why the task-state scan is mandatory

The most common way project notes drift out of sync is session-ending without updating them. A thinking or planning session that produces no file edits can still resolve tasks, shift priorities, and surface blockers — but if nothing was written, the project notes don't know.

The task-state scan runs unconditionally and says so explicitly if it finds nothing (`No task-state changes`). Silence would read as "skipped."

---

## Why "completed work only"

The session log is a historical record. `Next:` and `TODO:` lines belong in the task system. Once they appear in a session log, they generate noise on every future read: are these still open? Were they captured elsewhere? Was this the canonical list?

The forward-looking prohibition is clean and simple: if it happened, log it; if it needs to happen, route it to the task system.

---

## Why "user's POV" in the breadcrumb

The daily note is the user's narrative of their day. When an agent writes breadcrumbs in its own voice ("Coached the session", "Ran the analysis"), the daily note reads as a report from a third party rather than the user's own record.

Framing from the user's POV ("Drafted the proposal", "Reviewed the schema") keeps the daily note coherent as a personal record. The session log is where agent-POV belongs.

---

## The classification stamps (meta / personal)

The `meta: true` and `personal: true` frontmatter stamps exist to support allocation tracking: how much of the agent's work is building the system itself vs. doing the actual work the system supports?

`meta: true` requires an `unblocks:` claim — a concrete statement of what object-level work this system-build serves. This is written at build time, when the claim can be honest and specific. Vague claims ("improves the system") don't pass. Concrete claims ("breadcrumb now runs on hook so no sessions end without a log") do.

The stamps are optional and self-classifying — the agent adds them when the session fits, not on a checklist.

---

## What this isn't

Not a memory distillation skill. Breadcrumb logs the session and routes obvious promotions (project task-state, durable facts), but it doesn't do the deep episodic/semantic consolidation pass that a nightly distillation skill performs. If you want the full memory pipeline, pair breadcrumb with a consolidation skill that runs on a schedule.

Not a planning skill. Breadcrumb closes sessions, it doesn't open them. Morning review or a planning skill handles the forward-looking equivalent.

---

## Known limitations

- **Session discovery is self-referential.** Breadcrumb reviews the current conversation. For agents running multiple concurrent sessions, each session needs its own breadcrumb call.
- **The task-state scan depends on project note conventions.** If your project notes don't use a consistent task format, the scan can't classify reliably. Document your conventions.
- **No automated dedup.** If a session ends mid-way and you run breadcrumb twice, the second run may duplicate entries. Review before APPLY.
