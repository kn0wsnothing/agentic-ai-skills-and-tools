# Open Questions — Claude Code

## Setup

### 1. Create your questions folder

Pick a location for your open questions. This guide uses `{workspace}/questions/` as a placeholder — replace with your actual path.

```bash
mkdir -p {workspace}/questions
```

### 2. Add to CLAUDE.md

```markdown
### Open Questions
When asked to work on a question or run an open-questions session, follow the
instructions in `skills/open-questions/SKILL.md`. Questions are stored in
`{workspace}/questions/` with `status: open` frontmatter.
```

### 3. Copy the skill

```bash
mkdir -p skills/open-questions
cp /path/to/SKILL.md skills/open-questions/
```

---

## Running

### Start a session

```
Let's work on a question.
```

Or paste a question directly:

```
I want to think through this: [your question]
```

### With a questions folder

If you have questions in `{workspace}/questions/`, the agent will list all notes with `status: open` and ask which one to sit with. You pick; it runs.

---

## Question Note Format

Create notes like this in your questions folder:

```markdown
---
type: open question
status: open
date: 2025-09-14
---

# Question

Is the constraint I keep working around a real constraint, or a habit?

# Context

Came up while reviewing a project that's been slow for months.

# Builds on

[[an earlier note that's related]]
```

After a session, the agent will offer to update the `# Question` section (if the question sharpened) and the `status:` field (if it's been promoted, discarded, or left open). It will show you the change before writing.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — session structure and posture |
| **Agent** | Claude (Opus recommended for the judgment calls) |
| **Tools** | File read/write for listing and updating question notes |

---

## Tips

- **Use a strong model.** The posture inversions — staying uncertain, complicating clean ideas — require real judgment. Haiku or Sonnet will drift toward summarization and advice.
- **One session, one question.** Don't be tempted to work through a list. The value is in the depth, not the throughput.
- **Let it be unfinished.** A session that ends with a sharper question and no answer is a successful session.
