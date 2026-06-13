# Newsletter Package

A pre-publish packaging skill for newsletters. Takes a finished draft and works through headline, subtitle, tags, and header image one element at a time — collaborative brainstorm style. Closes by stamping metadata on the draft file and logging decisions for future reference.

Upstream skills: [newsletter-outline](../newsletter-outline/) → [newsletter-draft](../newsletter-draft/) → **newsletter-package**

---

## What it does

Newsletter packaging is a distinct creative act from writing. The headline has to be independent of the body — short, front-loaded, not clickbait. The subtitle adds scope. The tags connect to distribution. The image captures the emotional arc, not the literal topic. Getting these right takes iteration. This skill structures that iteration and keeps the history.

It runs 9 steps in sequence, but only one step at a time — the human drives the pace, picks from options, and moves on.

---

## The steps

1. **Read references** — voice guide, visual brand identity, packaging log, tag library
2. **Find and read the draft** — understand the argument, key metaphors, emotional arc
3. **Headline brainstorm** — 5–7 options tagged by angle; one round at a time until locked
4. **Subtitle** — 2–3 options that complement the headline, not repeat it
5. **Tags** — 3–5 from your tag library, with brief reasoning for each
6. **Image concept** — discuss the visual direction, propose 2–3 concepts
7. **Image generation** — write a prompt, get approval, generate; iterate on feedback
8. **Update draft frontmatter** — stamp headline, subtitle, tags, image metadata
9. **Log decisions** — append to the packaging log for pattern avoidance next time

After the issue ships, the skill handles the post-publish closure: status updates, file move to the published folder, publication log entry, handoff to the social-posting skill.

---

## Prerequisites

Your agent needs:

- **File read/write access** to your drafts folder and reference files
- **Image generation access** — to a model or API that can generate header images
- **A voice guide** — your publication's voice identity, anti-patterns, tone rules
- **A visual brand identity doc** — image style, motifs, negative prompts, palette
- **A packaging log** — append-only record of past decisions (created on first run)
- **A tag library** — your distribution platform's canonical tag set

---

## Quick start

1. Finish your draft and put it in your drafts folder
2. Copy `SKILL.md` into your agent's instructions
3. Configure the paths in the Configuration table (see SKILL.md)
4. Trigger the skill: `package [title]`, `headline for [title]`, or ask it to continue from `newsletter-draft`
5. Work through each step — the agent proposes, you pick or redirect

---

## Configuration

| Parameter | Description |
|-----------|-------------|
| `{voice_guide}` | Your publication's voice identity and anti-pattern rules |
| `{brand_identity}` | Visual style guide — image aesthetic, motifs, palette, negative prompts |
| `{packaging_log}` | Append-only log of past headline/image decisions |
| `{tag_library}` | Your distribution platform's canonical tag set |
| `{drafts_dir}` | Where draft notes live |
| `{published_dir}` | Where published notes live (for post-publish move) |
| `{publication_log}` | Publication log file — the record of shipped issues |

---

## Best practices

**Check the packaging log before starting.** Recent headline structures and image motifs are in there. Pattern repetition kills the feel of a consistent publication.

**Approve the image prompt before generating.** Image generation is expensive and hard to reverse. The explicit prompt approval step exists for a reason.

**The packaging log compounds over time.** Early entries are sparse. By issue 20, you have a record of what angles have been tried, which motifs are overused, which headline structures your audience responds to. Keep it.

**Do post-publish closure in the same session if you can.** Stamping the frontmatter and logging the published headline while the session is live prevents drift between what shipped and what the vault says shipped.

---

## Platform examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic (any agent with file I/O and image generation) | [examples/generic.md](examples/generic.md) |

---

## Design rationale

See [SPEC.md](SPEC.md) for the design decisions: why packaging is a separate skill from drafting, why the image prompt requires explicit approval, and how the packaging log and publication log serve different purposes.
