# Newsletter Package — Design spec

Design decisions behind the Newsletter Package skill and why they're made the way they are.

---

## Why packaging is a separate skill

Newsletter packaging — headline, subtitle, tags, header image — is a distinct creative act from writing the piece. They require different thinking.

Writing is generative and expansive. Packaging is reductive and precise: the headline has to carry the whole piece in under 10 words, without clickbait, front-loading the interesting thing, passing the voice check. The subtitle has to add scope without repeating the headline. The image has to capture the emotional arc, not the literal topic.

Trying to do both in one pass produces rushed packaging that doesn't represent the piece. Separating them — always reading the finished draft before brainstorming a single headline — produces packaging that earns the click.

The upstream pipeline (newsletter-outline → newsletter-draft → newsletter-package) reflects this: you write first, you package last. The skills don't overlap.

---

## Why one element at a time

The skill brainstorms one element (headline, subtitle, tags, image) before moving to the next. The human picks, edits, or asks for another round at each step.

The alternative — generating all packaging in one pass — produces internally inconsistent results. The headline shapes what a good subtitle looks like. The approved headline and subtitle together shape what the image should capture. Locking each element before moving to the next produces a coherent package.

It also keeps the human in the loop. Packaging is a judgment call about audience and voice that the agent can inform but not own. The one-at-a-time cadence forces explicit decisions rather than passive approval of a bundle.

---

## Why the packaging log matters

The packaging log is an append-only record of past decisions: chosen headline, rejected alternatives, subtitle, tags, image concept, image prompt, and notes on what worked.

It serves two functions:

**Pattern avoidance.** A strong publication has tonal variety. If the last four headlines were `[declarative]`, the reader starts to feel it. If the last three images all used the same motif, the visual identity feels monotonous. Checking the log before brainstorming prevents this without requiring the human to remember what shipped last month.

**Institutional memory.** Over time the log becomes a record of what angles your audience responds to, which image directions worked, and which rejected headlines were close but not right. That history is worth having.

The log is append-only. Nothing is ever deleted or updated — the record is a record.

---

## Why headline tagging by angle

The 5–7 options are each tagged with an angle (`[curiosity]`, `[declarative]`, `[question]`, `[contrarian]`, `[metaphor]`, `[minimal]`). This is not taxonomy for its own sake — it's a forcing function.

Without angle tags, brainstormed headlines cluster around whatever the most natural framing for the piece is. Most pieces have an obvious declarative headline; generating seven of those is waste. Requiring coverage across angles forces the agent to look for the piece's alternative entry points. A `[contrarian]` headline that challenges the assumption the piece disrupts is often stronger than the obvious `[declarative]`, and it wouldn't surface without the constraint.

The human can ignore the tags. They're for the brainstorm, not the selection.

---

## Why image prompt approval is mandatory

Image generation is irreversible, slow, and consumes real API or compute cost. A bad prompt produces an image that doesn't match the piece, which means either shipping a mismatched image or regenerating.

The two-step pattern (describe concept in plain language, write full prompt, get approval, generate) front-loads the cost of getting the concept right. Most mismatches happen at the concept level, not the prompt level. If the human approves the plain-language concept, the full prompt is usually on target. If they don't, the discussion happens before a generation runs.

This is not a general "show prompt before running" best practice. It's specific to image generation because: (1) the output is visual and can't be verified by the agent, (2) the cost of a wrong generation is higher than the cost of the extra round-trip, and (3) the image is the most brand-sensitive element in the package.

---

## Why post-publish closure is part of the skill

The packaging skill closes the loop: when the issue ships, it stamps the frontmatter, moves the draft to the published folder, and appends to the publication log.

This could be a separate skill or a manual step. It's part of this skill because the alternative is drift: the draft stays in the drafts folder marked `draft`, the publication log goes un-updated, and weeks later the vault's record of what shipped is unreliable.

The vault-based publication log is the source of truth for shipped issues. It needs to be updated at publish time, not reconstructed later. Closing the loop in the same session that prompted "publish" is the reliable path.

The shipped headline and vault filename are both recorded because they often differ. Titles get edited between draft and publish. Recording both prevents future confusion about which file corresponds to which issue.

---

## Relationship to upstream and downstream skills

| Skill | Role |
|-------|------|
| **newsletter-outline** | Structures the argument before writing begins |
| **newsletter-draft** | Writes the piece; hands off to package |
| **newsletter-package** | Packages the finished draft; hands off to social-posting |
| **social-posting** | Generates platform-specific promo posts after publishing |

The skills are designed to be used in sequence but are each self-contained. If you already have a draft and want to skip outline and draft, you can invoke newsletter-package directly.

---

## Known limitations

- **Image generation quality depends on your model.** The skill writes a detailed generation prompt and iterates on feedback, but the image model's capabilities determine the output ceiling. Adjust your `{brand_identity}` image style to what your model handles well.
- **Tag libraries drift.** If your distribution platform adds or deprecates tags, the library needs manual updating. The skill will recommend based on what's in the file.
- **The packaging log is only as useful as it's consistent.** If you skip logging for some issues, the pattern-avoidance check becomes unreliable. Log every issue.
- **Post-publish closure requires a confirmed publish date.** The skill asks if not supplied. Without it, the frontmatter and publication log entries are incomplete.
