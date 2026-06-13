# Newsletter Outline — Claude Code

## Setup

### 1. Add to CLAUDE.md

```markdown
### Newsletter Outline
When asked to outline a newsletter piece (trigger: "outline [topic]", "let's outline [topic]",
"start an outline"), follow the instructions in `skills/newsletter-outline/SKILL.md`.
```

### 2. Copy the skill

```bash
mkdir -p skills/newsletter-outline
cp /path/to/SKILL.md skills/newsletter-outline/
```

### 3. Configure paths

Edit the Configuration table in `SKILL.md` to match your workspace. Minimum required:

```
NOTES_DIR       → path to your writing archive
OUTLINES_DIR    → path to your outlines directory
PUBLISHED_DIR   → path to your published pieces
```

Optional but recommended:
```
GUIDING_QUESTIONS_PATH → your editorial compass questions file
STYLE_GUIDE_PATH       → your writing style/voice guide
```

---

## Running

Trigger in conversation:

```
outline [topic or title]
```

or

```
let's outline the piece about [topic]
```

The skill asks three framing questions (one at a time), recommends source notes, runs the overlap scan, then drafts sections one at a time for your approval. Each approved section is written to the outline file immediately.

---

## Session Discovery

Claude Code is a single-session agent. The outline session is typically interactive — you answer the framing questions, confirm notes, approve each section. The skill is designed for this back-and-forth flow.

If the session ends mid-outline, the partial file on disk reflects all approved sections. Resume by asking Claude to continue from where it left off, referencing the outline file.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 5-step outline pipeline |
| **Agent** | Claude (Sonnet or Opus; Opus recommended for complex topics) |
| **Tools** | Claude Code's file read/write |

---

## Tips

- **Framing questions are the real work.** Don't rush them. The quality of the outline is determined in Step 1.
- **Source note scan only helps if your archive is tagged.** If notes don't have consistent frontmatter or titles, the scan may miss relevant material. Tell the agent about notes it misses.
- **Overlap scan on first few essays.** If your published archive is small, the overlap scan is fast. As the archive grows, it becomes more valuable — repeat stories are a real failure mode.
- **Pairs with newsletter-draft.** The outline file is the input. When you're ready: "use newsletter-draft on [title] - outline.md".
