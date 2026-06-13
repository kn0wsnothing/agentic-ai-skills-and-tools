# Message Drafting — Generic / Any Agent

This guide shows how to adapt the Message Drafting skill for any agent platform or custom setup.

---

## Minimum Requirements

Your agent needs:

1. **Read access** to the workspace files the message may reference (project notes, people notes, meeting notes, daily notes)
2. **A voice guide** — some reference for the sender's register. Without it, the agent drafts in default LLM voice.
3. **A way to ask questions and wait for answers** — the intake gate pauses for input. If your platform can't do this, see the Async Adaptation section below.

Optional but useful:
- Write access to save drafts to the workspace
- People notes per recipient (communication preferences, relationship context)

---

## Path Configuration

Set these in your agent's persistent instructions or system prompt:

```
{workspace}        = /absolute/path/to/your/notes
{voice_guide}      = relative path to your voice/style reference
{style_reference}  = relative path to any style conventions file (optional)
{projects}         = relative path to active projects directory
{people}           = relative path to people notes directory
{daily_notes}      = relative path to daily notes
{inbox}            = relative path to inbox / drafts landing spot
```

---

## The Intake Gate

The skill's first step is a consolidated intake question. The agent asks once, confirms channel + recipient + length + intent + register + anything to avoid, then drafts.

If anything is pre-supplied in the original request, the agent infers it and skips asking. The gate only fires for genuinely unclear parameters.

| Platform type | How to implement the gate |
|---------------|--------------------------|
| Chat interface (Claude, GPT, etc.) | End the intake turn with the question; wait for the next message |
| Tool-calling agent | Use an `ask_user` or `input_required` tool call |
| Custom orchestrator | Emit a `pause` event with the question; resume on response |
| CLI agent | Print the question; use `input()` or equivalent |

---

## Voice Guide

The skill reads the voice guide before every draft. At minimum, yours should cover:

```markdown
## Register defaults
- [Channel-level defaults — DM vs. email vs. formal]

## Characteristic patterns
- [Phrases, rhythms, structures that sound like you]

## Things to avoid
- [Filler phrases you hate, structural patterns that feel wrong]
```

This file does not need to be long. Specific > comprehensive. Three precise constraints outperform a two-page style essay.

---

## People Notes (Optional)

A people note per recipient is the highest-leverage optional addition. It lets the agent calibrate register, avoid known landmines, and ground relationship context without asking.

Minimal structure:
```markdown
## Communication preferences
[Their preferred detail level, formality, channel]

## Relationship context
[Your standing with them, any active threads, shared history]

## Things to avoid
[Third rails, framings that have gone badly]
```

---

## Context Scan Scope

The skill reads sources before drafting. What to read:

| Source | When |
|--------|------|
| `{projects}/{project}/` main note | Message touches a project |
| `{people}/{recipient}.md` | Always, if the file exists |
| Today's daily note | For live context |
| Meeting notes for the relevant meeting | If the message references a meeting |
| Agent memory / preferences file | Always — check for prior corrections |

The key discipline: never paraphrase someone's view without reading the source. If the source doesn't exist or wasn't read, hedge explicitly ("my read was X") or ask.

---

## The Pre-Show Scan

Before the draft is shown, the agent scans it against a filler list. This is not optional. The scan happens silently; filler never reaches the user.

The scan also protects casual voice markers in DM channels — abbreviations, slash compounds, arrow notation are voice, not filler. The scan must distinguish between them.

If your platform doesn't support a "scan before show" step, instruct the agent to perform the scan as the last step before any output — in the same turn as the draft, before it's emitted.

---

## Saving Drafts

On approval, write to:
```
{inbox}/{Recipient} {message-type} — YYYY-MM-DD.md
```

With frontmatter:
```yaml
---
type: draft-message
status: draft
recipient: Full Name or Role
date: YYYY-MM-DD
context: [one-line summary of what this is]
---
```

If the message is tied to a project, save in the project folder instead.

---

## Async Adaptation

If your platform can't pause for user input (fully automated pipeline, no human in the loop), the skill can be adapted:

1. Run intake inference from available context — infer as many parameters as possible, flag the rest as assumed
2. Draft with explicit intake-assumption disclosure at the top of the output
3. Send the draft to the user with the assumption list for review
4. Proceed on approval

This loses the back-and-forth sharpening but preserves the source-fidelity and filler-scan disciplines.

---

## Correction Handling

When the user corrects a phrase or asks for a revision:
1. Treat the correction as pointing at a pattern, not just the flagged line
2. Re-scan the whole draft for the same pattern
3. Fix in one pass
4. Deliver the revision — no apology, no explanation of what was wrong

"More concise" means the whole draft gets tightened. "Remove the filler opener" means every similar opener in the draft gets removed in the same pass.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — six-step drafting flow: intake → voice → context scan → draft → pre-show scan → show |
| **Agent** | Any agent with file read access and an intake gate mechanism |
| **Tools** | File read (required); file write (for saving drafts); no special tools needed |
