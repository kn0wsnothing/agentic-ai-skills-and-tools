# Newsletter Outline — Generic / Any Platform

This guide covers wiring up the Newsletter Outline skill on any agent platform that supports persistent instructions and file read/write.

---

## What Your Platform Needs

- **Persistent instructions** — a system prompt, rules file, or CLAUDE.md equivalent that your agent loads every session
- **File read access** — the agent needs to read notes from your writing archive
- **File write access** — the agent needs to write the outline file on each approved section
- **A writing archive** — a directory of notes, research, and source material to draw from

No scheduling required — this skill is interactive, not automated.

---

## Setup

### 1. Add to your persistent instructions

Drop the following into your system prompt, rules file, or agent configuration:

```
### Newsletter Outline
When the user says "outline [topic]", "let's outline [topic]", or "start an outline",
follow the Newsletter Outline skill instructions. The skill runs five steps in sequence:
framing questions (one at a time), source note recommendations, overlap/callback scan,
section-by-section outline drafting (one section at a time, write on approval), and handoff.
Load the full instructions from: skills/newsletter-outline/SKILL.md
```

### 2. Place the skill file

Put `SKILL.md` somewhere your agent can read it. Common locations:
- `skills/newsletter-outline/SKILL.md`
- `prompts/newsletter-outline.md`
- Embedded directly in your system prompt

### 3. Configure paths

Edit the Configuration table in `SKILL.md` for your directory layout:

```
NOTES_DIR         Where your source notes live
OUTLINES_DIR      Where outlines should be saved
PUBLISHED_DIR     Where published pieces live (for overlap scan)
DRAFTS_DIR        Where in-progress drafts live
```

If you don't have a published archive yet, leave `PUBLISHED_DIR` and `DRAFTS_DIR` unconfigured — Step 3 will run with nothing to scan, which is fine.

---

## Triggering

Say any of:

- "outline [topic or title]"
- "let's outline [topic]"
- "start an outline"

Or, if you're using process-notes or a similar capture skill, the handoff phrase "ready to outline?" triggers it.

---

## Interaction Flow

The skill is designed for interactive back-and-forth:

1. Agent asks framing question 1 → you answer → agent asks question 2 → and so on
2. Agent recommends 3–5 source notes → you confirm or redirect
3. Agent presents overlap scan findings → you approve or make decisions
4. Agent drafts Foundation section → you approve or edit → agent writes it to file → agent drafts Opening → repeat
5. Once all sections are approved, agent confirms the file path and suggests newsletter-draft

Each approved section is written to the outline file immediately. If the session ends early, the partial file is usable.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 5-step outline pipeline |
| **Agent** | Whatever model you're running (stronger models produce better outlines; this skill benefits from reasoning quality) |
| **Tools** | File read/write (platform-native) |

---

## Tips

- **Framing questions are not warmup.** They determine which notes are relevant, which overlap to flag, and what the Foundation section says. Take them seriously.
- **Style guide pays dividends.** If you have a writing voice/structure guide, configure `STYLE_GUIDE_PATH`. The skill uses it to choose essay structure blueprints and emotional mechanics — without it, structure defaults to generic.
- **Guiding questions anchor editorial judgment.** If you maintain a file of compass questions ("why publish this?", "who specifically is this for?"), configure `GUIDING_QUESTIONS_PATH`. The skill names which question the piece serves — or flags if it doesn't serve any.
- **Immediate writes are intentional.** The skill writes each approved section immediately rather than batching. This means the file always reflects the current agreed state, and partial outlines on disk are usable.
- **Pairs with newsletter-draft.** When the outline is done: hand the outline file to newsletter-draft. That skill reads the outline and drafts prose section by section.
