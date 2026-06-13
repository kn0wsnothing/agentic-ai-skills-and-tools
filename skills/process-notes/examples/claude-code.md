# Process Notes — Claude Code

## Setup

### 1. Define your paths in CLAUDE.md

Add a configuration block to your `CLAUDE.md`:

```markdown
## Process Notes paths

- `{notes}`: `notes/main/`
- `{fleeting}`: `notes/fleeting/`
- `{processed}`: `notes/fleeting/processed/`
- `{open_questions}`: `notes/questions/open-questions.md`
- `{guiding_questions}`: `notes/questions/guiding.md`  # optional
```

### 2. Copy the skill

```bash
mkdir -p .claude/skills/process-notes
cp /path/to/SKILL.md .claude/skills/process-notes/
```

### 3. Register it in CLAUDE.md

```markdown
## Process Notes

When asked to process, develop, or continue a note — or when the user says
"let's process my notes" or "which notes should I develop?" — follow the
instructions in `.claude/skills/process-notes/SKILL.md`.
```

---

## Running

### Process a specific note

```
Process "constraints as creative fuel"
```

The agent reads `{fleeting}/constraints-as-creative-fuel.md` and begins the Socratic session. Sections are written one at a time; you confirm each before it moves on.

### Continue an incomplete note

```
Continue "constraints as creative fuel"
```

The agent reads the existing main note, scans which sections are already filled, confirms them briefly, and jumps to the first empty section.

### Suggest mode

```
Which notes should I develop?
```

The agent scans `{fleeting}` for unprocessed notes and `{notes}` for notes with `status: seed` or `in-progress`, then presents a grouped list for you to choose from.

---

## Session Shape

A typical session for a fleeting note with 5 required sections:

```
Agent: reads fleeting note, asks about the thesis
You: "The constraint is the idea — without limits, there's no direction"
Agent: writes Thesis section, shows it, asks "Does this capture your thinking?"
You: "yes"
Agent: asks about supporting argument
...
[5 rounds later]
Agent: all sections complete, bumps status to seed
Agent: "These could become their own notes — which feel worth seeding?"
You: "1 and 3"
Agent: creates two seed notes, links back to this note
Agent: "These questions came up — which 1-2 are worth keeping?"
You: "just the first one"
Agent: saves question to {open_questions}
Agent: "Note saved. Two options from here: post this idea or start a newsletter outline."
```

Total: 10-20 minutes for a 5-section note. Faster for short answers, slower for complex ones.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — Socratic loop, seed brainstorm, question gate |
| **Agent** | Claude (Sonnet or Opus) |
| **Tools** | Claude Code's file read/write, Bash for file moves |

---

## Tips

- **Short answers are fine.** "Yes that captures it" is a valid confirmation. The Socratic loop doesn't require elaborate responses.
- **Rough phrasing is correct.** The agent's voice standards preserve your natural phrasing. Don't worry about being polished.
- **The seed brainstorm always runs.** Even if you expect no spin-offs, the agent will check. Saying "nothing worth seeding" is a valid answer and takes 5 seconds.
- **Move, don't delete.** The skill moves fleeting notes to `{processed}/` rather than deleting them. You keep the provenance trail.
