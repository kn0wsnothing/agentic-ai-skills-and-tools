# Nightly Distillation

A scheduled memory capture skill for AI agents. Reviews the day's sessions, extracts what matters, and writes it to persistent memory — so tomorrow's agent knows what happened today.

This is the **additive** half of a memory management pair. Distillation captures. The [Memory Dream Cycle](../memory-dream-cycle/) consolidates. One adds, the other subtracts.

---

## How It Works

The distillation runs 6 phases in sequence:

1. **Session Discovery** — find today's substantive agent sessions
2. **Episodic Capture** — write what happened to the daily log
3. **Knowledge Promotion** — route new facts to the right memory tier (people, projects, procedures)
4. **Session Log** — write a brief summary for tomorrow's morning review
5. **Working Memory Update** — add anything that affects daily context
6. **Daily Note** _(optional)_ — update an Obsidian/Logseq/markdown daily note

---

## Prerequisites

Your agent needs:

- **File read/write access**
- **Persistent storage** — memory files must survive between sessions
- **Session history access** — some way to review what happened in today's sessions (platform-specific — see [Session Discovery](#session-discovery))
- **A tiered memory structure** — same as the Memory Dream Cycle (see below)
- **A scheduling mechanism** — cron, Task Scheduler, platform-native scheduling, or manual trigger

---

## Memory Structure

Uses the same layout as the Memory Dream Cycle. If you've already set that up, you're good.

```
your-workspace/
├── MEMORY.md                  # Working memory — loaded every session
└── memory/
    ├── episodic/              # Daily logs — append-only
    │   └── YYYY-MM-DD.md
    ├── semantic/              # Long-term facts
    │   ├── people.md
    │   └── projects.md
    ├── procedural/            # How-to guides
    │   └── your-workflows.md
    └── staging/               # Temporary files for handoff between skills
        └── session-log-YYYY-MM-DD.md
```

---

## Session Discovery

This is the most platform-specific part of the skill. The distillation needs to know what happened today. How it discovers that depends on your setup:

| Setup | Discovery method |
|-------|-----------------|
| **Multi-session platform** (OpenClaw, custom orchestrator) | Use the session list API to find active sessions, pull history from each |
| **Single-session agent** (Claude Code, Cursor) | Review the current conversation history |
| **File-logged sessions** | Scan a log directory for today's files |
| **Git-based workflow** | Review today's commits and associated conversations |

The SKILL.md describes what to look for in abstract terms. The platform examples show how to implement discovery on each platform.

---

## Quick Start

1. **Set up the memory structure** (see above, or reuse from Memory Dream Cycle)
2. **Copy `SKILL.md`** into your agent's persistent instructions
3. **Configure the paths** in the Configuration table
4. **Run manually first** — trigger it and review what it captures
5. **Schedule it** — run in the evening, before the dream cycle

---

## Best Practices

**Run in the evening.** 9pm or end of workday. This ensures the full day is captured. Running too early misses afternoon work.

**Run before the dream cycle.** The dream cycle needs complete episodic data to make good consolidation decisions. Distillation first, dream cycle second.

**Review the first few runs.** Check that episodic entries are accurate (not inferred), promotions land in the right files, and working memory updates are appropriate.

**Don't over-promote.** Not everything needs to be in semantic memory. The distillation should capture the *important* things — key decisions, people facts, process changes. Routine work belongs in episodic only.

**Keep episodic entries concise.** One line per event. The episodic log is a reference, not a transcript. "Discussed migration plan with Mattia — decided on phased rollout" not a paragraph about what was discussed.

---

## Pair with the Dream Cycle

These two skills work best together:

```
Evening (9pm):  Distillation captures today → episodic + semantic + procedural
Overnight (3am): Dream cycle consolidates → lean working memory
Morning (8am):  Agent starts with clean, complete memory
```

You can run distillation alone (useful if you don't need consolidation yet), but the pair gives you both capture and maintenance.

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Cursor | [examples/cursor.md](examples/cursor.md) |
| Factory.ai | [examples/factory-ai.md](examples/factory-ai.md) |
| OpenAI Agents SDK | [examples/openai-agents-sdk.md](examples/openai-agents-sdk.md) |
| OpenCode | [examples/opencode.md](examples/opencode.md) |
| OpenClaw | [examples/openclaw.md](examples/openclaw.md) |
| Generic scheduling | [examples/cron-schedule.md](examples/cron-schedule.md) |

---

## FAQ

**How is this different from just saving chat logs?**
Chat logs are raw transcripts — huge, unstructured, and expensive to load. Distillation extracts the signal: 3-5 important events from a 2-hour session. It also routes knowledge to the right tier (people facts to semantic, workflows to procedural), so information is findable later.

**What if I only have one session per day?**
The skill still works — Phase 1 just finds one session instead of many. The value is still in the structured extraction and promotion, not in aggregating multiple sessions.

**Can I run this manually instead of on a schedule?**
Yes. Say "run nightly distillation" or equivalent in your agent session. Scheduling is recommended for consistency, but manual is fine — especially when starting out.

**What if distillation and the dream cycle run at the same time?**
Don't do this — they both write to working memory. Schedule them with at least a few hours gap. Distillation first, dream cycle second.

---

## Design Rationale

See [SPEC.md](SPEC.md) for the full design rationale, including the knowledge promotion model, why session discovery is abstracted, and how this pairs with the dream cycle.
