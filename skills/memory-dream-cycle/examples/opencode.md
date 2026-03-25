# Memory Dream Cycle — OpenCode

How to use the Memory Dream Cycle with [OpenCode](https://github.com/opencode-ai/opencode).

---

## Setup

### 1. Memory structure

Create the memory directory in your project root:

```bash
mkdir -p memory/{episodic,semantic,procedural}
touch MEMORY.md
```

### 2. Add to your instructions

OpenCode reads project instructions from `AGENTS.md` (or similar configuration). Add a reference to the dream cycle:

```markdown
## Memory Management

This project uses persistent memory files. See `MEMORY.md` for current state.

### Memory Dream Cycle
When asked to run a dream cycle (or consolidate memory), follow the instructions
in `skills/memory-dream-cycle/SKILL.md`. This resolves contradictions, prunes
stale content, and keeps MEMORY.md lean (~200 lines).
```

### 3. Copy the skill

```bash
mkdir -p skills/memory-dream-cycle
cp /path/to/SKILL.md skills/memory-dream-cycle/
```

### 4. Configure paths

Edit the Configuration table in SKILL.md to match your project layout.

---

## Running

### Manual trigger

In an OpenCode session:

```
Run the memory dream cycle. Follow the instructions in skills/memory-dream-cycle/SKILL.md.
```

### Non-interactive mode

If OpenCode supports a non-interactive/CLI mode, you can automate:

```bash
opencode --message "Run the memory dream cycle following skills/memory-dream-cycle/SKILL.md."
```

Wrap this in a cron job for scheduling (see [cron-schedule.md](cron-schedule.md)).

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 5-phase consolidation pipeline |
| **Agent** | OpenCode's AI (model depends on your configuration) |
| **Tools** | OpenCode's built-in file read/write, shell access |

---

## Tips

- **Use the strongest model available.** Configure OpenCode to use a high-capability model for dream cycles. Contradiction detection requires strong reasoning.
- **Run manually first.** Trigger a few cycles interactively to review the output before automating.
- **Commit memory files.** If your project is in git, memory changes get version history — a nice safety net for reviewing what the dream cycle changed.
