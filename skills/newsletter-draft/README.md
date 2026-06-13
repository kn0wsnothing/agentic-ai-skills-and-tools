# Newsletter Draft

A guided chunked-drafting skill for AI agents. Takes an approved outline and writes the full newsletter draft in sections — one at a time, with user approval at each boundary.

Part of a three-skill content pipeline: [newsletter-outline](../newsletter-outline/) → **newsletter-draft** → [newsletter-package](../newsletter-package/)

---

## What It Does

Five steps, run in sequence:

1. **Find the outline** — locates the approved outline or accepts a paste; marks it as in-progress
2. **Preflight** — runs a style check before writing; flags missing source material
3. **Chunked drafting** — writes intro, each body section, and conclusion separately; shows each chunk for approval before proceeding
4. **Postflight** — audits the completed draft against structural requirements (epigraph, subheadings, sentence length, voice)
5. **Finalise and hand off** — saves the draft, updates outline status, prompts handoff to newsletter-package

---

## Key Principles

**One chunk at a time.** Each section is shown, approved (or edited and reshown), then saved. The agent never runs ahead. Never shows two chunks simultaneously.

**No fabrication.** The agent does not invent personal anecdotes, examples, or details not present in the outline or source notes. If concrete material is missing, it asks — not guesses.

**Save on approval.** Each approved chunk is written to the draft file immediately. Not batched at the end. If the session ends mid-draft, whatever was approved is already saved.

**Postflight is mandatory.** The agent checks the full draft before handing off — sentence length, first-person voice presence, banned phrases, structural completeness. It surfaces failures before finalising.

**Style guide is non-negotiable.** The agent reads your style guide before the first sentence and applies it throughout. Every chunk.

---

## Prerequisites

- **File read/write access** to your workspace
- **An approved outline** from the newsletter-outline skill (or a pasted outline)
- **A style guide** at a known path — see configuration
- **A drafts folder** where the output file will be created

---

## Workspace Layout

The skill uses these paths, configurable via `{workspace}`:

```
{workspace}/
├── Outlines/                     # Approved outlines from newsletter-outline
├── Drafts/                       # Output — draft files written here
└── Voice/                        # Style guide and platform rules
    ├── style-guide.md            # Personal writing style guide
    └── platform-rules.md         # Platform-specific formatting rules (optional)
```

Adapt paths to your own structure — the skill references them as configurable in SKILL.md.

---

## Configuration

| Parameter | Description |
|-----------|-------------|
| `WORKSPACE` | Root path of your note workspace |
| `OUTLINES_DIR` | Directory containing approved outlines |
| `DRAFTS_DIR` | Directory where draft files are written |
| `STYLE_GUIDE_PATH` | Path to your personal writing style guide |
| `PLATFORM_RULES_PATH` | Optional path to platform-specific voice/format rules |

---

## Quick Start

1. **Complete an outline** with the newsletter-outline skill, or prepare an outline in the expected format
2. **Copy `SKILL.md`** into your agent's persistent instructions, configuring the paths above
3. **Add your style guide** at `STYLE_GUIDE_PATH` — the agent loads it before every drafting session
4. **Trigger:** say "draft [title]" or "let's write the draft"

---

## Outline Format

The skill expects the outline to include:

- **Topic and reader framing** — who is this for, what should they feel or do
- **Section beats** — each major section with its argument and narrative moves
- **Epigraph specification** — source and passage for the opening quote
- **Provisional subheadings** — one per major beat cluster

If the outline is missing an overlap scan or callback map, the agent will flag it before drafting begins.

---

## Output

A draft file at `DRAFTS_DIR/[title].md` with frontmatter:

```yaml
---
tags:
note type: newsletter draft
status: draft
---
```

The outline's status is updated `outline` → `draft-in-progress` at the start, then `draft-in-progress` → `drafted` when finalised.

---

## Structural Requirements (enforced at postflight)

**Epigraph** — mandatory, every essay. A short translated quote from the piece's philosophical anchor, placed as a blockquote before the first paragraph, with inline attribution.

**Subheadings** — 5–8 sentence-case `## subheadings` breaking the body into navigable sections. Plain declaratives or questions in the writer's voice. Not LinkedIn-style subtitles.

**Sentence length** — any sentence over 40 words is flagged for breaking.

**First-person imperfection** — at least 2 disclosures per 800 words.

**Second-person moments** — at least 2 direct-address passages per 600 words.

**Closing tone** — final paragraph ends on a question or ongoing-process acknowledgment, not an imperative.

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic | [examples/generic.md](examples/generic.md) |

---

## Pipeline

This skill sits in the middle of a three-skill content pipeline:

```
[newsletter-outline]  →  [newsletter-draft]  →  [newsletter-package]
  Topic → outline          Outline → draft        Draft → packaged issue
```

**Upstream:** newsletter-outline produces the approved outline this skill consumes. If the outline is missing required elements (epigraph spec, callback map), newsletter-draft will flag it.

**Downstream:** newsletter-package takes the completed draft and produces the packaged issue — headline, subtitle, tags, header image prompt, and publish-ready formatting.
