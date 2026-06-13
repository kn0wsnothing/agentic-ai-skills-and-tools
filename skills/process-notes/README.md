# Process Notes

A Socratic thinking skill for developing ideas into permanent notes. It takes a raw fleeting note and, one question at a time, draws out the thinking needed to turn it into a complete, standalone main note.

This is the human-in-the-loop half of a Zettelkasten write pipeline. Capture gets the idea into the system; process-notes gets the thinking onto the page.

---

## How It Works

The skill runs in two modes:

1. **Fleeting → main** — takes an unprocessed fleeting note, runs a Socratic thinking session (one section at a time), and produces a new main note at `{notes}/`
2. **Continue → main** — picks up an incomplete main note (`status: seed` or `in-progress`) and resumes from the first empty section

After the thinking session:

- **Seed brainstorm** — scans the completed note for ideas worth spinning off into new notes; presents candidates and waits for selection
- **Question quality gate** — surfaces questions that emerged during the session; asks which 1-2 are worth keeping; saves selected questions to `{open_questions}/`
- **Handoff** — offers optional onward routing (social post, newsletter outline)

The fleeting note is moved to `{processed}/` when done, with `status: processed` and a `developed_into:` link to the new main note.

---

## Prerequisites

Your agent needs:

- **File read/write access** to your notes directory
- **A fleeting notes location** — notes with `status: unprocessed` waiting to be developed
- **A main notes location** — where completed notes live
- **A processed location** — where fleeting notes go after development
- **An open questions store** — a file or directory for questions that surface during sessions
- **Guiding questions** (optional but recommended) — a standing list of the themes or questions the author is working through; used as a compass during the session

---

## Quick Start

1. **Set the paths** in your agent's system prompt or equivalent config file (see Configuration below)
2. **Copy `SKILL.md`** into your agent's persistent instructions
3. **Trigger it:**
   - "Process [note title]"
   - "Develop [note title]"
   - "Which notes should I develop?"

---

## Configuration

| Parameter | Example | Description |
|-----------|---------|-------------|
| `{notes}` | `notes/main/` | Directory for completed main notes |
| `{fleeting}` | `notes/fleeting/` | Directory for unprocessed fleeting notes |
| `{processed}` | `notes/fleeting/processed/` | Where fleeting notes go after development |
| `{open_questions}` | `notes/questions/` | Directory or file for saved questions |
| `{guiding_questions}` | `notes/questions/guiding.md` | Author's standing list of themes and questions (optional) |

---

## Session Shape

Every session follows the same shape regardless of note length:

```
Read → ask one question → wait → write section → verify → confirm → repeat
```

The agent does not ask the next question until the user confirms the previous section is right. Short-circuit completions ("yes that's fine") are accepted — the Socratic loop doesn't require elaborate answers.

---

## Trigger Words

- "process [note title]"
- "develop [note title]"
- "continue [main note title]"
- "let's process my notes"
- "which notes should I develop?"

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic (any file-access agent) | [examples/generic.md](examples/generic.md) |

---

## Design Rationale

See [SPEC.md](SPEC.md) for the design decisions: why the session is Socratic rather than extractive, why section-by-section confirmation matters, and how the seed brainstorm and question quality gate work.
