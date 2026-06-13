# Message Drafting — Claude Code

## Setup

### 1. Add to CLAUDE.md

```markdown
## Message Drafting

When asked to draft a message, Slack, email, DM, or reply (or "/message-drafting"),
follow the instructions in `skills/message-drafting/SKILL.md`.

### Path configuration

| Placeholder | Path |
|-------------|------|
| `{workspace}` | `/absolute/path/to/your/notes` |
| `{voice_guide}` | `Content/Voice/voice-guide.md` |
| `{style_reference}` | `Content/Voice/style-guide.md` |
| `{projects}` | `01 Projects/Active` |
| `{people}` | `03 Reference/People` |
| `{daily_notes}` | `05 Daily` |
| `{inbox}` | `00 Inbox` |
```

Replace paths to match your workspace layout. The placeholder names are used throughout `SKILL.md` — configure them here once.

### 2. Copy the skill

```bash
mkdir -p skills/message-drafting
cp /path/to/SKILL.md skills/message-drafting/
```

---

## Running

### Trigger phrases

Any of these start the skill:

```
Draft a Slack to [person] about [topic]
Help me message [role] about [situation]
Write an email to [person]
Draft a reply to [message]
What should I say to [person] about [X]?
/message-drafting
```

### What the agent reads

Claude Code reads files via the `Read` tool. Before drafting, the agent will read:
- Your voice guide (configured above)
- The recipient's people note at `{people}/{Recipient Name}.md` if it exists
- Any project notes relevant to the message subject
- Today's daily note for live context

### Saving drafts

When you say "save it" or "write it to the vault", the agent writes to:
```
{inbox}/{Recipient Name} {message-type} — YYYY-MM-DD.md
```

Or, if the message is project-tied:
```
{projects}/{Project Name}/{Recipient Name} draft — YYYY-MM-DD.md
```

---

## Voice Guide Setup

The skill reads your voice guide before every draft. A minimal voice guide covers:

```markdown
## Register defaults
- Peer-to-peer default for most professional messages
- Slightly more formal for executives or external contacts
- DMs to close colleagues: casual — abbreviations, no salutation

## What I write like
- [3–5 sentences describing your natural register, characteristic phrases, what to avoid]

## Things to never write
- "Happy to" / "feel free to" / "just wanted to"
- Headers or bullet sections in short messages
- [add your own]
```

Put this in `{voice_guide}`. The more specific it is, the less correction you'll need.

---

## People Notes

People notes are per-person files at `{people}/{Full Name}.md`. Minimal structure:

```markdown
---
name: Full Name
role: Their role / title
---

## Communication preferences
[How they prefer to receive information — detail level, channel, formality]

## Relationship context
[Standing, shared history, any active threads]

## Things to avoid
[Third rails, framings that have landed badly, topics to tread carefully on]
```

The agent reads this before drafting to calibrate register and avoid known landmines. A short people note is better than none — even 3 lines of relationship context changes the draft.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — six-step drafting flow with intake gate and pre-show scan |
| **Agent** | Claude (Sonnet is fine for most messages; Opus for high-stakes or complex context-synthesis) |
| **Tools** | `Read` tool for workspace files; `Write` tool for saving drafts |

---

## Tips

- **The intake gate is worth it.** The consolidated intake question takes 10 seconds and prevents 3–4 rounds of correction. Don't skip it or route around it by front-loading context.

- **Point to source material explicitly.** If your message references a meeting, tell the agent which meeting note to read. "The message is a follow-up to the meeting on [date]" is more reliable than letting the agent infer.

- **Short voice guides outperform no voice guides.** The agent will apply default LLM register without one. Even a paragraph describing how you write is enough to anchor calibration.

- **Corrections are pattern fixes.** When you correct a phrase, the agent should scan the whole draft for the same pattern. If it only fixes the one line you pointed at, say "check the rest of the draft for the same issue."

- **"Save it" is confirmation.** Once you've reviewed the draft in conversation and say "save it" or "write it," the agent writes immediately — no additional gate.
