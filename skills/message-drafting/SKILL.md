---
name: message-drafting
description: >
  Draft outbound messages to a person — Slack, email, group chat, DM, written reply, talk track.
  Pins channel + audience + length + intent before any words. Always loads voice and style guides.
  Draws out the sender's actual position before drafting. Scans for filler before showing anything.
  Triggers: "draft a message", "draft a slack", "write an email", "help me message X",
  "what should I say to Y", "draft a DM", "draft a reply", "help me draft", "share my work".
---

# Message drafting

One skill for drafting outbound messages across any agent. Describe the operation; the agent uses its own read/write path.

## Purpose

Produce a message that:
1. Lands in the right register for the channel and recipient
2. Stays in the sender's voice
3. Says what the sender actually thinks — not synthesised positions, not LLM filler

The skill exists because without a gate, drafting defaults to vault-doc structure (headers, bullets, labelled sections) and LLM-pattern filler ("visibly," "spent the week," "upstream framing," "circle back"). Both are wrong for messages.

## Scope rules (non-negotiable)

- **Intake before drafting.** No words until channel + audience + length + intent + register are pinned.
- **Voice and style guide always.** Read before any draft.
- **Source-fidelity over synthesis.** When paraphrasing what someone said or what a position is, stay close to the actual transcript or note language. Quote where possible. Never invent a position for someone.
- **Match register to channel.** Slack DMs are short and direct. Emails are slightly more structured. Talk tracks are spoken cadence. Don't carry document structure into messages.
- **Pre-show scan before anything is shown.** Scan the draft against the filler list before the user sees it.

---

## Step 1 — Intake (mandatory gate before drafting)

Confirm or infer with high confidence:

1. **Channel** — Slack DM / Slack channel post / email / group chat (Discord, iMessage, WhatsApp) / talk track (spoken) / written reply / other
2. **Recipient(s)** — names or roles, relationship to sender
3. **Recipient context level** — what do they know already? what's new? what domain context can be assumed?
4. **Length cap** — Slack DM default 100–150 words; email default 200–400 words; talk track default 60–90 seconds spoken. Override if specified.
5. **Intent** — heads-up / ask / decision request / pushback / inform / coordinate / thank
6. **Register / tone** — formal, peer, casual, escalation, gentle, direct
7. **Anything to avoid** — third rails, sensitive threads, framings the sender has flagged before

If any of these is unclear and hasn't been pinned, ask one consolidated question. Don't draft on assumptions for high-stakes recipients.

**Draw out the sender's take.** If the sender hasn't stated their actual position on the message's subject, ask before drafting: what do you think? what's the ask? what outcome do you want? The draft leads from their take, not from a synthesised topic.

## Step 2 — Voice and style (always)

Read the sender's voice guide and style reference before drafting. At minimum:

- `{voice_guide}` — register, vocabulary, characteristic patterns, what to avoid
- Any style conventions file (sentence case, em-dash usage, salutation rules, etc.)

If no voice guide exists, ask the sender to describe their register in 2–3 sentences during intake.

**First person of whoever did the work.** A work-share of the sender's own analysis is "I looked at," "I read every item" — not a manufactured "we." Reserve "we" for the team/org as actor and collaborative asks.

## Step 3 — Context scan

Read whatever is relevant to the message subject. Don't paraphrase positions or events without reading the source.

| Source | When to read |
|--------|-------------|
| **Project notes** at `{projects}/{project}/` | Whenever the message touches a project. Read the main note, relevant briefs or plans, recent session log. |
| **People note** at `{people}/{recipient}.md` | Always for the recipient. Communication preferences, history, current relationship state. |
| **Today's notes** + last 2–3 days | Live state: today's focus, recent context, breadcrumbs. |
| **Meeting notes / transcripts** | If the message stems from or references a meeting. Read the relevant section before paraphrasing what anyone said. |
| **Memory / preferences file** | Check for relevant feedback, prior corrections, and standing preferences. |

If a meeting transcript has unreliable speaker attribution, prefer "follow up with X about Y" framing over attributed promises.

## Specialized format templates (optional)

If your organization uses channel-specific format specs (e.g., a particular Slack channel has its own share format, or internal announcements have a standard structure), read those as constraint sets before drafting. Treat them as constraints, not as something to explain.

## Step 4 — Draft

Apply voice + style guide. Match register. Stay close to source material when paraphrasing.

**One thing per sentence.** No nested clauses. No filler verbs. No corporate signposting.

**No structural over-framing.** Don't label sections ("Background," "Context," "Ask") in a Slack DM. Don't write headers in a short email. Match the channel's native shape.

**Synthesised position rule:** when summarising what someone said or believes, either (a) quote them directly from the source, or (b) paraphrase tightly with attribution ("she said the issue is X"). Never write declaratively about another person's view ("her position is X") unless they've said exactly that.

### Voice markers (casual / group chat register)

When channel is Slack DM, group DM, Discord, or iMessage, these patterns are part of natural voice — don't strip them in the filler scan:

- **Casual abbreviations:** "lmk", "atm", "imo", "tbh", "ish" — natural in DMs
- **Lowercase @-mentions** — match the platform's native casing
- **"basically"** as a connective shortcut
- **Slash compounds** for paired concepts: "visibility/communications", "when/how/if"
- **Arrow notation `->`** for cause/sequence
- **No closing salutation** — message ends after the last ask

Affirmative shape:

- **Action-first openers.** "I did X" not "Just wanted to update you that I did X"
- **Functional resource introduction.** Each link gets a noun phrase naming what it is, then the URL — no "you can find it here" framing.
- **Per-recipient closing asks.** In group messages, tag each person individually for what you want from them.
- **"I will [X] as I go"** for ongoing ownership — no "I'll try to" or "happy to" softening.
- **Sparing scope hedge.** "I don't think there's any time-pressure on X atm" defuses implied urgency without over-explaining.
- **Paragraph rhythm.** One topic per paragraph, free-standing. Comma-linked breath inside paragraphs is fine — don't chop into telegraphic single-clause lines.

**Implicit-frame rule:** don't make explicit what's already implicit between sender and receiver.
- Don't tell the recipient what's in their head ("you'd find this interesting," "this matters to you")
- Don't tell the recipient what their lane or day-to-day is — they know
- Don't restate the recipient's prior question before answering — they sent it
- Don't preface with "for context" / "for background" / "to be clear" — the body delivers the context
- Don't summarise at the end what the message just said
- Don't label what the message is ("this is a heads-up," "wanted to flag") more than once, and only if it genuinely clarifies action

## Step 5 — Pre-show scan (mandatory before user sees anything)

**Register check first.** If channel is a casual chat medium, consult the voice markers in Step 4 before stripping anything that looks like filler. Casual abbreviations, slash compounds, and arrow notation are voice, not filler.

Scan the full draft for the patterns below. If any one appears, scan the whole draft for similar patterns — not just the one line.

**Filler verbs / phrases to remove:**
- "visibly" / "publicly" / "spent the week [verb-ing]"
- "rhymes with" / "lines up with" (when used as flourish, not actual claim)
- "upstream framing" / "downstream of" (corporate-speak)
- "in your wheelhouse" / "your lane" (when projecting onto recipient)
- "we'll inevitably hit" / "circle back" / "loop in"
- "wanted to flag" used more than once
- "happy to" / "feel free to" / "just wanted to make sure"
- Any sentence that explains what the message is doing instead of doing it

**Structural over-framing to remove:**
- Bold section headers in short messages
- Labelled bullet sections ("State of play," "Open dependencies") — collapse into prose
- Numbered lists where 2–3 sentences would do
- Restating the recipient's question before answering

**Voice to verify:**
- Manufactured "we" on work the sender did alone
- Any sentence describing someone's view, position, or intent — confirm against source; if not in source, soften to a hedge or remove

**Implicit-frame projection to remove:**
- "For context, …" / "For background, …" / "To be clear, …"
- "You'd find this interesting" / "this matters to you"
- End-of-message summary recapping the body
- "This is a heads-up" / "wanted to flag" used more than once or as filler

**Specific complaint pattern:** if the user has flagged a phrasing problem already in this conversation, scan the whole draft for the same pattern, not just the one line.

## Step 6 — Show + iterate

Present the draft as the message itself — no labels around it ("Here's my draft:" → just show it).

When the user pushes back:
1. Treat the specific complaint as pointing at a pattern in the whole draft
2. Re-scan for the pattern
3. Fix in one pass, not iteratively
4. Deliver the revision — no apology, no performance

When the user says "save to workspace":
- Write immediately (no additional gate needed if user said to save)
- Default save location: `{inbox}/{recipient} {message-type} — YYYY-MM-DD.md` unless project-tied (then save to that project folder)
- Include frontmatter: `type: draft-message`, `status: draft`, `recipient`, `date`, `context`

## Rules

- **Intake before drafting.** Always.
- **Voice + style guide every time.** Don't skip.
- **Source-fidelity over synthesis.** Quote or attribute; don't invent positions.
- **Match register to channel.** Slack ≠ doc.
- **Scan for filler before showing.** Every time.
- **One pattern fix scans the whole draft, not just one line.**
- **No emojis unless explicitly requested.**
- **No "I hope this finds you well" / "happy to" / "feel free to" filler.**

## Related files (configure for your setup)

- `{voice_guide}` — voice canon
- `{style_reference}` — style conventions
- `{people}/{recipient}.md` — people notes
- `{projects}/` — project notes
