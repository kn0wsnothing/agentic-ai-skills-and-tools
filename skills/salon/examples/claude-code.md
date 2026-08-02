# Salon — Claude Code

## Setup

### 1. Create your folders

Pick locations for the artifacts this skill reads and writes. All are optional — configure only what you keep.

```bash
mkdir -p {workspace}/questions/open
mkdir -p {workspace}/notes/fleeting
mkdir -p {workspace}/notes/anecdotes
```

### 2. Add to CLAUDE.md

```markdown
### Salon
For any conversational thinking session — an open question, a book/film just
encountered, a half-formed idea, or an open-ended "let's talk about" opening —
follow the instructions in `skills/salon/SKILL.md`.

Paths:
- {open_questions}: questions/open/
- {fleeting_notes}: notes/fleeting/
- {anecdotes}: notes/anecdotes/
- {reading_notes}: notes/reading/reference/       # optional
- {highlights}: notes/reading/highlights/          # optional
- {session_logs}: logs/                            # optional
- {daily_notes}: daily/                            # optional
```

### 3. Copy the skill

```bash
mkdir -p .claude/skills/salon
cp /path/to/SKILL.md .claude/skills/salon/
```

---

## Running

### Start a session

Any of these will trigger it:

```
Let's work on a question.
I've been thinking about why the onboarding flow keeps stalling.
I just finished [book] and it's bugging me.
Let's talk about something.
```

The agent reads your opening line, silently picks a lane, and (for the open-question and idea lanes) may run one background search over your notes before replying. You won't see this happen — it's priming, not a visible step.

### A typical open-question session

```
You: Let's work on a question.
Agent: [lists open questions from {open_questions}/] Which one do you want to sit with today?
You: The one about whether the constraint is real or a habit.
Agent: [one move — a question or observation, not a list]
You: [respond]
Agent: [follows the thread — no summary, no advice]
... (several exchanges)
Agent: Where does this go — writing seed, new note, refined question, discard, or leave it open?
You: Refined question — it's actually about something narrower now.
Agent: [updates the note, shows the diff, confirms]
```

### At close

Every session — regardless of lane — gets a session log and a daily-note line automatically, if you've configured those paths. Then the agent offers a translation: a sharpened question, a fleeting note, an anecdote, or nothing. You pick.

---

## WAT mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — lane detection, base posture, translation offer |
| **Agent** | Claude (a strong model — see Tips) |
| **Tools** | Claude Code's file read/write; optionally a local search/retrieval tool wired in as `{concept_search}` |

---

## Tips

- **Use a strong model.** The posture — staying uncertain, complicating clean ideas, one move per response — requires real judgment. A fast/lightweight model will drift toward summarizing and advice-giving.
- **Don't configure a folder you don't use.** Every path is independently optional. An unconfigured path just means that lookup or write is skipped — the skill doesn't fail without it.
- **Let a session end unfinished.** A session that ends with a sharper question and no resolution is a successful one — resist the urge to force the translation offer into producing something every time.
