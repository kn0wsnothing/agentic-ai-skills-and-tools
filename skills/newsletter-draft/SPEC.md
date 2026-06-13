# Newsletter Draft — Design Spec

This document explains the design decisions behind the newsletter-draft skill.

---

## The Problem

Writing a long-form essay in one shot is brittle. The agent either produces a monolithic draft that needs wholesale revision, or it front-loads everything on a single approval step that puts too much on the user at once.

The alternative is chunked drafting with progressive commitment: write one section, get approval, save it, continue. Each boundary is a natural review point. The user can redirect early without losing what they've already approved. The agent never outruns the human.

---

## Why One Chunk at a Time

The constraint is deliberate. Showing multiple chunks simultaneously creates a false efficiency — the user gets a large block to review, approval becomes rubber-stamping, and errors compound through sections that built on earlier mistakes.

One chunk at a time:
- Forces the agent to do its best work on each section before moving on
- Gives the user a natural pause point at each narrative boundary
- Makes redirection cheap — a single section rewrite, not a full draft
- Keeps the conversation lean — the chunk is the content

The rule is absolute: never show two chunks simultaneously, even if the user says "just write the whole thing." The skill's value is in the paced review.

---

## Why Save on Approval

Each approved chunk is written to the draft file immediately, not batched. The reason is practical: sessions end unexpectedly. If the agent writes everything to the file only at Step 5, a session interruption loses all the approved work. Save on approval means whatever has been approved is already durable.

The implementation requirement is Read + Write (full file rewrite), not an append-only edit. This prevents corruption from partial writes and ensures the file is always in a valid state.

---

## The No-Fabrication Rule

This is the strongest constraint in the skill. The agent must never invent personal anecdotes, examples, or details not present in the outline or source notes.

This failure mode is common: a model asked to write a personal essay will pattern-match on what a personal essay looks like and generate plausible-sounding anecdotes. These sound fine in isolation but are factually wrong — they didn't happen. If the author is presenting the newsletter as their own thinking, invented stories are a credibility problem.

The rule requires asking when source material is missing, not filling in. The question "I don't have a real example for this section — do you have one?" is better than a fabricated one that has to be cut later.

---

## Why the Style Guide Is Read Every Session

The instruction to read the full style guide before every drafting session (not a summary) reflects a real failure mode: agents apply style rules inconsistently when they rely on compressed recall from earlier in a long context window.

Reading the full guide at session start ensures:
- Banned phrases are checked against the current list, not a remembered approximation
- Voice identity is refreshed, not assumed
- The rewrite operators are available when a chunk needs restructuring

The overhead is one file read. The cost of a draft that doesn't sound like the writer is much higher.

---

## The Epigraph Requirement

Every essay gets an epigraph — a short translated quote placed before the first paragraph, attributed inline.

This is a structural requirement, not a stylistic preference. The epigraph:
1. Sets the philosophical register before the reader enters the piece
2. Creates a payoff anchor the body can resolve against
3. Signals the essay's intellectual lineage

The design permits two execution patterns: an immediate reframe after the epigraph, or a standalone quote with a delayed payoff (the body returns to it). Either is valid. What is not valid: skipping it, or treating it as optional.

If the outline didn't specify an epigraph source, the agent surfaces this at preflight rather than inventing one.

---

## Subheadings as Navigation Contracts

Subheadings in long-form essays serve two functions: navigation (the reader can skim and find their place) and argument structure (the writer commits to what each section is actually about).

The requirement for 5–8 sentence-case subheadings is not decorative. A 1,800–2,000 word essay without subheadings is hard to read; an essay with too many feels fragmented. The 5–8 range forces the writer to identify the major beats.

The style constraint — plain declaratives or questions in the writer's voice, not LinkedIn-style subtitles — prevents a common drift: agents default to punchy three-word taglines or dramatic reframes because these pattern-match on "newsletter content." Plain, honest subheadings are harder to generate and more durable.

---

## The Overlap and Callback Gate

Before drafting begins, the skill checks whether the outline includes a published-overlap scan and a callback map.

**Published-overlap scan:** checks whether the topic has been covered in previous issues. Readers notice when an author repeats themselves without acknowledgment. Finding overlaps early (before prose exists) is much cheaper than finding them in editing.

**Callback map:** cross-references to earlier issues that the draft should acknowledge or build on. Newsletters that cross-reference their own history reward long-term readers and build a sense of continuity.

Both checks belong at outline stage (newsletter-outline does them in detail). The newsletter-draft skill checks whether they happened — if the outline was produced without them, it asks before proceeding rather than silently skipping.

---

## Postflight as a Mandatory Gate

The postflight checklist runs after all chunks are approved but before the draft is finalised. The sequence matters: the agent reviews the full assembled draft, not individual chunks.

Postflight catches issues that don't appear in any single chunk:
- Epigraph missing (was never added)
- Subheadings fell below the 5-subheading minimum
- A sentence that ran long because two chunks were joined
- First-person disclosures concentrated in one section, absent elsewhere

The agent is required to flag any failures and ask whether to fix before finalising. "Proceed as-is" is a valid answer — the user may be doing further editing. But the agent surfaces the issues; it doesn't silently pass a draft that fails its own checklist.

---

## Pipeline Position

This skill sits between newsletter-outline (which produces the approved outline) and newsletter-package (which produces the publish-ready issue from the completed draft).

The input contract: an outline with section beats, epigraph specification, provisional subheadings, and (ideally) overlap scan and callback map.

The output contract: a draft file with all sections, correct frontmatter, epigraph, subheadings, and postflight cleared. Outline status updated to `drafted`.

Neither upstream nor downstream skill is required to run this one — the agent can accept a pasted outline and hand off a draft path manually. But the pipeline produces the best results when all three skills are used in sequence.

---

## WAT Mapping

| Layer | Role |
|-------|------|
| **Workflow** | `SKILL.md` — the 5-step chunked drafting pipeline |
| **Agent** | Any agent with file read/write access and style guide context |
| **Tools** | File read, file write (full rewrite), outline search |
