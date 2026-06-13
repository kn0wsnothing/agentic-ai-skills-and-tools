# Voice Development — Claude Code

## Setup

### 1. Create the voice development folder

```bash
mkdir -p {workspace}/writing/voice/Freewriting
mkdir -p {workspace}/writing/voice/Exercises/Voice\ archaeology
mkdir -p {workspace}/writing/voice/Exercises/Imitation
mkdir -p {workspace}/writing/voice/Exercises/Constraints
mkdir -p {workspace}/writing/voice/Exercises/Socratic
mkdir -p {workspace}/writing/voice/Exercises/Codification
```

Adjust the path to wherever you keep your writing files.

### 2. Create the starter files

Create these three files in your `{voice_dir}`:

**`Voice development progress.md`**
```markdown
# Voice development progress

Phase: 1 — Observe
Started: YYYY-MM-DD
Last session: —

## Freewriting log
(sessions will be added here)

## Exercise log
(exercise completions will be added here)

## Check-in log
(weekly check-ins will be added here)
```

**`Voice profile.md`**
```markdown
# Voice profile

(Updated by archaeology and codification exercises)

## Current characterisation
(to be filled in after first archaeology exercise)

## AI-tell patterns to watch for
(list patterns that creep in when you're not paying attention)
```

**`Personal writing style guide.md`**
```markdown
# Personal writing style guide

(Updated by codification exercises — first version at month 6)

## Voice in 3 sentences
(to be written after first codification)

## Signature moves
(to be filled in)

## Default traps
(to be filled in)
```

### 3. Configure the skill path

In your `CLAUDE.md` or project instructions, add:

```markdown
### Voice Development
When running voice development exercises, follow `skills/voice-development/SKILL.md`.
Voice development folder: {workspace}/writing/voice/
```

Replace `{workspace}/writing/voice/` with the actual path you chose.

### 4. Copy the skill

```bash
cp skills/voice-development/SKILL.md ~/.claude/skills/voice-development/SKILL.md
```

Or reference it directly from your project's skills folder.

---

## Running

### Starting a session

Just invoke a command:

```
/freewrite
```

The skill reads your progress tracker and voice profile at session start, then runs the appropriate exercise.

### Weekly check-in

```
/check-in
```

Five minutes. Three questions. Updates the progress tracker.

### Check where you are

```
/voice-status
```

Reads current state and tells you what to do next.

---

## Model recommendation

For Exercise 1 (archaeology) and Exercise 5 (codification), use Opus or your strongest available model — these exercises require deep pattern recognition across multiple texts.

For freewriting analysis, check-ins, and status snapshots, any capable model works.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — five-exercise arc + session types |
| **Agent** | Claude Code (Opus for archaeology + codification) |
| **Tools** | File read/write via Claude Code's filesystem access |

---

## Tips

- **Freewriting works best unedited.** Write without stopping, paste the raw output. The alive/flat distinction only shows up in unguarded prose.
- **Do not skip the departure step** in Exercise 2. The imitation is setup; the departure is the data.
- **The constraint is the teacher.** In Exercise 3, don't ask the agent for help during the constraint itself. Do the exercise first, debrief after.
- **The style guide is a living document.** After codification, you'll still update it from check-ins and new exercises. It's not finished at month 6 — it just becomes more accurate.
