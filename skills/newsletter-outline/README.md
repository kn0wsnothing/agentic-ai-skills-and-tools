# Newsletter Outline

A structured outline skill for newsletter-style essays. Builds the spine — beats, narrative arc, and section structure — through framing questions, source-note curation, overlap and callback checks, and section-by-section drafting. Output is a saved outline ready to hand off to newsletter-draft.

The outline is scaffold, not prose. The job here is to commit to the argument, the reader, and the structure. The draft fills it in.

---

## How It Works

Five steps in sequence:

1. **Framing questions** — reader, desired outcome, one-sentence takeaway
2. **Source note suggestions** — relevant notes from your writing archive, confirmed before proceeding
3. **Overlap / attribution / callback scan** — published-piece overlap, source attribution, callback map, and illustrative-material check
4. **Section-by-section outline drafting** — one section at a time, each approved before writing the next
5. **Handoff** — file saved, ready for newsletter-draft

---

## Prerequisites

Your agent needs:

- **File read/write access** — to save the outline to persistent storage
- **A writing archive** — notes you've accumulated on this topic ({workspace}/notes/ or equivalent)
- **A published-pieces index** — a directory of past published work to scan for overlap
- **A guiding-questions file** — your 11 (or however many) editorial compass questions (optional but recommended — see Usage)
- **A style guide** — your voice and structure reference (optional but recommended)

---

## Workspace Structure

```
{workspace}/
├── notes/                   # Source notes — main notes, fleeting notes, research
├── outlines/                # Where outlines are saved
├── drafts/                  # In-progress drafts
├── published/               # Published pieces (for overlap scan)
└── style-guide.md           # Your writing style guide (optional)
```

Adjust paths in SKILL.md to match your setup.

---

## Quick Start

1. **Copy `SKILL.md`** into your agent's persistent instructions (CLAUDE.md, `.cursor/rules`, system prompt, etc.)
2. **Configure the paths** in the Configuration section
3. **Trigger it** — say "outline [topic or title]" or "let's outline [topic]"
4. **Answer the three framing questions** — one at a time, think carefully
5. **Confirm source notes**, approve the overlap scan, approve each section

---

## Usage

**Trigger phrases:**
- "outline [topic or title]"
- "let's outline [topic]"
- "start an outline"
- Handoff from process-notes: "ready to outline?"

**Guiding questions:** If you maintain a file of editorial compass questions ("what is the one thing I want readers to do differently?", etc.), configure `GUIDING_QUESTIONS_PATH`. The skill will identify which question this piece serves and anchor it in the Foundation section.

**Epigraph:** Every outline nominates an epigraph source (a philosophical or textual anchor — a specific passage, not the polished prose). The draft writes the epigraph; the outline specifies where it comes from and where its payoff lands.

**Subheadings:** The outline nominates provisional sentence-case subheadings for each section. These are plain declaratives in your voice, not taglines. They guide the draft and ensure navigable structure.

---

## Pairs With

This skill is part of a three-skill content pipeline:

| Skill | Role |
|-------|------|
| **newsletter-outline** ← you are here | Spine — beats, argument, structure |
| [newsletter-draft](../newsletter-draft/) | Prose — writing the sections from the outline |
| [newsletter-package](../newsletter-package/) | Ship — final checks, metadata, publish |

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic | [examples/generic.md](examples/generic.md) |

---

## Design Rationale

See [SPEC.md](SPEC.md) for the reasoning behind the framing-first method, the overlap scan, and the one-section-at-a-time approach.
