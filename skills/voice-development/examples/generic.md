# Voice Development — Generic Setup

Platform-agnostic setup for any agent framework that supports file read/write.

---

## What you need

- An agent with file read/write access to a local directory
- A voice development folder (`{voice_dir}`) you can point the skill at
- The three starter files (below)
- A way to invoke the skill (system prompt, slash command, or direct instruction)

---

## Folder structure

```
{voice_dir}/
  Voice development progress.md
  Voice profile.md
  Personal writing style guide.md
  Freewriting/
    YYYY-MM-DD.md
  Exercises/
    Voice archaeology/
    Imitation/
    Constraints/
    Socratic/
    Codification/
```

Create this structure wherever your agent can write files. For a local-first setup, a folder in your notes system or writing directory works well.

---

## Starter files

**`Voice development progress.md`**
```markdown
# Voice development progress

Phase: 1 — Observe
Started: YYYY-MM-DD
Last session: —

## Freewriting log

## Exercise log

## Check-in log
```

**`Voice profile.md`**
```markdown
# Voice profile

## Current characterisation
(filled in after first archaeology exercise)

## AI-tell patterns to watch for
```

**`Personal writing style guide.md`**
```markdown
# Personal writing style guide

## Voice in 3 sentences

## Signature moves

## Default traps
```

---

## Configuring the skill

In your system prompt or agent instructions, add something like:

```
You have access to a voice development skill. When the user invokes /freewrite,
/exercise [type], /check-in, or /voice-status, follow the instructions in
skills/voice-development/SKILL.md.

Voice development folder: {voice_dir}
Replace {voice_dir} with the actual path when running the skill.
```

---

## Invoking the skill

Trigger commands are listed in SKILL.md. Pass them directly to your agent:

```
/freewrite
/exercise archaeology
/exercise imitation Joan Didion
/exercise constraints
/exercise socratic [your topic]
/exercise codification
/check-in
/voice-status
```

If your platform doesn't support slash commands, describe the action in natural language:

```
Run the voice archaeology exercise following the voice-development skill.
```

---

## Session discovery

The skill reads three files at the start of every session:
1. `Voice development progress.md` — for phase and session history
2. `Voice profile.md` — for current voice characterisation and AI-tell patterns
3. `Personal writing style guide.md` — for current style reference

Make sure your agent has read access to these files before it begins.

---

## Scheduling check-ins

The weekly check-in (`/check-in`) works best on a consistent day. If your platform supports scheduled tasks, wire it to run once a week. Otherwise, add it to a recurring manual reminder.

The skill will not nag you about missed sessions. The check-in is a low-friction re-entry point — 5 minutes, 3 questions.

---

## Model guidance

Archaeology (Exercise 1) and codification (Exercise 5) require pattern recognition across multiple texts. Use your strongest available model for these sessions. The other exercises and check-ins work well with a mid-tier model.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — five-exercise arc + session types + cadence |
| **Agent** | Any capable LLM with file read/write access |
| **Tools** | File read/write (read 3 context files at start; write exercise outputs) |
