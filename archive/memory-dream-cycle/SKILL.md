# Memory Dream Cycle

You are running the Memory Dream Cycle — a scheduled memory consolidation process.

Your job: resolve contradictions, prune stale content, and keep working memory lean. You are the **subtractive** counterpart to whatever process captures daily events. Daily capture adds. You consolidate.

**Metaphor:** If daily memory capture is daytime note-taking, the dream cycle is REM sleep — consolidating, pruning, resolving, compressing.

---

## Memory Architecture

This skill assumes a tiered memory structure. Adapt the paths to your setup:

| Tier | Purpose | Default Path | Access |
|------|---------|-------------|--------|
| **Working memory** | Hot context loaded every session. Must stay lean. | `MEMORY.md` | Read/Write |
| **Semantic memory** | Long-term facts, people, decisions. Retrieved on demand. | `memory/semantic/*.md` | Read/Write |
| **Procedural memory** | Workflows, how-to guides, learned patterns. | `memory/procedural/*.md` | Read/Write |
| **Episodic memory** | Daily logs of what happened. Immutable record. | `memory/episodic/YYYY-MM-DD.md` | **Read-only** |

**Working memory** is the bottleneck. Every line costs context window budget in every session. The dream cycle's primary job is keeping it under a target size (default: ~200 lines).

**Semantic and procedural memory** are the demotion targets. Content pruned from working memory doesn't disappear — it moves here, where it's still searchable but doesn't consume context by default.

**Episodic memory** is the immutable ground truth. Never modified. Used for cross-referencing and contradiction checks.

---

## Configuration

Set these before running. The defaults work for most setups.

| Parameter | Default | Description |
|-----------|---------|-------------|
| `WORKING_MEMORY_PATH` | `MEMORY.md` | Path to your working memory file |
| `SEMANTIC_DIR` | `memory/semantic/` | Directory for semantic memory files |
| `PROCEDURAL_DIR` | `memory/procedural/` | Directory for procedural memory files |
| `EPISODIC_DIR` | `memory/episodic/` | Directory for episodic log files |
| `TARGET_LINES` | `200` | Target line count for working memory after consolidation |
| `LOOKBACK_DAYS` | `3` | How many days of episodic logs to read |
| `STALE_THRESHOLD_DAYS` | `14` | Entries older than this with no recent references get demoted |
| `SOURCE_OF_TRUTH_PATH` | _(optional)_ | External source of truth for project/task status (e.g., a task manager, project directory). Used for contradiction checks. |
| `ARCHIVE_FILE` | `memory/semantic/completed-projects.md` | Where completed project details go |
| `NOTIFICATION` | _(optional)_ | How to report results (platform-specific — see examples) |

---

## Phases

Run all 5 phases in order. Each phase builds on the previous one.

### Phase 1: Orientation (read-only)

Survey current memory state before making any changes.

1. Read working memory — count lines, note section structure
2. List all files in semantic and procedural directories
3. Read the last N days of episodic logs (see `LOOKBACK_DAYS`)
4. **Skip check:** If today's episodic file doesn't exist or is empty, skip the entire cycle. Nothing happened — nothing to consolidate. Report "Dream cycle skipped — no activity" and stop.
5. Note total memory footprint (file count, combined line count)

Output of this phase: a mental map of what exists and how it's organized. Don't write anything yet.

### Phase 2: Contradiction Scan

Compare working memory against recent episodic logs and (optionally) your source of truth. Look for:

- **Project status conflicts** — listed as "active" in working memory but completed/archived in your source of truth
- **People with outdated roles** — role or status changed in recent episodic entries but stale in working memory
- **System state drift** — tools listed as broken that have been fixed, configs that changed, temporary conditions that expired
- **Resolved decisions** — recorded as pending but actually decided (check episodic logs)
- **Passed events** — "upcoming" items with dates that have already passed

**Resolution rules:**
- Most recent episodic entry wins (it's closest to ground truth)
- External source of truth wins for projects/tasks (if configured)
- **When uncertain, don't guess.** Flag it for human review rather than auto-resolving. Add uncertain items to a `## Needs review` section at the bottom of working memory.

### Phase 3: Stale Pruning

Identify working memory content to demote or remove:

**Signals that content is stale:**
- Projects completed or archived (confirmed in Phase 2)
- Entries older than `STALE_THRESHOLD_DAYS` with no references in recent episodic logs
- Expired temporary information (promotions, one-time fixes, passed deadlines)
- References to files or paths that no longer exist (verify before removing)

**Demotion protocol (demote, don't delete):**
1. Read the target semantic file first (or create it if needed)
2. Append the demoted content with a date stamp header: `### Demoted YYYY-MM-DD`
3. Remove it from working memory

**Only delete outright** if content is truly obsolete — temporary debugging notes, superseded decisions, information that contradicts the current state with no historical value.

### Phase 4: Working Memory Consolidation

Rewrite working memory to hit the target line count.

**What stays (must-have for every session):**
- Identity/context section (who the user is, current state)
- Active people (with current roles only — no history)
- Active projects (name + one-line status + next milestone — no history, no completed items)
- Key decisions and rules (only things that affect daily behavior)
- System state (current config, known issues)

**What gets compressed or removed:**
- Inline history annotations ("updated on YYYY-MM-DD") — the semantic files preserve history
- Duplicate information across sections
- Detailed sub-bullets that belong in semantic/procedural files
- Work context entries already demoted in Phase 3

**Consolidation rules:**
- Preserve the existing section structure and heading format
- Merge duplicate entries across sections
- Convert relative dates to absolute dates
- Keep the file well-structured and scannable

**Write the complete working memory file.** Read it first, make all changes in memory, write the entire file back. Do not make incremental edits.

### Phase 5: Semantic File Maintenance

Light cleanup of files that received demoted content in Phase 3:

- Check for duplicate entries within each modified file
- Ensure newly demoted content has proper headings and date stamps
- Verify structure is clean (headings, organization — not raw dumps)

**Only touch files that were modified in this cycle.** Don't rewrite files that didn't change.

---

## Safety Constraints

These are non-negotiable:

1. **Episodic files are read-only.** Never modify daily logs. They are the immutable record.
2. **Source of truth is read-only.** Read it to check project/task status, never write to it.
3. **No task operations.** Never create, modify, or complete tasks. Memory consolidation only.
4. **No external actions.** No messages, no emails, no API calls beyond file operations.
5. **Demote, don't delete.** When removing content from working memory, move it to semantic files first. Only delete truly obsolete information.
6. **Flag uncertainty.** If a contradiction can't be confidently resolved, add it to `## Needs review` in working memory. Let the human decide.

---

## Output

After completing all phases, report a summary.

**Clean cycle (minor or no changes):**
```
🌙 Dream cycle — working memory: [X] → [Y] lines. No issues.
```

**Substantive cycle:**
```
🌙 Dream Cycle — YYYY-MM-DD

Summary:
- Working memory: [X] → [Y] lines ([Z] entries pruned, [N] demoted)
- Contradictions resolved: [list or "none"]
- Stale entries pruned: [list or "none"]
- Flagged for review: [list or "none"]
- Semantic files updated: [list]
```

---

## Scheduling

This skill is designed to run daily, ideally during off-hours (overnight, early morning). It should run **after** your daily memory capture/distillation process, so it's working with a complete picture of the day.

Recommended timing:
- If you have a nightly capture job at 9pm → run the dream cycle at 3am
- If you capture manually → run the dream cycle before your first session each day
- At minimum → run weekly

See `examples/cron-schedule.md` for platform-specific scheduling instructions.
