# Memory Dream Cycle

A nightly memory consolidation skill for AI agents with persistent memory. Inspired by how REM sleep consolidates human memory — the agent scans its memory files for contradictions, staleness, and bloat, then consolidates.

Every agent system with persistent memory has this problem: memory files grow, accumulate contradictions, and waste context window budget on stale information. The dream cycle is the subtractive counterpart to daily memory capture.

---

## How It Works

The dream cycle runs 5 phases in sequence:

1. **Orientation** — survey the current memory state (read-only)
2. **Contradiction Scan** — cross-reference memory tiers for conflicts
3. **Stale Pruning** — identify and demote outdated content to long-term storage
4. **Consolidation** — rewrite working memory to a target size (~200 lines)
5. **Semantic Maintenance** — clean up files that received demoted content

It's designed to run daily (overnight), after whatever process captures the day's events.

---

## Prerequisites

Your agent needs:

- **File read/write access** — the skill reads and writes memory files
- **Persistent storage** — memory files must survive between sessions
- **A tiered memory structure** — at minimum, a working memory file and a directory for long-term storage (see [Memory Structure](#memory-structure) below)
- **A scheduling mechanism** — cron, Task Scheduler, platform-native scheduling, or manual trigger
- **A capable model** — this requires strong reasoning (Claude Opus, GPT-4 class, or equivalent). Smaller models may miss contradictions.

---

## Memory Structure

The skill assumes a tiered memory layout. You probably already have something like this if you're using persistent memory. If not, here's the recommended structure:

```
your-workspace/
├── MEMORY.md                  # Working memory — loaded every session
└── memory/
    ├── episodic/              # Daily logs — append-only, never modified
    │   ├── 2026-03-23.md
    │   ├── 2026-03-24.md
    │   └── 2026-03-25.md
    ├── semantic/              # Long-term facts — people, projects, decisions
    │   ├── people.md
    │   ├── work-context.md
    │   └── completed-projects.md
    └── procedural/            # How-to guides — workflows, patterns
        ├── deployment.md
        └── data-pipeline.md
```

**Working memory** (`MEMORY.md`) is the critical file. It's loaded every session and should stay under ~200 lines. Everything else is retrieved on demand.

**Episodic logs** are the immutable record. The dream cycle reads them but never modifies them.

If you don't have this structure yet, create it. The dream cycle will help maintain it.

---

## Quick Start

The fastest path to a working dream cycle:

1. **Set up your memory structure** (see above)
2. **Copy `SKILL.md`** into your agent's persistent instructions
3. **Configure the paths** — update the Configuration table in SKILL.md to match your file layout
4. **Run it manually first** — trigger the skill and review the output before scheduling it
5. **Schedule it** — see [examples/cron-schedule.md](examples/cron-schedule.md) for platform-specific instructions

Start with a manual run. Review what it does. Then automate.

---

## Configuration

All configuration is done by editing the parameters table at the top of SKILL.md. Key settings:

| Parameter | What to Set |
|-----------|------------|
| `WORKING_MEMORY_PATH` | Path to your main memory file |
| `TARGET_LINES` | How lean you want working memory (default: 200) |
| `SOURCE_OF_TRUTH_PATH` | Where the dream cycle checks project/task status (optional but recommended) |
| `STALE_THRESHOLD_DAYS` | How old an entry must be before it's considered stale (default: 14) |

See SKILL.md for the full configuration table.

---

## Best Practices

**Start with manual runs.** Run the dream cycle manually for the first 2-3 cycles. Review the output. Make sure it's making good decisions before you automate it.

**Review the `## Needs review` section.** The dream cycle flags contradictions it can't resolve. Check this section after each run and resolve the flags. Over time, you'll see fewer flags as your memory gets cleaner.

**Run after daily capture.** The dream cycle should run *after* whatever process captures the day's events. It needs a complete picture of the day to make good consolidation decisions.

**Adjust the target line count.** 200 lines is a starting point. If your agent frequently lacks context, increase it. If sessions feel slow or context-heavy, decrease it. The right number depends on your model's context window and how many active projects you have.

**Don't skip the source of truth.** If you have a task manager, project tracker, or notes system, configure `SOURCE_OF_TRUTH_PATH`. The dream cycle's contradiction resolution is significantly better when it can cross-reference against an external source.

**Check the semantic files.** After the first few runs, review your semantic memory files to make sure demoted content landed correctly. The dream cycle should be organizing, not dumping.

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

**How is this different from just rewriting my memory file manually?**
You could do that. The dream cycle does it systematically — checking for contradictions you'd miss, demoting rather than deleting (so nothing is lost), and doing it consistently on a schedule. Manual memory maintenance doesn't scale.

**Can I run just some of the phases?**
Yes, but they're ordered for a reason (see [SPEC.md](SPEC.md)). If you only have time for one phase, Phase 4 (Consolidation) gives you the most value. But skipping the contradiction scan means you might be consolidating incorrect information.

**What if the dream cycle makes a mistake?**
Demoted content still exists in semantic files — nothing is permanently lost. If it incorrectly removes something from working memory, add it back. If it flags something unnecessarily, resolve the flag. The system is designed to be conservative.

**How long does a cycle take?**
Typically 2-5 minutes of agent time, depending on memory size and model speed. It's doing a lot of reading and cross-referencing, but the actual writes are small.

**Does this work with multiple agents sharing memory?**
Not directly. The dream cycle operates on one agent's memory. For shared memory, you'd need a coordination layer to prevent concurrent writes. This is a known limitation.

---

## Design Rationale

See [SPEC.md](SPEC.md) for the full design rationale, including why we chose tiered memory, why demotion over deletion, why episodic immutability, and why the phases are ordered the way they are.
