# Nightly Distillation — Design Spec

This document explains the design decisions behind the Nightly Distillation skill.

---

## The Problem

AI agents have sessions. Sessions end. Without active capture, what happened in a session exists only in the session's message history — which may be hard to search, may not persist, and is never loaded into future sessions by default.

This creates a predictable failure mode: the agent does good work, the session ends, and the next session starts with no memory of what happened. The human has to re-explain context. The agent asks questions it already answered yesterday.

Nightly distillation solves this by systematically extracting the important parts of today's sessions and writing them to persistent, structured memory files.

---

## Why "Distillation"

The word is deliberate. This isn't a backup or a transcript. It's a *distillation* — extracting the essence and discarding the noise. A 2-hour conversation might produce 3 episodic entries and 1 semantic promotion. That's correct. Most of what happens in a session is intermediate reasoning, tool output, and back-and-forth that doesn't need to persist.

The skill's job is editorial judgment: what from today is worth remembering?

---

## The Knowledge Promotion Model

Not all information has the same shelf life or access pattern. The distillation skill routes new knowledge to the appropriate tier:

### Episodic (what happened)
- Timestamped events
- Decisions made
- Problems encountered and solved
- Conversations held

Episodic memory answers: "What happened on March 25th?" It's the raw chronological record.

### Semantic (what's true)
- People facts (roles, relationships, preferences)
- Project state (status, decisions, milestones)
- Domain knowledge (learned facts about the user's work, tools, environment)

Semantic memory answers: "Who is this person?" or "What's the status of this project?" It's accumulated knowledge, not tied to a specific day.

### Procedural (how to do things)
- Workflows and processes
- Tool usage patterns
- Corrections to existing procedures
- Workarounds and fixes

Procedural memory answers: "How do I deploy this?" or "What's the right BigQuery syntax for this?" It's instructions, not facts.

### Working memory (what matters right now)
- Current project status
- Active people and their roles
- System state
- Rules that affect daily behavior

Working memory is the most expensive tier — it's loaded every session. Only information that's needed in *every* context belongs here.

**The distillation skill's core judgment call:** which tier does each piece of new information belong to?

---

## Why Session Discovery Matters

The distillation skill needs to find and review the day's sessions. This is the most platform-specific part of the skill — every agent framework handles sessions differently.

The SKILL.md intentionally abstracts this: "List sessions active in the last N hours." How you do that depends on your platform:

- **Multi-session platforms** (OpenClaw, custom orchestrators): use the session list API to find all sessions, then pull history from each
- **Single-session agents** (Claude Code, Cursor): review the current conversation history
- **Logged agents** (custom setups): scan log files for today's activity

The abstraction is important because the *rest* of the skill (episodic capture, knowledge promotion, working memory update) is identical regardless of how you discovered the sessions.

---

## Why "Only Record What Happened"

The strongest constraint in the skill: never infer, reconstruct, or embellish events. If it wasn't explicitly in the session, it doesn't go in the log.

This seems obvious but it's a real failure mode. Models are good at pattern-matching and will confidently state things like "John had a meeting with Shea about the project" based on partial signals when no such meeting occurred. Once that's in episodic memory, it becomes a "fact" that future sessions reference.

The constraint is absolute: no inference, no "probably," no filling in gaps. If you're not sure something happened, don't record it.

---

## Why Evening Timing

The distillation runs in the evening for practical reasons:

1. **Complete picture.** Running at 9pm means most of the day's work is captured. Running at 3pm would miss afternoon sessions.
2. **Buffer for the dream cycle.** The dream cycle runs overnight and needs complete episodic data. Running distillation first ensures it has the full picture.
3. **No contention.** Running during off-hours means the agent isn't competing with active sessions for context or file access.

The timing is a recommendation, not a requirement. If your workflow is different, adjust accordingly. The key constraint is: distillation runs *before* the dream cycle.

---

## Session Log: The Bridge to Morning

The session log (`memory/staging/session-log-YYYY-MM-DD.md`) is a small but important output. It's a one-liner-per-session summary designed for one consumer: tomorrow's morning review.

The morning review needs to know "what happened yesterday" to plan today effectively. Reading full episodic logs is too much. The session log is the right granularity — just enough to orient, not enough to overwhelm.

---

## Design Tradeoffs

### Thoroughness vs. speed
The skill reads potentially many sessions and cross-references multiple files. This takes time. We optimize for completeness over speed — a 5-minute distillation that catches everything is better than a 30-second one that misses a key decision.

### Promotion aggressiveness
The skill is moderately aggressive about promotion — if something looks like a people fact or project decision, it promotes it. The dream cycle will catch over-promotion (if working memory gets bloated). Under-promotion (missing something important) is harder to recover from.

### Working memory writes
The skill updates working memory conservatively. It adds new information but doesn't do major restructuring — that's the dream cycle's job. Distillation adds; dream cycle consolidates.

---

## Integration with Memory Dream Cycle

These skills are designed as a complementary pair:

```
Day's sessions
    ↓
[Nightly Distillation] — 9pm
    ↓ captures events, promotes knowledge
Episodic + Semantic + Procedural + Working memory
    ↓
[Memory Dream Cycle] — 3am
    ↓ resolves contradictions, prunes stale content
Clean working memory for tomorrow
    ↓
[Morning Review] — 8am (optional)
    ↓ reads session log + clean memory
Today's plan
```

The pipeline works end-to-end: capture → consolidate → plan. Each step consumes the output of the previous one.

---

## Known Limitations

- **Session discovery is platform-specific.** The skill can't fully abstract how you find and read sessions. The examples help bridge this gap.
- **No duplicate detection across days.** If the same event spans two days (late-night session), it might get captured in both days' episodic logs. This is acceptable — the dream cycle handles dedup.
- **Promotion quality depends on model quality.** A weaker model may miss important facts or over-promote trivial details. Use a strong model.
- **No verification.** The skill trusts its own output. Review the summaries, especially in the first few runs.
