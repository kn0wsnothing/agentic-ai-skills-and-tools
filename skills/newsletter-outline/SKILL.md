# Newsletter Outline

You are running the Newsletter Outline skill. Your job is to build a structured outline for a newsletter-style essay through five steps: framing questions, source notes, overlap/attribution/callback checks, section-by-section drafting, and handoff.

**Scope:** The outline is the spine — beats, arguments, and narrative structure. Not prose. Don't write polished opening lines or draft-quality text. Commit to the argument and structure; the draft fills it in.

---

## Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| `NOTES_DIR` | `{workspace}/notes/` | Your writing archive — main notes, research, fleeting notes |
| `OUTLINES_DIR` | `{workspace}/outlines/` | Where outlines are saved |
| `PUBLISHED_DIR` | `{workspace}/published/` | Published pieces for overlap scan |
| `DRAFTS_DIR` | `{workspace}/drafts/` | In-progress drafts (also scanned for overlap) |
| `GUIDING_QUESTIONS_PATH` | _(optional)_ | Your editorial compass questions file |
| `STYLE_GUIDE_PATH` | _(optional)_ | Your writing style guide |
| `OUTLINE_TEMPLATE_PATH` | _(optional)_ | A template for new outline files |

---

## Before Starting

If `STYLE_GUIDE_PATH` is configured, read it before proceeding. Apply:
- **Structure blueprints** — use them to choose the right essay type (principle essay vs. framework/idea essay)
- **Emotional mechanics map** — use when planning narrative journey and section beats
- **Tone recipe** — use when writing the Foundation section

**Anti-AI voice standard for outline writing:** The outline sets the voice for the draft. AI-ish framing here is hard to fix later.
- Section descriptions must be specific, not importance-adjacent ("readers will understand the significance of X" → say what X actually *is*)
- Beat descriptions should name the actual claim or move, not describe the move in the abstract ("open with a paradox" is too generic — name the specific paradox)
- No hollow framing in section titles or beat notes
- Where the outline is uncertain about the argument, flag it as an open question rather than papering over it

---

## Trigger

User says:

- "outline [topic or title]"
- "let's outline [topic]"
- "start an outline"
- Handoff from process-notes ("ready to outline?")

---

## Guiding Questions (optional, touchstone)

If `GUIDING_QUESTIONS_PATH` is configured, read it before starting. These are your editorial compass questions — the "why publish this" filter for all content creation.

After collecting the framing answers (Step 1), identify which guiding question(s) this piece serves. Name it in the Foundation section: "This piece connects to: [guiding question]." If a piece doesn't clearly connect, flag it.

---

## Step 1 — Framing Questions (one at a time)

Ask **three questions**, one at a time. Wait for the answer before asking the next.

1. "Who is the specific reader you're writing to?"

2. "What do you want them to feel or do after reading?"

3. "What's the one sentence you want them to remember?"

---

## Step 2 — Source Note Suggestions

After collecting all three answers:

1. **Scan** `NOTES_DIR` for relevant notes
2. **Recommend** the most relevant notes based on topic and framing answers (3–5 max)
3. Show each with a one-line summary of why it's relevant
4. **Ask:** "These look relevant — confirm or suggest different ones?"

---

## Step 3 — Published Overlap Scan, Attribution, and Callback Map

Before drafting any sections, run all four checks. Present findings together for approval.

### 3a. Published overlap scan

1. **Identify the topic cluster(s)** this piece belongs to (from source notes and framing answers)
2. **Scan published pieces in `PUBLISHED_DIR`** and in-progress drafts in `DRAFTS_DIR` for overlapping material
3. **Flag overlapping material:** stories, arguments, metaphors, vignettes, or key examples that appear in both the source notes and published pieces
4. **For each overlap, recommend:** callback, reframe, or replace
5. **Show the overlap report.** Wait for decisions before proceeding.

### 3b. Source attribution log

If the outline adapts from a specific external source (a book, a translation, a framework by a named author):
- Note the attribution in the outline itself, in the relevant section beat
- Format: `[Adapted from: Author, Work]` inline with the beat description

### 3c. Callback map

1. **List which previous pieces** will be referenced in this piece
2. **Map each callback to a specific section** of the outline
3. **Check for adjacency collisions:** two callbacks to the same piece in consecutive sections
4. **Show the callback map.** Format: `Section → references [piece title] (callback / builds on / contrasts with)`

### 3d. Illustrative material verification

For each section's planned zoomed-in moment or illustrative example:
- **Check:** Does this example illustrate *this section's specific argument*, or just the piece's general theme?
- **Flag mismatches** and show them for the writer to decide.

**After all four checks are approved, proceed to Step 4.**

---

## Step 4 — Draft Outline (one section at a time)

**File path:** `OUTLINES_DIR/[title] - outline.md`

**Create the outline file on the first approved section** (with frontmatter + Foundation). Then **append each subsequent approved section to the file immediately** — do not wait until all sections are done.

For each outline section (~6 total: Foundation, Opening, Journey, Turn, Destination, Writing logistics):

1. **Draft and show the section** in full
2. **Ask:** "Approve, edit, or regenerate this section?"
3. **On approval** → **write/append to the file immediately**, then proceed to the next section
4. **On edit** → incorporate feedback and reshow

Draw on:
- Framing answers for the Foundation section
- Source notes for content in Journey, Turn, and Destination sections
- Blueprint structure (from style guide) for section ordering and narrative arc
- Emotional mechanics map for the types of moves planned per section

### Mandatory outline elements

**Epigraph (required, every essay):** Every outline must specify the *source and passage* the epigraph will draw from — a philosophical or textual anchor (e.g., "Tao Te Ching ch. 15, the muddy water passage") and where its payoff lands later in the piece. Don't write the polished epigraph prose at outline stage — that's draft work. But never leave the epigraph unspecified.

**Subheadings (required, every essay):** Each major section (Opening, Journey, Turn, Destination) should nominate provisional sentence-case subheadings — one per natural beat-cluster within the section. These guide the draft and ensure navigable structure. Subheadings are plain declaratives or questions in the writer's voice, not LinkedIn-style subtitles or punchy taglines. Final count: 5–8 across the published piece.

---

## Step 5 — Confirm and Handoff

Once all sections are approved (file already contains all sections):

1. **Confirm write** — send the file path as a standalone message (nothing else — no labels, no backticks, no surrounding text)

2. **Handoff prompt:** "Outline saved. When you're ready to write the draft, use newsletter-draft."

**Frontmatter:**
```yaml
---
type: outline
status: outline
topic: "[topic cluster]"
---
```

---

## Rules

- One question at a time; one section at a time
- Sentence case for all headings in the outline
- Filenames: `[title] - outline.md`
- Keep messages concise
- Write/append immediately on each section approval — don't batch
- Each skill is self-contained — assume no context from other sessions
