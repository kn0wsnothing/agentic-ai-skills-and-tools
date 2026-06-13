# Message Drafting

A disciplined drafting skill for outbound messages — Slack DMs, email, group chat, written replies, talk tracks. Pins channel, audience, length, and intent before touching any words. Draws out the sender's actual position before drafting. Scans for filler before the message is shown.

---

## The Problem

Without a gate, AI drafting defaults to:

- **Document structure** — headers, labelled sections, bullets in a 100-word Slack message
- **Synthesised positions** — paraphrasing someone's view without reading the source, sometimes inverting it
- **Filler patterns** — "visibly," "upstream framing," "happy to," "wanted to flag," "just wanted to check in"
- **Register mismatch** — email formality in a DM, or DM compression in a message that needed a clear ask

All four are correctable after the fact, but they require multiple rounds. This skill prevents them at the source.

---

## How It Works

Six steps, two of them mandatory gates:

1. **Intake (mandatory)** — confirm channel, recipient(s), context level, length cap, intent, register, and anything to avoid. One consolidated question if anything is unclear.
2. **Voice and style** — read the sender's voice guide and style reference before any drafting.
3. **Context scan** — read the sources the message touches: project notes, the recipient's people note, today's state, relevant meeting notes. No paraphrasing without reading the source.
4. **Draft** — apply voice, match register, stay close to source material. One thing per sentence, no structural over-framing, no implicit-frame projection onto the recipient.
5. **Pre-show scan (mandatory)** — scan the full draft against a filler list before the user sees it. Casual voice markers (DM abbreviations, slash compounds, arrow notation) are protected.
6. **Show + iterate** — present the draft as the message itself, not as a doc. Treat any pushback as pointing at a pattern in the whole draft, not just the flagged line.

---

## What "Lead from the Sender's Take" Means

The skill doesn't draft from a topic. It drafts from the sender's actual position. If that position isn't explicit, intake surfaces it — what do you think? what's the ask? — before the first word is written.

This is the discipline that prevents manufactured "we" on solo work, invented positions on others' views, and hedged openings that bury the actual point.

---

## Prerequisites

Your agent needs:

- **Read access** to any notes, project files, or meeting notes the message may reference
- **A voice or style guide** — even a short one. Without it, the agent has no anchor for register calibration.
- **An optional people-notes store** — one file per person with communication preferences and relationship context (strongly recommended for high-stakes recipients)

---

## Configuration

| Placeholder | What it points to | Example |
|-------------|------------------|---------|
| `{workspace}` | Root of your knowledge base | `~/notes`, `~/vault` |
| `{voice_guide}` | Your voice and style reference | `Content/voice-guide.md` |
| `{projects}` | Active projects directory | `01 Projects/Active` |
| `{people}` | People notes directory | `03 Reference/People` |
| `{daily_notes}` | Daily notes directory | `05 Daily` |
| `{inbox}` | Inbox / drafts landing spot | `00 Inbox` |

Set these in your agent's persistent instructions or CLAUDE.md equivalent.

---

## Quick Start

1. **Copy `SKILL.md`** into your agent's skills or instructions directory
2. **Configure the paths** above in your agent's persistent instructions
3. **Point to your voice guide** — the skill reads it before every draft
4. **Trigger:** say "draft a message to X" or "help me write an email about Y"

---

## Scope

Handles: Slack DMs, Slack channel posts, email, group chat (Discord, iMessage, WhatsApp), written replies, talk tracks (spoken cadence), work shares.

Does not handle: long-form publishing, formal reports, newsletter issues. Use a separate drafting workflow for those.

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic / any agent | [examples/generic.md](examples/generic.md) |

---

## Design Rationale

See [SPEC.md](SPEC.md) for the full design rationale — the failure modes this skill was built around, why intake is a hard gate, and the implicit-frame rule.
