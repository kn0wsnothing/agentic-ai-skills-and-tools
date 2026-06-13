# Multi-agent analysis retrospective

This prompt governs retrospectives at the end of an analysis run, or after a verification loop reveals problems. The purpose is to convert one bad run into durable system improvements before too much time passes.

Works in all three session states — same session, post-compaction, or new session. Reconstruction is always disk-first (see below) so context rot doesn't degrade the retrospective. Same-session is still preferable when feasible because the user's memory of the run is fresh, but it is no longer required.

## When to invoke

User triggers explicitly ("let's do the retrospective", "retro this run") OR the orchestrator proactively suggests when:

- Verification reveals data or query correctness issues
- The same lesson got re-taught more than once in the session
- A significant detour consumed real time without producing usable output
- An assumption was made that should have been verified
- The planning agent and the execution agent diverged on what the work was supposed to be

Never auto-start. Confirm before beginning.

## Reconstruction is disk-first

Both agents persist their full transcripts to disk. Use those, not in-memory conversation, regardless of session state.

- **Orchestrator transcripts**: your orchestrating agent's JSONL session files. Survives compaction and new sessions — the JSONL holds every turn before any in-context summarization.
- **Execution agent transcripts**: your execution agent's session database or log files.

If the retrospective runs in the same session as the analysis, you may already have the orchestrator side in context — but pull from disk anyway. The JSONL is the source of truth; the in-memory conversation may have been compacted or summarized.

## Inputs to gather

Before starting, identify:

- The analysis run's brief, plan, and findings (document paths)
- The relevant orchestrator session(s) — by date, project mention, or recency
- The relevant execution agent session(s) — likely under your SQL or analysis workspace
- Existing relevant memories or lessons-learned notes (search by topic before classifying — a finding that duplicates an existing note is itself a finding)

Confirm the session IDs with the user before pulling transcripts. Do not guess.

## Delegate the bulk reading to subagents

Pass 1 generates significant token traffic — full transcripts can be 1000+ lines each. Do not let the orchestrating model process raw transcripts. Spawn two subagents in parallel:

- **Log-scanner subagent for orchestrator side**: reads the orchestrator transcript, returns a structured timeline of decisions, assumptions, user corrections, and tool calls relevant to the analysis run. Tight markdown only.
- **Log-scanner subagent for execution side**: reads the execution agent transcript, returns a structured timeline of queries executed, results returned, claims made about the data, and any verification flags raised.

Each subagent gets explicit instructions: identify the divergence-relevant events (assumption stated as fact, missing verification, user reset, contradicted result), don't summarize the whole session. Pass each subagent the brief / plan / findings paths so it knows what "on track" looks like.

The orchestrator merges the two summaries, presents the unified timeline to the user, and runs Passes 2–4. Those passes (locate divergence, classify fix surface, write memories and diffs) need judgment and stay with the orchestrating model.

## Four passes

### Pass 1 — Reconstruct

Spawn the two log-scanner subagents in parallel as described above. Each returns a side-specific timeline of divergence-relevant events.

Once both return, merge into a single two-column timeline (orchestrator actions / execution agent actions) with timestamps. Mark gaps with `[?]` and ask the user to fill them — especially terminal interactions the execution agent didn't capture.

Show the merged timeline to the user before moving to Pass 2. Observable only — no interpretation yet.

Optional third source: list executed artifacts (SQL files, query outputs, plan documents) from the execution workspace when the transcript references files but doesn't show contents.

### Pass 2 — Locate divergence

Identify the first place an assumption replaced verification. There is usually one upstream miss and several downstream consequences. Don't list ten findings if there's one root cause. State the root cause as a single sentence before listing downstream effects.

### Pass 3 — Classify fix surface

For each finding, map to exactly one of:

- **Memory** — a lessons-learned note or auto-memory entry. Use when the lesson is "don't do X" with no process change needed.
- **Prompt** — an edit to a system prompt or instructions document would have prevented it. Use when a missing rule in the prompt is the gap.
- **Skill / process** — a workflow step is missing from an existing process, or a new process is warranted. Use when the fix requires more than a memory or rule.
- **Architecture** — the current orchestrator/executor split cannot fix it without restructuring (tool routing, agent boundaries, MCP setup). Use sparingly.

Distinct failure mode worth flagging separately:

- **Memory-existed-and-got-ignored.** The rule was already captured but didn't fire. Don't just re-write the note. Diagnose why it didn't surface — wording, indexing, retrieval, or the agent that needed it didn't read it.

### Pass 4 — Apply and log

- Apply memory writes immediately. Use your system's auto-memory format for ongoing rules; use your episodic store for the run itself if significant.
- Propose prompt and process diffs as gated writes. Show the diff, wait for go/no-go.
- Log architecture items in your project notes, tagged for follow-up.

## Distinctions to make

- **Bad query output** vs **bad orchestrator instructions** vs **bad user spec** vs **honest ambiguity**. Don't conflate. Execution-layer issues often trace upstream to an orchestrator instruction that didn't constrain enough — fix the constraint, not the query.
- **Failure to verify** vs **failure to know**. The first is a process miss; the second is an information gap. They have different fix surfaces.
- **One-off** vs **pattern**. A pattern (3+ similar incidents across runs) warrants a stronger fix surface — usually prompt or process, not just another memory note.

## Output

Close with a structured summary written to your session log:

- The run and where it diverged (one paragraph)
- Findings table: finding / fix surface / action taken or deferred
- Changes applied this session (memories written, prompt diffs proposed, process edits)
- Items deferred (architecture, larger structural changes) with project routing
- Anything that should be re-checked next run

Add a breadcrumb to today's notes pointing to the log.

## Anti-patterns

- Long retrospective prose nobody re-reads — keep action-oriented
- "We'll do better next time" — not a fix
- Findings without fix-surface mapping
- Re-writing notes that already exist (search first; if the note exists, the failure is upstream of capture)
- Treating execution-agent failures in isolation when orchestrator-side instructions caused them
- Writing a process or skill for a one-off — let patterns earn their fix surface
