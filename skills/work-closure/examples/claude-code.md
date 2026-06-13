# Work Closure — Claude Code

## Setup

### 1. Add to CLAUDE.md

```markdown
## Work Closure

When asked to run work closure (or "/work-closure"), follow the instructions in
`skills/work-closure/SKILL.md`.

### Path configuration

| Placeholder | Path |
|-------------|------|
| `{workspace}` | `/path/to/your/notes` |
| `{daily_notes}` | `05 Daily` |
| `{inbox}` | `00 Inbox` |
| `{projects}` | `01 Projects` |
| `{areas}` | `02 Areas` |
| `{meetings}` | `06 Meetings` |
| `{logs}` | `80 Agent/Logs` |
| `{archive}` | `04 Archive` |
```

Replace paths to match your workspace layout. The placeholder names are used throughout `SKILL.md` — configure them here once.

### 2. Copy the skill

```bash
mkdir -p skills/work-closure
cp /path/to/SKILL.md skills/work-closure/
```

---

## Running

### Manual (end of day)

```
Run work closure. Follow skills/work-closure/SKILL.md.
```

Or if you've registered it as a slash command:

```
/work-closure
```

### Automated (non-interactive mode is not recommended)

Work closure is designed to be interactive — the routing decisions and open-loops sweep require your input. A fully automated run would skip the gates and defeat the purpose.

If you want to automate the *collection* phase only (Phase A) and queue up the batch for your next session, that's a valid adaptation — but it requires modifying SKILL.md to split Phase A from Phase B.

---

## Single-session limitation

Claude Code is typically a single-session agent. Work closure reviews the current conversation and your workspace files — it doesn't have access to other terminal sessions or other project contexts from the same day.

If you ran Claude Code sessions in multiple projects during the day:
- Run work closure at the end of each session, or
- Tell the agent what happened in other sessions during the open-loops sweep (Step 3b)

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the three-phase interactive shutdown |
| **Agent** | Claude (Sonnet or Opus; Opus recommended for routing judgment) |
| **Tools** | File read/write, Bash (for project landscape scan), optional subagents for meeting-note extraction |

---

## Tips

- **Run before day-closure.** Work closure writes the `## Work closure` summary that day-closure reads. If you use both, run work closure first.
- **The project landscape scan uses bash grep.** Make sure Claude Code has Bash tool access to your workspace for Phase B to work efficiently. Without it, the agent will fall back to reading project files one at a time (slower but still correct).
- **Inbox.md format.** The skill expects `{inbox}/Inbox.md` as the canonical capture file and treats everything else in `{inbox}/` as unprocessed files. Adjust if your inbox layout differs.
- **Meeting notes.** If you use a tool that drops meeting transcript files into `{meetings}/`, configure a frontmatter marker (`action_items_extracted: true`) so the skill can distinguish processed from unprocessed notes on subsequent runs.
