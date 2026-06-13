# Multi-agent analysis retrospective

A structured four-pass retrospective protocol for multi-agent analysis runs. Converts one bad run into durable system improvements by reconstructing what actually happened, locating the root cause, classifying the right fix surface, and applying changes before the session ends.

## What this does

Runs four passes on the completed analysis run:
1. **Reconstruct** — spawns two log-scanner subagents in parallel to read the orchestrator and execution agent transcripts from disk; merges into a two-column timeline; shows it to the user before any interpretation
2. **Locate divergence** — finds the first place an assumption replaced verification; states one root cause before listing downstream effects
3. **Classify fix surface** — maps each finding to exactly one category: memory note, prompt edit, process/skill change, or architecture change
4. **Apply and log** — writes memory notes immediately; proposes prompt and process diffs as gated changes; logs architecture items for follow-up

## When to use it

- A verification step revealed incorrect queries or data errors
- The same correction was made more than once in the session
- The planning agent and execution agent diverged on what the task was
- A significant detour produced no usable output
- An assumption was treated as a verified fact

Don't auto-start. Confirm with the user first.

## How to use it

Invoke after completing (or abandoning) an analysis run. You need:
- The analysis brief, plan, and findings document paths
- The session IDs for both the orchestrator and execution agent sessions

The prompt handles the rest: it tells you how to delegate the transcript reading to subagents, how to merge the timelines, and how to classify and apply fixes.

Adapt the transcript-reading instructions to your specific agent stack — the prompt uses generic references to "orchestrator JSONL" and "execution agent database" rather than tool-specific commands.

## Key design decisions

**Disk-first reconstruction.** In-memory conversation may have been compacted or summarized. The transcript files on disk are the source of truth. The protocol enforces this regardless of whether the retrospective runs in the same session or a new one.

**Parallel subagent reads.** Full transcripts are long. The orchestrating model should never process raw transcripts — it delegates to two lightweight log-scanner subagents running in parallel, each returning a structured timeline of divergence-relevant events only.

**One root cause before downstream effects.** There is usually one upstream miss and several consequences. The protocol forces a single root-cause sentence before the effects list — it prevents the retrospective from becoming a ten-item blame spread with no actionable center.

**Fix-surface taxonomy is exclusive.** Each finding maps to exactly one surface: memory, prompt, process, or architecture. This prevents the common failure mode of writing a new memory note for something that needs a prompt rule, or adding a process step for a one-off mistake.

**Memory-existed-and-got-ignored is a distinct failure mode.** If the rule was already captured and still didn't fire, re-writing the memory is not the fix. The protocol requires diagnosing why retrieval failed.
