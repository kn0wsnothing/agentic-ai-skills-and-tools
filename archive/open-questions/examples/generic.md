# Open Questions — Generic / Other Platforms

Setup for any agent platform that supports persistent instructions and file access.

---

## Prerequisites

- An agent with persistent instructions (system prompt, CLAUDE.md, rules file, etc.)
- File read/write access for listing and updating question notes
- A folder for your open questions

---

## Setup

### 1. Create your questions folder

Pick a location. Replace `{questions}/` throughout with your actual path.

```
{workspace}/questions/
```

### 2. Add the skill to your agent's persistent instructions

Include this in your system prompt or rules file:

```
When asked to work on a question or run an open-questions session, follow
the instructions in skills/open-questions/SKILL.md. Questions are stored
in {questions}/ as markdown files with status: open frontmatter.
```

### 3. Copy SKILL.md into your agent's skill directory

However your platform loads skills — paste the content of `SKILL.md` into
the appropriate location.

---

## Running

Trigger the session by saying:

```
Let's work on a question.
```

Or paste a question directly to skip the list:

```
I want to think through: [your question]
```

---

## Question Note Format

One markdown file per question, in `{questions}/`:

```markdown
---
type: open question
status: open
date: YYYY-MM-DD
---

# Question

The question, stated plainly.

# Context

Why this question surfaced. What's at stake.

# Builds on

Links, references, or related questions.
```

**Status values:** `open` → active; `promoted` → became something (a piece, a note); `discarded` → dissolved or answered.

The agent will list notes with `status: open` at the start of a session. After the session, it should offer to update the `# Question` section and `status:` field — proposing the change before writing.

---

## Without a Questions Folder

The folder is optional. If you don't have one, paste a question directly at the start of a session. The agent will run the session without any file operations.

The folder adds durability — questions accumulate, get worked through slowly, and get marked when they're done. But the thinking session itself works without it.

---

## Platform Notes

| Concern | Notes |
|---------|-------|
| **Model quality** | Use your strongest available model. The posture inversions require judgment — weaker models drift toward advice and summarization. |
| **Context length** | Open question sessions run long. Ensure your context window can hold a full session (10–20 exchanges). |
| **File access** | The skill needs to read `{questions}/` to list questions and write to individual notes after the session. If your platform sandboxes file access, configure accordingly. |
| **Persistent instructions** | The posture instructions need to stay loaded throughout the session — not just at the start. Ensure your platform doesn't drop earlier context mid-session. |
