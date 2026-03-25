# Memory Dream Cycle — OpenClaw

How to use the Memory Dream Cycle with [OpenClaw](https://github.com/openclaw/openclaw). This is where the skill was built and battle-tested.

---

## Setup

### 1. Memory structure

OpenClaw's default workspace already supports this layout:

```
~/.openclaw/workspace/
├── MEMORY.md
└── memory/
    ├── episodic/
    ├── semantic/
    └── procedural/
```

If you don't have this structure yet, create it:

```bash
mkdir -p ~/.openclaw/workspace/memory/{episodic,semantic,procedural}
touch ~/.openclaw/workspace/MEMORY.md
```

### 2. Create the cron job

The dream cycle runs as an isolated cron job. The entire skill is the prompt — no scripts needed.

Read the contents of `SKILL.md` and use it as the cron job's message payload. You can create the job via the OpenClaw CLI or the cron tool:

```bash
openclaw cron add \
  --name "memory-dream-cycle" \
  --schedule "0 3 * * *" \
  --tz "Your/Timezone" \
  --session-target isolated \
  --model "anthropic/claude-opus-4-6" \
  --timeout 300 \
  --message "$(cat skills/memory-dream-cycle/SKILL.md)"
```

Or via the agent's cron tool in a session — paste the SKILL.md content as the `message` field in an `agentTurn` payload.

### 3. Configure paths

Edit the SKILL.md Configuration table before creating the cron job:

| Parameter | OpenClaw Default |
|-----------|-----------------|
| `WORKING_MEMORY_PATH` | `MEMORY.md` (relative to workspace) |
| `SEMANTIC_DIR` | `memory/semantic/` |
| `PROCEDURAL_DIR` | `memory/procedural/` |
| `EPISODIC_DIR` | `memory/episodic/` |
| `SOURCE_OF_TRUTH_PATH` | Your vault or project directory (optional) |

### 4. Configure delivery

To get a summary posted to a Discord channel (or other messaging surface):

```
delivery:
  mode: announce
  channel: discord
  to: YOUR_CHANNEL_ID
```

---

## Running

### Manual trigger

In any OpenClaw session:

```
Run the memory dream cycle. Follow skills/memory-dream-cycle/SKILL.md.
```

Or say "dream" / "consolidate memory" if you've wired up a trigger.

### Via cron

Once the cron job is created, it runs automatically on schedule. Check status:

```bash
openclaw cron list
```

Force a run:

```bash
openclaw cron run --id YOUR_JOB_ID
```

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | SKILL.md content → cron job prompt (the prompt IS the workflow) |
| **Agent** | Isolated Opus session (spawned by cron) |
| **Tools** | OpenClaw's standard agent tools — file read/write, exec, memory_search |

---

## How It Works in Production

This is how we actually run it:

- **Schedule:** Daily at 3:00 AM HKT
- **Model:** Claude Opus (strong reasoning for contradiction detection)
- **Timeout:** 300 seconds (typical run: 2-4 minutes)
- **Delivery:** Summary posted to #general on Discord
- **Relationship:** Runs after nightly distillation (9pm) — distillation adds today's events, dream cycle consolidates overnight

The cron prompt includes the full SKILL.md content plus our specific configuration (file paths, vault cross-referencing). The isolated session starts fresh, reads memory, runs all 5 phases, writes the changes, and posts a summary.

No scripts. No code. Just a prompt and an agent.

---

## Tips

- **Use Opus.** The dream cycle requires strong reasoning. Sonnet misses contradictions.
- **Set a reasonable timeout.** 300 seconds is plenty for daily cycles. Increase if your memory is large.
- **Pair with nightly distillation.** Distillation captures. Dream cycle consolidates. They're complementary.
- **Check the output.** The first few cycles, read the full summary. Once you trust it, skim for flags in `## Needs review`.
