# Breadcrumb

You are running the Breadcrumb skill — a structured session wrap-up that writes a one-line timestamped entry to the daily note, a full session log to the logs directory, updates agent context, and routes durable memory to the right destination.

Trigger words: "breadcrumb", "wrap up", "log this session", "we're done".

---

## Configuration

| Parameter | Description |
|-----------|-------------|
| `{daily_notes}` | Directory for daily note files (YYYY-MM-DD.md) |
| `{logs}` | Directory for session log files |
| `{memory}` | Root directory for memory files |
| `{projects}` | Active project notes directory (optional) |
| `{inbox}` | Unprocessed capture queue (optional) |
| `{agent_name}` | Canonical agent identifier used in log frontmatter |
| `{timezone}` | Timezone for wall-clock timestamps |

---

## Scope rules (non-negotiable)

- **Completed work only.** Never write `Next:`, `TODO:`, `Pending:`, or any forward-looking line. Those belong in the task system.
- **One bullet** in the daily note. All detail goes to the session log file.
- **One sentence** for the breadcrumb text. Start with an action verb.

---

## Step 1 — Write the session log

Write to: `{logs}/YYYY-MM-DD-{agent_name}-{topic-slug}.md`

**Slug rule (non-negotiable):** the slug is a short hyphenated descriptor of the session's topic or outcome — never a number. Examples:
- `2026-04-24-claude-newsletter-outline-v2.md` ✓
- `2026-04-23-claude-schema-debug-analytics.md` ✓
- `2026-04-17-claude-2.md` ✗ (number, not a topic)

If the day has multiple sessions on the same topic, append a sub-descriptor rather than a number: `-morning`, `-followup`, `-reprise`.

Required frontmatter:

```yaml
---
type: agent-log
session_id: YYYY-MM-DD-NNN
agent: {agent_name}
platform: {platform}
mode: operations | thinking | publishing
vault_changes: true | false
---
```

**Classification stamps** (add when applicable):
- If the session's primary work was **building or maintaining the agent system itself** (skills, scripts, hooks, memory stores, harness config), add:
  ```yaml
  meta: true
  unblocks: "one line naming the object-level work this build serves"
  ```
  The `unblocks:` claim is mandatory when `meta: true` — it's the build's justification, written while it's honest.
- If the session was **personal build or life work** (venture, newsletter, content, personal practices), add `personal: true`.

Body: session summary, files modified, decisions, observations worth keeping.

Writes to `{logs}/` are auto — no gate needed.

---

## Step 2 — Compose the breadcrumb

One sentence describing what was done. Start with an action verb. Write from the user's point of view — the daily note is the user's narrative of their day, not the agent's.

**Recognized action verbs:** created, updated, built, fixed, wrote, added, modified, shipped, drafted, designed, proposed, specced, planned, resolved, decided, synced, published, configured, enabled, reviewed, refactored, deleted, removed, merged, reverted, tested, verified, investigated, diagnosed, analyzed.

**Keep structure-free:**
- No colon-prefixed labels (`Next:`, `Open:`, `Memory:`) — line-by-line parsers turn these into duplicate bullets
- No list markers, section headers, or multiple paragraphs
- No forward-looking lines

Format:

```
- HH:MM — {one-sentence breadcrumb} → [Session log]({logs}/YYYY-MM-DD-{agent_name}-{topic-slug}.md)
```

**Timestamp protocol:** read the current time from the system clock in `{timezone}`. Never estimate.

---

## Step 3 — Task-state scan (mandatory)

Before composing the vault delta proposal, walk every task surface this session could have touched and classify each change. This step runs every session — even when no project files were directly edited, conversation alone can resolve tasks.

Surfaces to scan:

- **Today's daily note** (`{daily_notes}/YYYY-MM-DD.md`) — did any Focus items ship, get started, get dropped, or get replaced?
- **Each project in play** (`{projects}/{project}/{project}.md`) — `## Next actions`, `## Waiting for`. A project is "in play" if files under it were edited OR if the session discussed it.
- **Inbox** (`{inbox}`) — anything captured mid-session that hasn't been routed? Anything that should be?

For every item on those surfaces, classify:

| Status | Action |
|--------|--------|
| **Completed** | Flip `[ ]` → `[x]`; mark done in whatever status convention you use |
| **In flight** | Started but not finished; mark as in-progress |
| **Superseded / rescoped** | Mark dropped or rewrite to reflect new shape |
| **New follow-up** | Add as a new open task; route to the right project or inbox |
| **Waiting-for change** | New blockers added, or resolved blockers cleared |

Every classification feeds step 4. If the scan returns nothing, say so explicitly in the proposal (`No task-state changes`) — silence reads as skipped.

**Delegation escape hatch:** if the scan would require reading more than ~3 long project notes (300+ lines each), delegate the reads to a subprocess or smaller model with the session summary as briefing, then classify inline afterward.

---

## Step 4 — Compose the vault delta proposal

Assemble a proposal listing each destination and exact content:

- **Daily note** — append breadcrumb under the Notes section; tick off Focus items that shipped; add any new same-day Focus items
- **Agent context** (`{memory}/agents/{agent_name}-context.md`) — update the current state section (sparse)
- **Project notes** (`{projects}/{project}/{project}.md`) — for each in-play project:
  - Completed tasks: mark done
  - In-flight tasks: mark in-progress
  - Superseded tasks: mark dropped or rewrite
  - New follow-ups: add with appropriate tags
  - Waiting-for: add new blockers, clear resolved ones
  - Notes / session log section: observations or decisions belonging to the project's running record
- **People notes** (`{memory}/people/{name}.md` or equivalent) — only if a person entity was meaningfully touched
- **Semantic memory** (`{memory}/semantic/`) — only for distilled principles worth keeping long-term
- **Episodic memory** (`{memory}/episodic/`) — only for significant decisions worth recalling months later
- **Inbox** — only for explicit unrouted captures

---

## Step 5 — Present the proposal, wait for APPLY

Show paste-ready blocks for each destination, explicitly labeled. End with:

> Reply `APPLY` to write all of the above, or specify file-level approvals.

**Do not write anything outside the auto-write zone (`{logs}/`) before the user confirms.**

---

## Step 6 — Apply

On `APPLY`:

1. Append breadcrumb to the daily note at the correct chronological position
2. Update agent context
3. Write any proposed notes to project/memory/reference destinations
4. Report completion with the list of files written

---

## Step 7 — Verify (optional)

If the session log mechanism can silently produce a fallback message (e.g., "No major outcomes captured") when it doesn't recognize the input, check the returned summary before confirming. If it contains the fallback, re-run with a recognized action verb.

---

## Failure modes (reference)

Documented failures kept for institutional memory:

- `Next:` lines in breadcrumb text produced doubled bullets in the daily note
- Colon-prefixed labels (`Open:`, `Memory:`) turned into separate daily-note entries
- Unrecognized leading verbs silently triggered the fallback message; session content was dropped
- Multi-paragraph `session_text` produced multiple daily-note bullets
- Session logs were numbered (`-1`, `-2`) instead of topic-slugged, making the logs directory illegible
- Task-state scan skipped entirely; project notes drifted out of sync with actual completion state

Fix pattern in all cases: one sentence, recognized action verb, no labels, no lists, no forward-looking content, topic-based slug.

---

## Rules summary

| Rule | Details |
|------|---------|
| Propose-first for anything outside `{logs}/` | No exceptions |
| Task-state scan is mandatory every session | Even thinking-only sessions can resolve tasks |
| Completed work only | Never write forward-looking lines |
| One bullet in the daily note | Detail goes to the log file |
| Topic-slug only | Never number session logs |
| Agent identification | Always use the canonical agent name — never copy through a tool's default |
