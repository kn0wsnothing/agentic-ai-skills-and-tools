# Memory Dream Cycle — Cursor

How to use the Memory Dream Cycle with [Cursor](https://cursor.com).

---

## Setup

### 1. Memory structure

Create the memory directory in your project root:

```bash
mkdir -p memory/{episodic,semantic,procedural}
touch MEMORY.md
```

### 2. Add to Cursor Rules

In your project's `.cursor/rules` directory (or `.cursorrules` file), add a reference to the dream cycle:

```markdown
## Memory Management

This project uses persistent memory files. See `MEMORY.md` for current state.

### Memory Dream Cycle
When asked to run a dream cycle, follow the instructions in
`skills/memory-dream-cycle/SKILL.md`. This consolidates memory by resolving
contradictions, pruning stale content, and keeping MEMORY.md lean (~200 lines).
```

If using Cursor's newer project rules (`.cursor/rules/`), create a dedicated rule file:

```bash
mkdir -p .cursor/rules
cat > .cursor/rules/memory-dream-cycle.md << 'EOF'
---
description: Memory consolidation — run when asked to "dream cycle" or "consolidate memory"
globs: ["MEMORY.md", "memory/**"]
---

Follow the instructions in `skills/memory-dream-cycle/SKILL.md` to run the
memory dream cycle. This resolves contradictions, prunes stale content, and
keeps MEMORY.md lean.
EOF
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

In Cursor's chat or Composer:

```
Run the memory dream cycle. Follow skills/memory-dream-cycle/SKILL.md.
```

### Agent mode

In Cursor's Agent mode, the skill can be triggered contextually. If you've set up the rule file with the right description, Cursor may auto-apply it when you're working with memory files.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 5-phase consolidation pipeline |
| **Agent** | Cursor's AI (Claude or GPT, depending on your settings) |
| **Tools** | Cursor's built-in file read/write, terminal access |

---

## Tips

- **Use the best model available.** If Cursor offers Claude Opus or GPT-4 class, use that for dream cycles. Smaller models miss contradictions.
- **Trigger manually first.** Run it a few times interactively before trying to automate.
- **Cursor doesn't have native scheduling.** For automated runs, wrap a Claude Code CLI call in a cron job (see [cron-schedule.md](cron-schedule.md)) or trigger via a CI pipeline.
