# Newsletter Draft — Generic Setup

This guide covers how to wire the newsletter-draft skill to any agent platform with file read/write access.

---

## Prerequisites

- An agent with file read/write access to your workspace
- An approved outline (from newsletter-outline or hand-written)
- A style guide at a stable path your agent can read
- A drafts directory where output files will be written

---

## Minimum Configuration

The skill needs four paths. Set them in your agent's system prompt or instruction file:

```
STYLE_GUIDE_PATH: path/to/your-style-guide.md
PLATFORM_RULES_PATH: path/to/platform-rules.md  # optional
OUTLINES_DIR: path/to/outlines/
DRAFTS_DIR: path/to/drafts/
```

Replace these placeholders wherever they appear in `SKILL.md`.

---

## Triggering the Skill

### Direct trigger

```
Draft the newsletter titled "[title]".
```

```
Let's write the draft. The outline is at outlines/[slug].md.
```

### From newsletter-outline handoff

If you use the newsletter-outline skill, it ends with a handoff prompt. Reply to continue:

```
Yes, draft it.
```

The agent locates the outline by title and proceeds.

---

## What the Agent Does

**Step 1 — Outline.** Finds the outline file by title. If not found, asks for a paste. Reads the full outline. Updates its status frontmatter to `draft-in-progress`.

**Step 2 — Preflight.** Reads the full style guide. Checks for a published-overlap scan and callback map in the outline. Flags missing source material. If anything is missing it asks — it does not proceed silently.

**Step 3 — Chunked drafting.** Writes one section at a time:
- Intro (epigraph + hook + first personal disclosure)
- Each body section with its subheading
- Conclusion + reader invitation

After each section: shows the chunk, asks "Approve, edit, or regenerate?". On approval: saves immediately to the draft file, then moves to the next chunk. On edit: incorporates feedback and reshows. On regenerate: rewrites the section from scratch.

**Step 4 — Postflight.** Reads the full completed draft. Checks: epigraph present, 5–8 subheadings, sentence length, first-person voice, second-person moments, banned phrases, closing tone. Flags any failures and asks whether to fix before finalising.

**Step 5 — Finalise.** Applies postflight fixes. Updates outline status to `drafted`. Confirms with file path and word count. Prompts handoff to newsletter-package.

---

## Outline Requirements

For best results, the outline should include:

| Element | Why it's needed |
|---------|----------------|
| Topic and reader framing | Grounds the voice and level of abstraction |
| Section beats | Tells the agent what each section argues, not just its topic |
| Epigraph source | Required — agent will ask if missing |
| Provisional subheadings | Seeded from the outline; refined during drafting |
| Published-overlap scan | Flagged at preflight if absent |
| Callback map | Flagged at preflight if absent |
| Source notes / raw material | Prevents fabrication requests mid-draft |

---

## Style Guide Requirements

The style guide is the single most important configuration. The agent reads it before the first sentence of every session and applies it throughout. At minimum, it should include:

- Voice identity — how the author sounds, not just what they avoid
- Tone recipe — the emotional register for this newsletter
- Sentence structure rules — length, rhythm, punctuation patterns
- Vocabulary — avoid/prefer pairs, banned phrases and constructions
- Emotional mechanics — named moves the author uses intentionally
- Rewrite operators — what to do when a chunk isn't landing
- Preflight checklist — what to check before the first sentence
- Postflight checklist — what to audit on the completed draft

Without a style guide, the agent defaults to generic competent prose. The style guide is what makes the output sound like the author.

---

## Draft File Format

The output file is created at `{DRAFTS_DIR}/[title].md` with frontmatter:

```yaml
---
tags:
note type: newsletter draft
status: draft
---
```

Adjust the frontmatter to match your own conventions.

---

## Resuming a Mid-Draft Session

If a session ends mid-draft, the approved chunks are already saved. In the next session:

```
Continue the draft for "[title]" — [section] is already written.
```

The agent reads the existing file and picks up from the last approved section.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — 5-step chunked drafting pipeline |
| **Agent** | Any agent with file read/write and sufficient context window |
| **Tools** | File read, file write (full file rewrite — not append-only), file search |
