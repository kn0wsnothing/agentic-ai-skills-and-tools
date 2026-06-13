# Prose Scrub — Claude Code

How to wire up the prose-scrub skill in a Claude Code project.

---

## Setup

### 1. Copy the skill

```bash
mkdir -p .claude/skills/prose-scrub
cp /path/to/skills/prose-scrub/SKILL.md .claude/skills/prose-scrub/
```

Or symlink from a shared skills library:

```bash
ln -s /path/to/skills/prose-scrub/SKILL.md .claude/skills/prose-scrub/SKILL.md
```

### 2. Build or install a tell-scan

The skill requires a mechanical tell-scan — a small script or grep list that emits `file:line: [HIGH|flag] category: "match"` hits and exits non-zero if any HIGH-severity tell is present.

Minimum viable tell-scan: a bash script running grep patterns for the six killers. Add your own organization-specific patterns over time.

Place it at a path you control (e.g., `scripts/prose_tells_scan.sh`) and set `{tell_scan}` in your configuration to that path.

### 3. Configure paths in CLAUDE.md

Add a configuration block to your `CLAUDE.md`:

```markdown
## Prose scrub configuration

| Parameter | Value |
|-----------|-------|
| `{tell_scan}` | `scripts/prose_tells_scan.sh` |
| `{style_guide}` | `docs/voice-guide.md` |
| `{domain_vocab}` | `docs/canonical-vocabulary.md` |
| `{workspace}` | `/path/to/your/workspace` |
```

Adjust paths to match your project structure.

### 4. Add the trigger instruction

In `CLAUDE.md`:

```markdown
## Prose scrub

Invoke the prose scrub skill from `.claude/skills/prose-scrub/SKILL.md`:
- Automatically: as the final step of any analysis workflow before a deliverable is presented
- On demand: `/prose-scrub <file>` against any stakeholder-facing prose
```

---

## Running

### On demand

```
/prose-scrub path/to/draft.md
```

or

```
Run prose scrub on the insights doc before we present it.
```

### As the final step of an analysis workflow

In your analysis workflow skill, add prose scrub as the mandatory last step before presenting output to the user:

```markdown
## Final step — prose scrub

Before presenting any Tab 1 / executive summary / insights or recommendations doc as done,
run the prose scrub following `.claude/skills/prose-scrub/SKILL.md`. The deliverable is
not done until the re-scan returns zero HIGH-severity hits.
```

---

## Tell-scan scaffold

A minimal bash scaffold to start from:

```bash
#!/usr/bin/env bash
# prose_tells_scan.sh — mechanical tell-scan, minimum viable version
# Usage: ./prose_tells_scan.sh <file>
# Exits non-zero if any HIGH-severity tell found.

FILE="$1"
FOUND_HIGH=0

check() {
  local severity="$1" category="$2" pattern="$3"
  while IFS= read -r line; do
    echo "$line — [$severity] $category"
    [[ "$severity" == "HIGH" ]] && FOUND_HIGH=1
  done < <(grep -nEi "$pattern" "$FILE" 2>/dev/null | \
    sed "s/^/$FILE:/")
}

# Negation cadence — HIGH
check HIGH "negation-cadence" \
  ", not (just |merely |simply |only )?(a |an |the )?\w" 

# Negation cadence variants — HIGH
check HIGH "negation-cadence" \
  "\b(rather than|more than a|not (just|merely|simply))\b"

# Vague-observer attribution — HIGH
check HIGH "vague-observer" \
  "\b(it reads as|that looks like|appears to|seems to|reads as)\b"

# Importance-claim — HIGH
check HIGH "importance-claim" \
  "\b(this (matters|is (important|significant|notable|worth noting))|underscores|highlights)\b"

# Journalese metaphor — flag
check flag "journalese" \
  "\b(shrinking pie|left on the table|did the work|rising tide|drag the bucket)\b"

# Analyst shorthand — flag
check flag "analyst-voice" \
  "\b(the (frame|lever|cut|angle|engine|signal))\b"

exit $FOUND_HIGH
```

Extend with your own organization-specific patterns. Add format-checking patterns (`--review` mode) as your needs grow.

---

## WAT mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — genre gate, 4-step scrub pipeline |
| **Agent** | Claude (Opus required for editor subagent; orchestration on any model) |
| **Tools** | File read/write, shell access for tell-scan, subagent spawning |

---

## Tips

- **Build the tell-scan incrementally.** Start with the six core patterns. Add an organization-specific pattern each time a tell ships that the scan missed.
- **The reader card is worth the two minutes it takes.** Every time the editor pass returns a rewrite that's technically clean but registers-wrong, the reader card was thin. Invest in it.
- **Wire it as a mandatory final step.** The on-demand invocation catches what you remember to check. The mandatory-final-step pattern catches everything.
- **Approved rewrites go back to the canonical file.** The skill presents findings and a rewrite; you approve; it writes. The original draft is replaced in place — the history is in git.
