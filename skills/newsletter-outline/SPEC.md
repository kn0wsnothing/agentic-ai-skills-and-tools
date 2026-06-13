# Newsletter Outline — Design Spec

This document explains the design decisions behind the Newsletter Outline skill.

---

## The Problem

Most writers sit down to draft with a vague idea, a pile of notes, and a blank document. The result is usually a draft that discovers its argument halfway through — which means either a long editing cycle or a published piece that buries its lede.

The outline skill forces the commitment earlier. Before a word of prose is written, the writer has answered: who is this for, what should they walk away with, what is the one thing they'll remember. Those three answers constrain everything that follows. The draft stops being exploration and starts being execution.

---

## Why Framing Questions First

The three framing questions aren't warmup — they're the filter. They determine:
- Which source notes are relevant (Step 2 depends on them)
- Which published pieces might overlap (Step 3 depends on them)
- The Foundation section (Step 4 starts with them)

Collecting them before touching any notes prevents the common failure mode: building an outline around *what you have* rather than *what the reader needs*. The notes are evidence for an argument; the argument comes from the framing.

---

## Why Source Notes Before the Outline

The outline is only as good as its raw material. Recommending source notes (Step 2) before drafting sections serves two purposes:

1. **Surfacing forgotten material.** Writers often forget what they've written. A scan of the notes archive often surfaces a more relevant note than the one they had in mind.

2. **Limiting scope.** Confirming 3–5 notes before starting prevents scope creep — the outline stays focused on what the source material can actually support.

---

## Why the Overlap Scan

The overlap scan (Step 3) is the most friction-adding step, and it's there deliberately.

Recurring storytellers have a failure mode: they tell the same story in multiple pieces without knowing it. A vignette from a two-year-old essay reappears — same setup, same punchline — in a new piece, and long-time readers notice. The overlap scan catches this before the draft is written.

The four checks in Step 3 (overlap, attribution, callback map, illustrative material) address different failure modes:

- **Overlap:** don't repeat yourself accidentally
- **Attribution:** don't borrow without acknowledging
- **Callback map:** reuse deliberately — callbacks build continuity when explicit
- **Illustrative material:** don't let an anecdote do work it can't actually support

The scan presents all four findings together and waits for approval before proceeding. This is intentional: the writer makes one informed decision about the structural risks, then moves forward.

---

## Why Section at a Time, Approved Before Writing

The step-by-step approval loop (Step 4) exists because outline sections are interdependent. The Turn depends on what the Journey sets up. If the Journey is wrong, an approved Turn is worthless.

Writing each section only after the previous is approved keeps the structure coherent. If section N is wrong, you fix it before building section N+1 on top of it — not after.

Immediate write-on-approval (no batching) means the file reflects the current agreed state. If the session ends early, the partial outline is usable. If the writer wants to edit a section later, the file is the source of truth.

---

## The Epigraph Requirement

Every essay gets an epigraph — a short passage from the piece's philosophical anchor. The outline nominates the source and passage; the draft writes the polished prose.

This requirement exists because epigraphs anchor the essay's register. They signal to readers what kind of thinking they're entering. An unspecified epigraph at outline stage usually means no epigraph at draft stage — or a hastily chosen one that doesn't earn its position.

The constraint: the outline must name the source and passage, and must note where its payoff lands in the body. This forces the writer to know what the piece is *for* at a level deeper than the framing questions alone reveal.

---

## The Subheadings Requirement

Provisional subheadings at outline stage serve two purposes:

1. **Navigation for the reader.** A published piece needs navigable structure. Discovering subheadings at draft stage usually produces LinkedIn-style taglines rather than genuine wayfinding. Doing it at outline stage, when the structure is explicit, produces better subheadings.

2. **Forcing function for the outline.** If a section can't be described in a sentence-case declarative, the section doesn't have a clear claim yet. The subheading requirement surfaces structural vagueness before it becomes a drafting problem.

Subheadings are plain declaratives or questions in the writer's voice. Not taglines, not punchy subtitles. The bar is: would a reader skimming these know where to find what they're looking for?

---

## Anti-AI Voice Standard

The outline sets the voice register for the draft. If the outline is written in AI-ish framing ("explore the intersection of X and Y", "readers will gain a deeper understanding"), the draft follows.

The standard — specific claims over abstract descriptions, named paradoxes over "open with a paradox", open questions over papering over uncertainty — is enforced at outline stage because it's cheaper to fix here than in the draft.

---

## Integration with the Content Pipeline

The Newsletter Outline skill is the first step in a three-skill pipeline:

```
[newsletter-outline]
    ↓ saved outline file
[newsletter-draft]
    ↓ saved draft file
[newsletter-package]
    ↓ final checks, metadata, publish
```

Each skill is self-contained — it assumes no in-session context from the others. The file system is the handoff mechanism: the outline file is the input to newsletter-draft; the draft file is the input to newsletter-package.

---

## Design Tradeoffs

### Gate friction vs. flow

The three framing questions, source confirmation, and overlap scan all add friction before a word of outline prose is written. This is a deliberate tradeoff: the friction is cheap at outline stage; discovering the same problems at draft or publication stage is expensive. Writers who find the gates annoying are usually writers who know the piece well enough that the gates are fast. For everything else, the gates catch real problems.

### Opinionated structure vs. flexibility

The six-section structure (Foundation, Opening, Journey, Turn, Destination, Writing logistics) is opinionated. It won't fit every essay type. The style guide's structure blueprints are the override mechanism — if the essay is a framework essay rather than a principle essay, the sections shift accordingly.

The skill doesn't enforce a specific structure; it enforces a *structured process*. The structure blueprints determine which sections to use.
