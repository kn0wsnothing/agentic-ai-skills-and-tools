---
name: newsletter-draft
description: >
  Use when the user says "draft [title]", "write the draft for [title]",
  "let's write the draft", or when handed off from newsletter-outline.
  Takes an approved outline and writes the full newsletter draft in
  chunks — intro, body sections, conclusion — applying the style guide
  throughout. One chunk at a time, user approval at each boundary.
---

# Newsletter Draft

Takes an approved outline and writes the full newsletter draft in sections — intro, body sections, conclusion. Each section is shown separately for approval before proceeding. The style guide is applied throughout.

## Before starting

Read the **full style guide** at `{STYLE_GUIDE_PATH}`.

Apply throughout drafting:
- **Voice identity and tone recipe** — every sentence
- **Sentence structure and rhythm rules** — length, punctuation, pacing
- **Vocabulary and word choice** — avoid/prefer list, banned phrases
- **Emotional mechanics map** — use specific moves intentionally
- **Rewrite operators** — apply when a chunk isn't landing
- **Preflight checklist** — before starting the draft
- **Postflight checklist** — before finalising

If `{PLATFORM_RULES_PATH}` is configured, also read that for platform-specific formatting rules.

---

## Trigger

- "draft [title]"
- "write the draft for [title]"
- "let's write the draft"
- Handoff from newsletter-outline: "take this to newsletter-draft"

---

## Step 1 — Find the outline

1. Search `{OUTLINES_DIR}` for the named outline file
2. If not found: ask the user to paste the outline directly
3. Read the full outline before drafting anything
4. Mark the outline as in-progress: update its frontmatter `status: outline` → `status: draft-in-progress`

---

## Step 2 — Preflight

Run the preflight checklist from the style guide before writing the first chunk. Note any specific risks for this piece (e.g., abstract topic → need more analogies; personal piece → concrete anecdotes required, not fabricated).

**Overlap and callback gate.** Check whether the outline includes a published-overlap scan and a callback map (cross-references to earlier issues). If either is missing, flag it before drafting:

> "This outline doesn't have a published overlap scan / callback map. Want me to run those checks now, or proceed as-is?"

If the user asks to run them: perform the checks (scan for topic overlap with published pieces, map any callbacks to earlier issues, confirm attribution for borrowed ideas), show results, get approval, then continue.

**No-fabrication rule.** Never invent personal anecdotes, specific stories, or details not present in the outline or source notes. If concrete material is missing, ask:

> "I don't have a real example for [section] — do you have one, or should I take a more principles-based angle?"

---

## Step 3 — Write in chunks (one at a time)

Write and show each chunk separately. Wait for approval before proceeding to the next.

**Draft file:** `{DRAFTS_DIR}/[title].md`

**Create the draft file on the first approved chunk** (with frontmatter + intro). Then **append each subsequent approved chunk immediately** — do not wait until all chunks are done. Use Read + Write, never a diff-only edit.

**Chunk order:**

1. **Intro** — epigraph (mandatory, every essay) + hook/paradox + first personal disclosure
   - The epigraph is a short translated quote from the piece's philosophical anchor
   - Place it as a blockquote between the title and the first paragraph
   - Attribute inline (e.g., *— Source, Work, ref.*)
   - Choose either: (a) a one-line interpretive reframe after the epigraph, or (b) standalone epigraph with a delayed payoff later in the piece. Never skip the epigraph.
   - Propose the intro's subheading along with the prose (the Opening section has no heading — the first subheading begins where the Journey starts)

2. **Body section 1** — with its subheading

3. **Body section 2** — with its subheading

4. *(continue per outline sections)*

5. **Conclusion + reader invitation**

**Subheadings — mandatory, every essay.** A 1,800–2,000 word essay needs 5–8 `## sentence case subheadings` breaking the body into navigable sections. Subheadings are plain declaratives or questions in the writer's voice, not LinkedIn-style subtitles or punchy taglines. Propose each subheading with its chunk.

**For each chunk:**
1. Write and show in full
2. Ask: "Approve, edit, or regenerate?"
3. On approval → write/append to the draft file immediately, then proceed to the next chunk
4. On edit → incorporate feedback and reshow

Apply rewrite operators from the style guide when a chunk needs restructuring.

---

## Step 4 — Postflight

Before finalising the draft, run the postflight checklist against the full file:

- **Epigraph** — present at top. Every essay must have one. If missing, add before finalising
- **Subheadings** — 5–8 sentence-case `## subheadings` throughout. If absent or fewer than 5, add before finalising
- **Sentence length** — any sentence over 40 words → flag for breaking
- **Personal voice** — at least 2 first-person imperfection disclosures per 800 words
- **Second-person moments** — at least 2 direct-address passages per 600 words
- **Banned phrases** — check against the style guide's banned list
- **Analogy check** — every technical concept has a concrete analogy
- **Closing tone** — final paragraph ends on a question or ongoing-process acknowledgment, not an imperative

Flag any failures before finalising:

> "Postflight flagged: [issue]. Fix now or proceed?"

---

## Step 5 — Finalise and hand off

Once all chunks are approved and postflight is clear:

1. Apply any postflight fixes to the draft file
2. Update the outline's status: `status: draft-in-progress` → `status: drafted`
3. Confirm the write — send the file path as a standalone message, then word count in the next message
4. Hand off:

> "Draft saved. Ready to package? Use newsletter-package for headline, subtitle, tags, and header image."

**Draft frontmatter:**

```yaml
---
tags:
note type: newsletter draft
status: draft
---
```

---

## Constraints

- Read the full style guide before every drafting session — not a summary, the full document
- One chunk at a time — never show multiple chunks simultaneously
- Never fabricate personal details — ask if source material is missing
- Sentence case for all headings in the draft
- Keep messages concise — the chunk is the content, not a preamble about the chunk
- Each skill is self-contained — assume no shared context from other sessions
