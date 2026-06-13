# Work Closure — Generic / Any Agent

This example shows how to adapt the Work Closure skill for any agent platform or custom setup. It covers the minimum requirements, the gate mechanism, and the most common adaptation points.

---

## Minimum Requirements

Your agent needs:

1. **File read/write access** to your workspace directory
2. **A way to ask questions and wait for answers** — the skill's gate mechanism relies on explicit user confirmation at three points before it proceeds. If your platform can't pause for input, work closure doesn't work as-is (see Headless Adaptation below).
3. **The ability to read multiple files in parallel** — Phase A scans several sources simultaneously. Sequential reads work but are slower.

Optional but useful:
- Bash/shell access for the project landscape scan in Phase B
- Subagent support for delegating verbose meeting-note extraction

---

## Path Configuration

Set these in your agent's persistent instructions or system prompt. Use them consistently as placeholders throughout SKILL.md:

```
{workspace}    = /absolute/path/to/your/notes
{daily_notes}  = relative path to daily notes directory
{inbox}        = relative path to inbox directory (contains Inbox.md)
{projects}     = relative path to projects directory
{areas}        = relative path to areas/responsibilities directory
{meetings}     = relative path to meeting notes directory
{logs}         = relative path to agent session logs (optional)
{archive}      = relative path to archive directory
```

---

## Gate Mechanism

The skill has three hard gates where execution must pause for user input:

1. **Phase A → Phase B:** "List assembled — prune, add, or ready to clarify?"
2. **Phase B → inbox verification:** "Confirm or correct the batch?"
3. **Phase C entrance:** "Inbox cleared. Ready to close the workday?"

How you implement these depends on your platform:

| Platform type | Gate implementation |
|---------------|-------------------|
| Chat interface (Claude, GPT, etc.) | End each gate turn with a direct question; wait for the next message |
| Tool-calling agent | Use an `ask_user` or `input_required` tool call |
| Custom orchestrator | Emit a `pause` event with the question; resume on response |
| CLI agent | Print the question; `input()` or equivalent |

The key constraint: **no disposition, write, or next-phase action may appear in the same turn as a gate question.** The question ends the turn.

---

## Project Landscape Scan

Phase B opens with a scan of your active projects to build a routing surface. The SKILL.md shows a bash grep approach for Obsidian/markdown vaults:

```bash
for f in {projects}/Active/*/*.md; do
  outcome=$(awk '/^outcome:/{sub(/^outcome:[ ]*/,""); print; exit}' "$f")
  echo "$f :: ${outcome:-<no outcome>}"
done
```

If your project system is different, adapt accordingly:

- **Notion/Linear/Jira:** query your project list API and extract project names + goals
- **Plain text / no metadata:** read the first heading or first paragraph of each project file
- **No project system:** skip the landscape scan; use the inbox items themselves to derive destinations during Phase B

The purpose is a dedupe surface — seeing what's already tracked before proposing new routes. If you can get "project name + what done looks like" for each active project, you have what you need.

---

## Daily Note Format

The skill reads `{daily_notes}/YYYY-MM-DD.md` and expects to find at minimum:

- `## Focus` — today's intended work items
- `## Notes` — captures and observations from the day

It writes the workday summary to a `## Work closure` section (created if absent).

If your daily notes use different section names, update the relevant steps in SKILL.md to match.

---

## Inbox Format

The skill treats the inbox as two things:

1. **`{inbox}/Inbox.md`** — a markdown file with bullet-point captures
2. **`{inbox}/` folder** — individual files dropped here (PDFs, voice memos, clipped pages, etc.)

Both must be empty at the end of Phase B. If your inbox is a single file only (no folder drops), remove the folder-scan from Step 3 and the folder-verification from the inbox check.

---

## Meeting Notes (Optional)

If you don't use a meeting notes folder, remove Step 3's meeting-note extraction from SKILL.md entirely. The rest of the skill works without it.

If you do use meeting notes, the skill distinguishes processed from unprocessed notes via a frontmatter marker. Use whatever marker fits your system — `action_items_extracted: true` is the example in SKILL.md, but any boolean flag works. The important thing is that the skill doesn't re-extract notes it already processed on a prior run.

---

## Headless / Non-Interactive Adaptation

If your platform can't pause for user input (fully automated pipeline, no human in the loop), work closure can be adapted to a report-only mode:

1. Run Phases A and B in collection-only mode — extract and propose dispositions but don't execute any writes
2. Write the proposed batch to a staging file (e.g., `{logs}/work-closure-YYYY-MM-DD-pending.md`)
3. Send the staging file to the user for async review and confirmation
4. On confirmation (next session, email reply, webhook), execute the approved batch

This is more complex to implement but preserves the core value (surfacing open loops, proposing routes) without requiring synchronous interaction.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the three-phase interactive shutdown |
| **Agent** | Any agent with file read/write and a gate mechanism |
| **Tools** | File read/write (required); Bash (recommended for project scan); subagents (optional for meeting extraction) |
