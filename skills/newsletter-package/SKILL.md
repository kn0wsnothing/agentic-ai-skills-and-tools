# Newsletter Package

You are running the Newsletter Package skill. Your job: take a finished newsletter draft and brainstorm the packaging — headline, subtitle, tags, header image — one element at a time in collaborative style. Then stamp the draft with the chosen metadata, log the decisions, and handle post-publish closure when the issue ships.

---

## Before starting (MANDATORY — do not skip)

Read ALL of these references before generating any headlines, subtitles, or image prompts. Do not start brainstorming until all four are loaded:

1. **Voice guide** (`{voice_guide}`) — voice identity, anti-patterns, tone rules. Every headline and subtitle must pass the anti-formula voice check.
2. **Visual brand identity** (`{brand_identity}`) — image style, motifs, palette, negative prompts.
3. **Packaging log** (`{packaging_log}`) — past headline structures and image choices. Create the file if it doesn't exist.
4. **Tag library** (`{tag_library}`) — your distribution platform's canonical tag set.

Confirm to the user: "Read voice guide, brand identity, packaging log, and tag library. Ready to start."

---

## Triggers

- `package [title]`
- `headline for [title]`
- `image for [title]`
- Handoff from newsletter-draft: "ready to package?"

---

## Step 1 — Find and read the draft

Search `{drafts_dir}` for the draft file. Read it in full before brainstorming. Understand the core argument, key metaphors, and emotional arc.

---

## Step 2 — Headline brainstorm

Generate 5–7 headline options, each tagged by angle:

- `[curiosity]` — gap or question that pulls the reader in
- `[declarative]` — bold claim or thesis statement
- `[question]` — direct question the piece answers
- `[contrarian]` — challenges a common assumption
- `[metaphor]` — uses an image or analogy from the piece
- `[minimal]` — short, evocative, 3–5 words

Show all at once. User picks, edits, combines, or asks for another round.

**Constraints:**
- No clickbait
- Match the publication voice — understated, precise, practitioner
- Front-load the interesting thing
- Under 10 words ideal
- Check the packaging log for patterns — avoid repeating structures from recent headlines

**Ask:** "Pick one, mash a few together, or want another round?"

---

## Step 3 — Subtitle

Generate 2–3 subtitle options that complement the chosen headline.

**Constraints:**
- Add context or scope — do not repeat the headline
- One sentence, under 20 words
- Conversational but specific

**Ask:** "Pick one, edit, or skip subtitle?"

---

## Step 4 — Tags

Read the tag library from `{tag_library}`.

- Recommend 3–5 tags based on topic fit
- Brief reasoning for each (one line)
- May suggest new tags if none fit — user creates them in their distribution platform

**Ask:** "These tags work, or adjust?"

---

## Step 5 — Image concept

Read `{brand_identity}` and review the packaging log for recent image choices.

- Discuss the visual concept: "What feeling or metaphor from this piece should the image capture?"
- Propose 2–3 visual concepts with one-line descriptions
- Note which motifs were used recently — avoid repetition

**Ask:** "Which direction, or something else entirely?"

---

## Step 6 — Generate image

**MANDATORY: Do not generate any image until the prompt is explicitly approved by the user.**

Process:
1. Describe the visual concept in plain language (2–3 sentences): what the image shows, the mood, what makes it distinctive
2. Write the full generation prompt following the rules in `{brand_identity}` — include style, motifs, negative prompt, palette, paper or surface characteristics
3. Ask: "Happy with this prompt, or want to adjust before generating?"
4. Only run the generation after explicit approval

**Aspect ratio:** 16:9 (standard newsletter header)

**Iterate based on feedback, not visual inspection.** The user sees the image and tells you what to change ("too warm", "too busy", "path should go left"). Adjust the prompt based on their words.

**Ask:** "Happy with this, or want to try a variation?"

---

## Step 7 — Update draft frontmatter

Add packaging metadata to the draft file:

```yaml
headline: "Chosen headline"
subtitle: "Chosen subtitle"
distribution_tags: [tag1, tag2, tag3]
header_image: "{image path or URL}"
image_prompt: "The prompt used"
```

---

## Step 8 — Log decisions

Append to `{packaging_log}`:

```markdown
## YYYY-MM-DD — [title]

**Headline:** [chosen]
**Rejected headlines:** [brief list]
**Subtitle:** [chosen]
**Tags:** [list]
**Image concept:** [one-line description]
**Image prompt:** [full prompt]
**Notes:** [what worked, what didn't, visual direction feedback]
```

If visual brand learnings emerged, append a note to the brand identity doc under `## Iteration log` — create the section if it doesn't exist.

---

## Step 9 — Handoff

Tell the user: "Package complete. Download the header image, publish in your distribution platform, then use social-posting for promo."

Send the draft file path as a standalone message.

---

## After publishing (when user confirms it's live)

When the user returns to confirm the newsletter has been published:

1. **Get the publish date** — ask if not already stated.

2. **Stamp the draft frontmatter** before moving it:
   - `status: published` (was `draft`)
   - `published: YYYY-MM-DD` (the live publish date)
   - Confirm `headline:` holds the headline that actually shipped

3. **Move draft to published folder** — from `{drafts_dir}` to `{published_dir}`

4. **Append a row to the publication log** (`{publication_log}`), newest first:

   ```
   | YYYY-MM-DD | <shipped headline> | <vault filename> | published |
   ```

   The shipped headline and vault filename often differ — record both so neither has to be guessed later.

5. **Confirm the move** — send the new path as a standalone message.

6. Remind: "Ready for promo posts? Use social-posting."

---

## Configuration

| Parameter | Description |
|-----------|-------------|
| `{voice_guide}` | Your publication's voice identity and anti-pattern rules |
| `{brand_identity}` | Visual style guide — image aesthetic, motifs, palette, negative prompts |
| `{packaging_log}` | Append-only log of past packaging decisions |
| `{tag_library}` | Your distribution platform's canonical tag set |
| `{drafts_dir}` | Where draft notes live |
| `{published_dir}` | Where published notes live (for post-publish move) |
| `{publication_log}` | Record of all shipped issues |

---

## Notes

- One element at a time — headline, then subtitle, then tags, then image
- Check the packaging log before every session to avoid repeating patterns
- Keep messages concise
- Each skill is self-contained — assume no context from other sessions
- Image generation requires explicit prompt approval every time — this is not skippable
