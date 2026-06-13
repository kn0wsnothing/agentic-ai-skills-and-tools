# Newsletter Package — Claude Code

How to wire up the newsletter-package skill in a Claude Code project.

---

## Setup

### 1. Create your reference files

Before running the skill, you need four reference files. Create them if they don't exist:

**Voice guide** — your publication's voice identity, anti-patterns, and tone rules. What sounds like you. What never does. A few concrete examples of both.

**Visual brand identity** — your image aesthetic, preferred motifs, palette, negative prompts, aspect ratio, paper or surface characteristics. Written to be pasted into an image generation prompt. Include an `## Iteration log` section that the skill will append to over time.

**Packaging log** — start with a blank file or a header:
```markdown
# Packaging log

<!-- Entries added by newsletter-package skill, newest first -->
```

**Tag library** — your distribution platform's canonical tag set, one per line or as a markdown list. Pull this from your platform's tags page.

### 2. Copy the skill

```bash
mkdir -p .claude/skills/newsletter-package
cp /path/to/SKILL.md .claude/skills/newsletter-package/
```

Or symlink from a shared skills library:

```bash
ln -s /path/to/skills/newsletter-package/SKILL.md .claude/skills/newsletter-package/SKILL.md
```

### 3. Configure paths in CLAUDE.md

Add a configuration block to your `CLAUDE.md`:

```markdown
## Newsletter package configuration

| Parameter | Value |
|-----------|-------|
| `{voice_guide}` | `content/reference/voice-guide.md` |
| `{brand_identity}` | `content/reference/visual-brand-identity.md` |
| `{packaging_log}` | `content/reference/packaging-log.md` |
| `{tag_library}` | `content/reference/tag-library.md` |
| `{drafts_dir}` | `content/drafts/` |
| `{published_dir}` | `content/published/` |
| `{publication_log}` | `content/reference/publication-log.md` |
```

Adjust paths to match your project structure.

### 4. Add the trigger instruction

In `CLAUDE.md`:

```markdown
## Newsletter package

When the user says "package [title]", "headline for [title]", "image for [title]",
or when handed off from newsletter-draft ("ready to package?"), invoke the
newsletter-package skill from `.claude/skills/newsletter-package/SKILL.md`.
```

---

## Running

### Triggered from newsletter-draft

If you use the newsletter-draft skill, it ends with a handoff prompt. Say "yes" or "ready to package" and the packaging skill picks up automatically.

### Invoked directly

```
package [title of your draft]
```

or

```
headline for [title of your draft]
```

or

```
image for [title of your draft]
```

---

## Post-publish closure

When your issue is live, return to Claude Code and confirm:

```
Published. It went live on [date].
```

The skill will stamp the frontmatter, move the draft file to your published folder, and append to the publication log. Check the output to confirm both the file path and the publication log entry are correct.

---

## WAT mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — 9-step packaging pipeline + post-publish closure |
| **Agent** | Claude (Opus recommended — packaging requires real judgment on voice and angle) |
| **Tools** | File read/write, image generation access |

---

## Tips

**Build the reference files before the first session.** The voice guide and brand identity doc are the skill's inputs — the quality of the packaging directly reflects the quality of those files. A thin voice guide produces generic headlines. A detailed one produces options that sound like you.

**The packaging log starts paying dividends around issue 5–10.** Early runs it's sparse context. By issue 20 it's a real pattern record. Keep logging even when it feels redundant.

**Run packaging in a dedicated session.** Packaging requires creative back-and-forth that benefits from a clean context. If you've spent two hours drafting and debugging, starting packaging in the same session usually means rushed headline work. A fresh session with a fresh read of the draft produces better options.

**Image generation models vary.** The skill writes detailed prompts — but results depend on your model. If you're using a model new to you, run a few test generations to calibrate prompt length and specificity before packaging your first issue.
