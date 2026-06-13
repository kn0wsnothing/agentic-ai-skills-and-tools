# Morning Review — Claude Code

## Setup

### 1. Install the skill

```bash
mkdir -p {your-project}/skills/morning-review
cp /path/to/SKILL.md {your-project}/skills/morning-review/
```

### 2. Wire into CLAUDE.md

Add to your `CLAUDE.md`:

```markdown
### Morning Review
When the user says "morning review", "/morning-review", "let's do the review",
or "start the review", follow the instructions in
`skills/morning-review/SKILL.md`. One step at a time — the user decides everything.
```

### 3. Configure paths

In CLAUDE.md (or a separate config block the skill can read), set your workspace paths:

```markdown
## Workspace paths
- WORKSPACE: /path/to/your/notes
- INBOX_FILE: {WORKSPACE}/Inbox/Inbox.md
- PROJECTS_DIR: {WORKSPACE}/Projects/Active/
- AREAS_DIR: {WORKSPACE}/Areas/
- MEETINGS_DIR: {WORKSPACE}/Meetings/
- DAILY_NOTE_PATTERN: {WORKSPACE}/Daily/YYYY-MM-DD.md
- TIMEZONE: America/New_York  # adjust to your timezone
```

### 4. Configure calendar access (optional but recommended)

Calendar access surfaces meeting load for Focus capacity checks. Claude Code supports calendar via MCP servers:

```json
// .claude/settings.json — example with iCloud calendar MCP
{
  "mcpServers": {
    "icloud-calendar": {
      "command": "npx",
      "args": ["-y", "@your-mcp-server/icloud-calendar"]
    }
  }
}
```

Or with Google Calendar MCP — see the MCP server's documentation for setup.

---

## Running

### Manual trigger

In any Claude Code session:

```
morning review
```

or

```
/morning-review
```

### Scheduled (automated)

Run at a fixed time each morning:

```bash
# Crontab — 7:30am daily
30 7 * * * cd /path/to/project && claude --print --model claude-sonnet-4-6 \
  "Run the morning review following skills/morning-review/SKILL.md." \
  >> /var/log/morning-review.log 2>&1
```

For the automated case, consider a non-interactive model variant that writes a pre-processed collection list for you to review rather than running the full interactive session.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the four-phase review pipeline |
| **Agent** | Claude (Sonnet works well; Opus for heavier synthesis days) |
| **Tools** | File read/write, Bash for vault scans, optional calendar MCP |

---

## Workspace Scan Commands

The skill runs vault-wide scans in Phase B. These are the patterns used internally — useful if you want to adapt them for your workspace layout:

**Overdue items:**
```bash
grep -rn "due:: " Projects/ Areas/ --include="*.md" \
  | grep -v "\[x\]" | grep -v "status:: done"
```

**Open tasks by project:**
```bash
grep -rn "\[status:: next\]\|\[status:: waiting\]" \
  Projects/Active/ --include="*.md"
```

**Active project outcomes:**
```bash
for f in Projects/Active/*/*.md; do
  outcome=$(awk '/^outcome:/{sub(/^outcome:[ ]*/,""); print; exit}' "$f")
  echo "$f :: ${outcome:-<no outcome>}"
done
```

Adapt the field names to your own task/frontmatter conventions.

---

## Tips

- **Run before checking email.** The review sets intentions before external demands crowd in.
- **The sweep is a conversation.** Phase B (collection) can run non-interactively. Phase C (clarify) needs you present. Don't schedule a fully automated run if you want the routing decisions to be yours.
- **First few runs will surface backlog.** If your inboxes have built up, the first review will be longer than normal. That's the point — work through it once, and subsequent days are faster.
- **Use a consistent trigger phrase.** "Morning review" or "/morning-review" is most reliable. Variations like "let's plan today" also work if you've set them up in CLAUDE.md.
