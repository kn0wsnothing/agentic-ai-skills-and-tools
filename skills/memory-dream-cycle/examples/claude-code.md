# Memory Dream Cycle — Claude Code

How to use the Memory Dream Cycle with [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

---

## Setup

### 1. Memory structure

Create the memory directory in your project root:

```bash
mkdir -p memory/{episodic,semantic,procedural}
touch MEMORY.md
```

### 2. Add to CLAUDE.md

Add a reference to the dream cycle in your project's `CLAUDE.md`:

```markdown
## Memory Management

This project uses persistent memory files. See `MEMORY.md` for current state.

### Memory Dream Cycle
When asked to run a dream cycle (or on a schedule), follow the instructions in
`skills/memory-dream-cycle/SKILL.md`. This consolidates memory by resolving
contradictions, pruning stale content, and keeping MEMORY.md lean (~200 lines).
```

### 3. Copy the skill

```bash
mkdir -p skills/memory-dream-cycle
cp /path/to/SKILL.md skills/memory-dream-cycle/
```

Or add this repo as a git submodule:

```bash
git submodule add https://github.com/kn0wsnothing/agentic-ai-skills-and-tools.git vendor/agentic-skills
```

Then reference `vendor/agentic-skills/skills/memory-dream-cycle/SKILL.md` in your CLAUDE.md.

### 4. Configure paths

Edit the Configuration table in SKILL.md to match your project layout. The defaults work if you're using the recommended structure above.

---

## Running

### Manual trigger

In a Claude Code session:

```
Run the memory dream cycle. Follow the instructions in skills/memory-dream-cycle/SKILL.md.
```

### Automated via Claude Code CLI

Use `claude` CLI with `--print` mode for non-interactive execution:

```bash
claude --print --model claude-opus-4-6 \
  "Run the memory dream cycle. Follow the instructions in skills/memory-dream-cycle/SKILL.md."
```

### Scheduled

Wrap the CLI command in a cron job or CI pipeline. See [cron-schedule.md](cron-schedule.md) for scheduling examples.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 5-phase consolidation pipeline |
| **Agent** | Claude (Opus recommended for reasoning quality) |
| **Tools** | Claude Code's built-in file read/write, shell access |

---

## Tips

- **Use Opus** for the dream cycle. Sonnet may miss subtle contradictions.
- **Run overnight.** Schedule it during off-hours so MEMORY.md is clean when you start your day.
- **Review the first 2-3 runs.** Check that demotion decisions make sense before fully trusting it.
- **Commit memory files.** If your project is in git, commit MEMORY.md and memory/ so you have version history as a safety net.
