# Message Drafting — Design Spec

This document explains the design decisions behind the Message Drafting skill.

---

## The Problem

AI agents are bad at messages by default. Left unconstrained, they produce:

1. **Document structure in message form.** Headers, labelled sections, bullet clusters — the shape of a brief, dropped into a 100-word Slack DM. This reads as formal when casual is expected, and verbose when direct is required.

2. **Synthesised positions without source-grounding.** An agent summarises what someone said in a meeting and gets it subtly wrong — flipping the framing, rounding to the nearest plausible view, or inventing a position that wasn't stated. The sender sends it. The recipient is confused or annoyed.

3. **Filler patterns.** "Visibly," "spent the week," "rhymes with," "upstream framing," "circle back," "happy to," "just wanted to flag." These are LLM tells — phrases that appear in AI-generated text without appearing in natural human writing. Recipients notice them even if they can't name them.

4. **Register mismatch.** Email formality in a chat DM, or chat compression in a message that needed a full context paragraph. The agent picks a default register and applies it regardless of channel.

5. **Implicit-frame projection.** Telling the recipient what's in their head ("you'd find this interesting"), what their lane is ("this is more in your wheelhouse"), or restating their question back to them before answering. This is condescending in a professional context and reads as AI-written.

All five are correctible in review — but they take multiple rounds. The skill prevents them at the source by gating intake, anchoring to source material, and scanning before show.

---

## Why Intake Is a Hard Gate

The natural instinct is to start drafting as soon as a topic is named. The problem is that every parameter that isn't pinned becomes an assumption — and wrong assumptions compound.

The most common failures come from unpinned channel (wrong register), unpinned context level (wrong amount of explanation), and unpinned intent (wrong ask). A single consolidated intake question before drafting eliminates all three in one round.

The intake gate also surfaces the sender's actual take. Most drafting requests name a topic but not a position. The agent that dives straight to draft has to invent the position. The agent that asks "what do you think, what's the ask?" gets the real one. The draft that leads from the sender's own take is shorter, sharper, and truer.

---

## Why Voice Guides Are Read Every Time

Voice calibration degrades without a reference. An agent that's been doing data analysis or writing reports brings that register into a message draft unless it re-reads the guide. The discipline of "read voice guide before every draft" is a forcing function — it costs two seconds and prevents the most common tone failure.

If no voice guide exists, the intake step asks for a 2–3 sentence description of the sender's register. This is less precise than a guide, but far better than default LLM voice.

---

## Source-Fidelity vs. Synthesis

The skill has a strong rule: when paraphrasing someone's view, either quote them directly or paraphrase tightly with attribution. Never write declaratively about someone's position unless they've said exactly that.

This rule exists because synthesis failure is the hardest to catch and the most damaging. An agent can synthesise something plausible-sounding that's the opposite of what was actually said — and both the sender and reviewer will miss it because it sounds right. The only protection is staying close to the source text.

Practical implication: if the message references a meeting, the agent reads the meeting notes before drafting the section that references the meeting. If the meeting notes don't exist, the agent asks rather than infers.

---

## The Filler Pre-Show Scan

The pre-show scan was added after a documented failure where 4+ rounds of correction were needed to strip LLM-pattern phrases from a single Slack message. The correction pattern revealed that each flagged phrase was a symptom of a broader register problem — fixing one line at a time was slower than scanning the whole draft at once.

The scan is mandatory and happens before the user sees anything. This means filler never reaches the user — they never have to ask for it to be removed.

The scan also protects casual voice markers in DM channels. Words like "lmk," "tbh," and "basically," and patterns like slash compounds ("visibility/communications") and arrow notation ("recommendation->action") are natural in group chat. A scan that strips them is a register failure, not a cleanup.

---

## The Implicit-Frame Rule

The implicit-frame rule is unusual and worth explaining. The rule prohibits telling the recipient things they already know about themselves or their context:

- "You'd find this interesting" — projects onto the recipient's interest
- "This is more in your wheelhouse" — projects their domain
- "Just to set the scene" — the scene is already set; if they're receiving the message, they have context
- End-of-message summary — the message just said it; the recipient read it

These patterns appear in AI-generated messages because the agent is writing for a generic recipient and filling in assumed context. They're condescending in a professional context and read as AI-written. The rule removes them structurally rather than waiting for user feedback.

---

## The Pattern-Fix Rule

When a user corrects a specific phrase, the skill re-scans the whole draft for the same pattern. This rule was added after observing that partial corrections — fixing the flagged line without scanning — produced messages that still contained the same issue elsewhere. One correction round should clear the whole draft, not just the flagged instance.

The same rule applies to compound corrections: "be more concise" means the whole draft gets tightened, not the paragraph the user happened to point at.

---

## Register Calibration Table

| Channel | Default length | Register | Voice markers protected |
|---------|---------------|----------|------------------------|
| Slack DM | 100–150 words | Direct, peer | Abbreviations, slash compounds, arrow notation, no closing salutation |
| Slack channel post | 150–300 words | Peer to slight-formal | Action-first opener, functional resource introduction |
| Email | 200–400 words | Slight-formal | Sentence case headings if headers are needed, no "I hope this finds you well" |
| Group DM (Discord, iMessage) | 50–150 words | Casual | All DM markers plus emoji if channel norm |
| Talk track (spoken) | 60–90 sec | Spoken cadence | Short sentences, natural pause points, no structural bullets |
| Written reply | Match original | Mirror sender | Match the register of the message being replied to |

---

## What This Skill Is Not

This skill does not handle:

- Long-form publishing (newsletter issues, essays, reports)
- Formal documents with structured sections
- Content that will be published without a human recipient (social posts, blog posts)

Use a separate drafting workflow for those. Message drafting is specifically for direct human-to-human communication in a professional context.
