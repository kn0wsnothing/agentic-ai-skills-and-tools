# Newsletter Package — Generic agent

How to wire up the newsletter-package skill on any agent that has file read/write access and image generation capability. Adapt tool names and invocation patterns to your platform.

---

## Minimum requirements

Your agent needs:

- **File read/write** — to read drafts and reference files, update frontmatter, move files, append to logs
- **Image generation access** — to a model or API that generates images from text prompts (DALL-E, Midjourney, Stable Diffusion, or similar)
- **A voice guide** — your publication's voice identity and anti-pattern rules
- **A visual brand identity doc** — image style, motifs, palette, negative prompts
- **A packaging log** — append-only record; create it on first run

Optional but useful:
- **Multi-file read** — speeds up loading the four reference files before brainstorming
- **File move / rename** — needed for post-publish closure (draft → published folder)

---

## What to create

### Reference files

Create these four files before running the skill for the first time:

**Voice guide** — write out what your publication's voice sounds like. Anti-patterns are as important as positive rules. Include a few headline examples that pass and a few that don't. This is what the agent checks every headline against.

**Visual brand identity** — your image aesthetic in prompt-ready language. Include: preferred style (e.g., ink-wash, flat illustration, photography), motifs you return to, color palette, surface/texture characteristics, negative prompts (what to exclude), and an `## Iteration log` section the skill will append to over time.

**Packaging log** — start with an empty markdown file. The skill appends to it after every session.

**Tag library** — your distribution platform's canonical tag set. Export or copy from your platform.

### Folder structure

The skill expects distinct drafts and published folders:

```
{workspace}/
├── content/
│   ├── drafts/          # {drafts_dir}
│   ├── published/       # {published_dir}
│   └── reference/
│       ├── voice-guide.md           # {voice_guide}
│       ├── visual-brand-identity.md # {brand_identity}
│       ├── packaging-log.md         # {packaging_log}
│       ├── tag-library.md           # {tag_library}
│       └── publication-log.md       # {publication_log}
```

Adjust to your actual structure. The placeholders in SKILL.md map to whatever paths you configure.

---

## Loading the skill

The newsletter-package skill is natural-language instructions in `SKILL.md`. Load it by:

1. **Pasting into your system prompt** — self-contained; this works for most platforms
2. **Referencing by path** — if your agent loads from a skills directory automatically
3. **Triggering by name** — if your platform supports named skills or slash commands

Replace the placeholder parameters with real paths, either in your SKILL.md copy or in your system prompt:

```
When running the newsletter package, use these paths:
- {voice_guide} = content/reference/voice-guide.md
- {brand_identity} = content/reference/visual-brand-identity.md
- {packaging_log} = content/reference/packaging-log.md
- {tag_library} = content/reference/tag-library.md
- {drafts_dir} = content/drafts/
- {published_dir} = content/published/
- {publication_log} = content/reference/publication-log.md
```

---

## Image generation integration

The skill writes a detailed generation prompt and asks for explicit approval before running. How you connect generation depends on your platform:

| Setup | Approach |
|-------|----------|
| **Agent with built-in image tools** | The agent calls its image generation tool directly after approval |
| **External API (DALL-E, etc.)** | Agent assembles the prompt, you copy it to the API yourself, paste the result back |
| **Local model (Stable Diffusion, etc.)** | Agent writes the prompt to a file; you run the generation and confirm |
| **No image generation** | Skip Steps 5–7; handle image separately; update frontmatter manually |

The prompt approval step (Step 6) is mandatory regardless of how you generate the image. It exists because generation is expensive and output can't be verified by the agent — the human has to see it and direct iteration.

---

## Post-publish closure

When your issue ships, tell the agent the publish date. The skill will:

1. Stamp the draft frontmatter (`status: published`, `published: YYYY-MM-DD`)
2. Move the draft file to `{published_dir}`
3. Append a row to `{publication_log}`: `| YYYY-MM-DD | shipped headline | filename | published |`

If your platform can't move files (read/write only), the skill can update the frontmatter in place and log the move as a manual step for you to complete.

---

## Triggering

Standard trigger phrases:

- `package [title]`
- `headline for [title]`
- `image for [title]`
- Handoff from newsletter-draft: "ready to package?"

For post-publish closure:

- `Published. It went live on [date].`
- `It's live — [date].`

---

## WAT mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — 9-step packaging pipeline + post-publish closure |
| **Agent** | Any agent with file read/write and image generation access (strong model recommended — packaging requires voice judgment) |
| **Tools** | File read/write, image generation API or tool, optional file move |

---

## Adapting for platforms without file move

If your agent can't move files, post-publish closure looks like this:

1. Stamp the frontmatter in the draft file (`status: published`, `published: YYYY-MM-DD`)
2. Append to the publication log
3. Tell you: "Please move `{filename}` to `{published_dir}` manually."

The record is still accurate. The physical move is the one manual step.

---

## Adapting for agents without persistent files

If your agent doesn't have a persistent file system (e.g., a stateless API), the packaging log and publication log need to live somewhere the agent can read and write across sessions — a Git repo, a shared doc, a database, or a notes tool. The skill's logic is the same; the storage layer is yours to choose.

The reference files (voice guide, brand identity, tag library) can be pasted into the system prompt if you don't have persistent file access. Update them manually as your brand evolves.
