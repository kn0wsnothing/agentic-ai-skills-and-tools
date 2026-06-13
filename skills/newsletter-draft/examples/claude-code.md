# Newsletter Draft — Claude Code

## Setup

### 1. Add to CLAUDE.md

```markdown
### Newsletter Draft
When asked to draft a newsletter (or on handoff from newsletter-outline),
follow the instructions in `skills/newsletter-draft/SKILL.md`. Writes the
full draft in chunks — one section at a time, approved before proceeding.
```

### 2. Copy the skill

```bash
mkdir -p skills/newsletter-draft
cp /path/to/SKILL.md skills/newsletter-draft/
```

### 3. Configure paths

In `SKILL.md`, replace the `{...}` placeholders with your actual paths:

| Placeholder | Replace with |
|-------------|-------------|
| `{STYLE_GUIDE_PATH}` | e.g., `{workspace}/Voice/style-guide.md` |
| `{PLATFORM_RULES_PATH}` | e.g., `{workspace}/Voice/platform-rules.md` (optional) |
| `{OUTLINES_DIR}` | e.g., `{workspace}/Outlines/` |
| `{DRAFTS_DIR}` | e.g., `{workspace}/Drafts/` |

### 4. Add your style guide

The skill reads the style guide before every drafting session. Without it, voice consistency is not enforced. The guide should include:

- Voice identity and tone recipe
- Sentence structure and rhythm rules
- Vocabulary: avoid/prefer lists, banned phrases
- Emotional mechanics map
- Rewrite operators
- Preflight and postflight checklists

---

## Usage

### Triggered manually

```
Draft the newsletter titled "The cost of clarity".
```

```
Let's write the draft. The outline is in Outlines/the-cost-of-clarity.md.
```

### Triggered from newsletter-outline handoff

When newsletter-outline finishes, it ends with:

> "Outline saved. Ready to draft? Use newsletter-draft."

Reply: "Yes, let's draft it." The agent picks up from the saved outline.

---

## What Happens

1. The agent locates the outline file (or asks you to paste it)
2. Reads the full style guide
3. Updates the outline's status frontmatter to `draft-in-progress`
4. Runs preflight — flags missing overlap scan, callback map, or source material
5. Writes the intro (with epigraph), shows it, asks "Approve, edit, or regenerate?"
6. On approval: saves it to `{DRAFTS_DIR}/[title].md`, moves to the next section
7. Repeats for each body section and the conclusion
8. Runs postflight on the completed file
9. Updates the outline status to `drafted`
10. Prompts handoff to newsletter-package

---

## Tips

**Keep source material in the outline.** The agent's no-fabrication rule means it will stop and ask if an example is missing. Having the raw material in the outline avoids interruptions.

**The epigraph.** If your outline was built with newsletter-outline, the epigraph source is already specified. If you're working from a hand-written outline, include a line like: "Epigraph: Tao Te Ching ch. 11 — the empty hub passage." Without it, the agent will ask before proceeding.

**Regenerate freely.** "Regenerate" at any chunk boundary discards that section and the agent rewrites from scratch. Use it if a chunk is off-register rather than trying to edit it into shape.

**Session continuity.** Each approved chunk is saved immediately, so you can pick up mid-draft in a new session. Tell the agent: "Continue the draft for [title] — intro is already written." It will read the file and pick up from where you left off.

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — 5-step chunked drafting pipeline |
| **Agent** | Claude Code |
| **Tools** | File read/write (`Read`, `Write`), outline search (`Bash` / `Glob`) |
