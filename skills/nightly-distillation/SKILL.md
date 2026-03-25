# Nightly Distillation

You are running the Nightly Distillation — a scheduled memory capture and promotion process.

Your job: review today's agent sessions, capture what happened to episodic memory, and promote important knowledge to the right memory tier. You are the **additive** counterpart to the Memory Dream Cycle (which consolidates and prunes).

**Metaphor:** If the dream cycle is REM sleep, nightly distillation is the last hour of wakefulness — writing down what happened today before you forget.

---

## Memory Architecture

This skill uses the same tiered memory structure as the Memory Dream Cycle:

| Tier | Purpose | Default Path | Access |
|------|---------|-------------|--------|
| **Working memory** | Hot context loaded every session. | `MEMORY.md` | Read/Write |
| **Semantic memory** | Long-term facts — people, decisions, project state. | `memory/semantic/*.md` | Read/Write |
| **Procedural memory** | Workflows, how-to guides, learned patterns. | `memory/procedural/*.md` | Read/Write |
| **Episodic memory** | Daily logs of what happened. | `memory/episodic/YYYY-MM-DD.md` | **Append-only** |

Additionally, this skill may write to a **daily note** if you maintain one (e.g., in Obsidian, Logseq, or plain markdown).

---

## Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| `WORKING_MEMORY_PATH` | `MEMORY.md` | Path to your working memory file |
| `SEMANTIC_DIR` | `memory/semantic/` | Directory for semantic memory files |
| `PROCEDURAL_DIR` | `memory/procedural/` | Directory for procedural memory files |
| `EPISODIC_DIR` | `memory/episodic/` | Directory for episodic log files |
| `DAILY_NOTE_PATH` | _(optional)_ | Path pattern for daily notes (e.g., `notes/daily/YYYY-MM-DD.md`) |
| `SESSION_LOG_PATH` | `memory/staging/session-log-YYYY-MM-DD.md` | Where to write the session summary for morning review |
| `SOURCE_OF_TRUTH_PATH` | _(optional)_ | External project/task system for cross-referencing |
| `PEOPLE_DIR` | _(optional)_ | Directory for people/contact notes |
| `LOOKBACK_HOURS` | `24` | How far back to look for session activity |
| `NOTIFICATION` | _(optional)_ | How to report results (platform-specific) |

---

## Phases

Run all phases in order.

### Phase 1: Session Discovery

Find all agent sessions from today that contain substantive content.

1. List sessions active in the last `LOOKBACK_HOURS` hours
2. Filter to substantive sessions — skip:
   - Heartbeat-only sessions (automated health checks)
   - Cron/scheduled job sessions (they have their own logging)
   - Sessions with no meaningful content
3. For each substantive session, pull recent message history

**How you discover sessions depends on your platform:**
- If your platform has a session list API → use it
- If sessions are logged to files → scan the log directory
- If you're a single-session agent → review your own conversation history

The goal is: what happened today that's worth remembering?

### Phase 2: Episodic Capture

Update today's episodic log with events from discovered sessions.

1. Read today's episodic file (create if it doesn't exist): `EPISODIC_DIR/YYYY-MM-DD.md`
2. Compare session content against what's already captured
3. Append any **missing substantive events** — decisions, conversations, tasks completed, problems solved, insights surfaced

**Episodic entry format:**
```
- HH:MM — Description of what happened [source: session-name]
```

**Rules:**
- Only record what actually happened. Do not infer or reconstruct events.
- Include a source tag so entries are traceable back to their origin.
- Keep entries concise — one line per event, not a transcript.
- If a location is relevant, only state it if explicitly mentioned in the session.

### Phase 3: Knowledge Promotion

Scan today's sessions and episodic entries for knowledge that should be promoted to long-term memory. This is where new facts get routed to the right tier.

**What to promote:**

#### a. People facts → Semantic memory
New information about people — role changes, preferences, relationships, contact details.
- Append to the relevant person file in `SEMANTIC_DIR` or `PEOPLE_DIR`
- Create a new person file if this is a first mention
- Format: plain prose with date — e.g., "Started new role as team lead (2026-03-25)"

#### b. Project decisions → Semantic memory or source of truth
Significant project decisions, status changes, or milestones.
- If you have a `SOURCE_OF_TRUTH_PATH`, update the relevant project note
- Otherwise, append to a semantic file (e.g., `memory/semantic/projects.md`)
- Only promote significant decisions — not every conversation about a project

#### c. Procedural knowledge → Procedural memory
New workflows, corrections to existing processes, learned patterns, tool usage notes.
- Create or update dedicated files in `PROCEDURAL_DIR`
- One file per topic (e.g., `memory/procedural/deployment-process.md`)
- Procedural knowledge should be written as instructions, not narratives

#### d. Area-level insights → Semantic memory
Cross-cutting insights that don't belong to a single project.
- Append to the relevant area or domain file in `SEMANTIC_DIR`
- Only genuinely significant insights — err on the side of skipping

**Promotion rules:**
- Read the target file before writing (don't duplicate existing content)
- Include a date stamp on all new entries
- When referencing other notes, use whatever linking convention your system supports

### Phase 4: Session Log

Write a concise session summary for use by morning review or daily planning.

Create `SESSION_LOG_PATH` with one line per substantive session:

```
- session-name — Brief topic summary
- another-session — What was discussed/decided
```

This is not a detailed log — it's a quick reference so tomorrow's planning knows what happened today. Keep it to one line per session.

### Phase 5: Working Memory Update

Update working memory with any changes that affect daily context.

1. Read the full current working memory file
2. Update only sections that need it:
   - Active projects: update status if decisions were made
   - People: add new people or update roles
   - System state: note any config changes or new issues
   - Key decisions: add new rules that affect daily behavior
3. Write the complete file back

**Rules:**
- Keep working memory lean — add only what's needed in every session
- Remove anything that's now outdated (the dream cycle will catch what you miss)
- Do not grow working memory unboundedly — if you're adding, consider what to compress

### Phase 6: Daily Note (optional)

If `DAILY_NOTE_PATH` is configured, update the daily note:

1. Read today's daily note (or create from template if it doesn't exist)
2. Append a journal section with timestamped entries from today's sessions
3. Add any significant decisions to a decisions section
4. Do not overwrite existing content — append only

**If you don't use daily notes, skip this phase entirely.**

---

## Safety Constraints

1. **Only record what happened.** Never infer, reconstruct, or embellish events. If it wasn't in the session, it doesn't go in the log.
2. **No task operations.** Never create, modify, or complete tasks. This is memory capture, not task management.
3. **No external actions.** No messages, emails, or API calls beyond file operations.
4. **Read before writing.** Always read a file before modifying it to avoid overwriting content.
5. **Write the complete file.** When updating a file, read it fully, make changes, and write the entire file back. Don't make partial edits that could corrupt content.
6. **Don't duplicate.** Before appending to any file, check if the content is already there.

---

## Output

After completing all phases, report a summary.

**Quiet day:**
```
📋 Nightly distillation — [N] sessions processed, [M] episodic entries added. No promotions.
```

**Active day:**
```
📋 Nightly Distillation — YYYY-MM-DD

Summary:
- Sessions processed: [N]
- Episodic entries added: [M]
- Knowledge promoted:
  - People: [list or "none"]
  - Project decisions: [list or "none"]
  - Procedural: [list or "none"]
  - Insights: [list or "none"]
- Working memory: [what changed or "no changes"]
- Session log: written for morning review
```

---

## Scheduling

Run daily in the evening, after the day's work is done but before the dream cycle.

Recommended timing:
- **Distillation at 9pm** → Dream cycle at 3am (gives buffer for late sessions)
- Or: **Distillation at end of workday** → Dream cycle overnight
- At minimum: run before the dream cycle, so it has complete data to consolidate

See `examples/cron-schedule.md` for platform-specific scheduling instructions.

---

## Relationship to the Dream Cycle

These two skills are designed as a pair:

| | Nightly Distillation | Memory Dream Cycle |
|---|---|---|
| **Direction** | Additive — captures new information | Subtractive — consolidates and prunes |
| **Timing** | Evening (end of day) | Overnight (after distillation) |
| **Primary target** | Episodic + semantic memory | Working memory |
| **Goal** | Nothing from today is lost | Nothing stale persists |

You can run either skill independently, but they work best together. Distillation ensures the day is captured. The dream cycle ensures memory stays lean. One adds, the other subtracts.
