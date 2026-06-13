# Process Notes — Generic (Any File-Access Agent)

## Requirements

Your agent needs:

- File read and write access to your notes directory
- The ability to move or copy files (for the fleeting → processed handoff)
- A way to receive the skill instructions (system prompt, instruction file, or equivalent)

No database, no search index, no special tooling required. The skill uses only file operations.

---

## Setup

### 1. Define your paths

Pick concrete directories for each parameter and record them somewhere your agent can read (system prompt, config file, or instruction file):

```
{notes}           → where completed main notes live
{fleeting}        → where raw, unprocessed fleeting notes live
{processed}       → where fleeting notes go after development
{open_questions}  → a file or directory for saved questions
{guiding_questions} → optional: a file with the author's standing questions/themes
```

Example:

```
{notes}           → workspace/notes/main/
{fleeting}        → workspace/notes/fleeting/
{processed}       → workspace/notes/fleeting/processed/
{open_questions}  → workspace/notes/questions/open-questions.md
{guiding_questions} → workspace/notes/questions/guiding.md
```

### 2. Install the skill

Copy `SKILL.md` to wherever your agent loads persistent instructions from. Register it explicitly — something like:

```
When asked to process, develop, or continue a note, follow the instructions
in skills/process-notes/SKILL.md.
```

### 3. Set up your note template

The skill asks Socratic questions tied to your main note template's sections. Define which sections are required in your template, and the skill will walk through them in order.

Minimum required sections: Thesis, Supporting argument, Why it matters, Questions it opens, Connections.

Add or remove sections to match your system.

---

## Running

Trigger phrases the skill recognizes:

```
Process [note title]
Develop [note title]
Continue [note title]
Let's process my notes
Which notes should I develop?
```

The agent reads the note, begins the Socratic session, writes sections as you respond, verifies each section before moving on, and handles the seed brainstorm and question gate at the end.

---

## File Move Convention

When a fleeting note is developed into a main note, the skill moves it to `{processed}/`. The recommended protocol:

1. Copy the file to `{processed}/`
2. Verify the copy succeeded (read it back)
3. Delete the original

This copy → verify → delete sequence ensures the original survives a partial failure. Only delete after the copy is confirmed.

If your agent has a native move command that's atomic, use it. If not, use the three-step sequence.

---

## Adapting to Your Note System

The skill is designed around a Zettelkasten-style two-layer model (fleeting notes → main notes), but the core loop works with any note structure that has:

- A "raw idea" stage (whatever you call fleeting notes)
- A "developed note" stage (whatever you call main notes)
- A status field in frontmatter to distinguish stages

The section names and required fields are configurable. The Socratic loop, section-by-section confirmation, seed brainstorm, and question gate are the fixed parts.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — Socratic loop, seed brainstorm, question gate, handoffs |
| **Agent** | Any LLM with file read/write access |
| **Tools** | File read, file write, file move (or copy + delete) |
