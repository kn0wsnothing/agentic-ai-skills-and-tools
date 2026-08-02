# Salon — generic (any file-access agent)

Setup for any agent platform that supports persistent instructions and file access.

---

## Prerequisites

- An agent with persistent instructions (system prompt, rules file, or equivalent)
- File read/write access, if you want the translation offer and auto-trace to actually write anything
- Everything else is optional — the skill degrades gracefully to pure conversation with no file access at all

---

## Setup

### 1. Decide which folders you're wiring up

None are required. Configure only the ones you keep:

```
{open_questions}   → a folder of question notes with status: open frontmatter
{fleeting_notes}   → a folder for early-stage ideas feeding a writing pipeline
{anecdotes}        → a folder for personal scenes/stories worth keeping
{reading_notes}    → reference notes about books you've read
{highlights}       → book highlight/quote notes
{session_logs}     → a folder for session logs
{daily_notes}       → daily note files
{concept_search}   → a semantic search tool or command over your notes
```

### 2. Add the skill to your agent's persistent instructions

```
When the user opens with an open question, a book/film reference, a
half-formed idea, or any open-ended "let's talk about" prompt, follow the
instructions in skills/salon/SKILL.md.
```

### 3. Copy `SKILL.md` into wherever your platform loads skills from

---

## Running

Trigger phrases:

```
Let's work on a question.
I've been thinking about [x].
I just read/saw [x].
Let's talk about [x].
```

Or anything open-ended with no clear task attached — the "anything" lane is the default fallback, not an error case.

The agent silently detects a lane from the opening line, applies the base posture (stay with uncertainty, one move per response) for the whole session, then closes with housekeeping writes (if configured) and a translation offer.

---

## Without any folders configured

The skill still works as pure conversation. Session start has nothing to list or pull; session close has nothing to write. The posture and lane arcs are the parts that matter most and require no file access at all — only the artifact and provenance layer does.

---

## Platform notes

| Concern | Notes |
|---------|-------|
| **Model quality** | Use your strongest available model. The posture inversions — staying uncertain, complicating clean ideas, "no move" as a valid turn — require judgment weaker models don't reliably exercise. |
| **Context length** | Sessions run long, especially the open-question lane (10–20 exchanges is normal). Make sure your context window holds a full session without dropping the posture instructions partway through. |
| **File access** | If you wire up any folder, the skill needs read access at session start and write access at session close. If your platform sandboxes file access, configure the allowed paths accordingly. |
| **Persistent instructions** | The base posture needs to stay active for the whole session, not just the opening turn — verify your platform doesn't truncate or drop earlier system context mid-conversation. |
