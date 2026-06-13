# DataOps to AIOps research synthesis

A synthesis prompt for collapsing multiple AI-generated research reports on DataOps-to-AIOps transformation into a single unified intelligence brief, structured as an inverted pyramid so every reader level gets what they need without wading through what they don't.

## What this does

Takes a context file (who this is for, what decision it supports) and two or more research report files, and produces a five-level brief:
1. **The bottom line** — 100–150 words for C-suite; one paragraph with the core finding, its implication, and the clearest action signal
2. **Key findings** — 3–5 declarative findings ordered by relevance to the stated context, each with a corroboration note and evidence strength rating
3. **Analysis and evidence** — full supporting detail, contradictions between sources surfaced and explained (not smoothed), connections to the specific context
4. **Technical depth and methodology** — architecture patterns, implementation approaches, methodology behind each report's findings, open research gaps
5. **Source report summaries** — one entry per source for traceability

## When to use it

- You have run multiple deep-research passes on the same transformation topic and need to synthesize them into something a decision-maker can act on
- You need a single document that works for both a 2-minute executive read and a 30-minute practitioner deep-dive
- You want contradictions between sources surfaced, not resolved by averaging

## How to use it

Invoke with:
```
Use the synthesis prompt at [path/to/prompt]. Context file: [path/to/context.md]. Reports: [path1], [path2], [path3].
```

The context file is load-bearing — the entire synthesis is grounded in it. Write it before running the prompt. It should cover: who the research is for, what decision or question it supports, and the organization's current situation. Keep it under a page.

## Key design decisions

**Context file is mandatory, not optional.** Without it, the synthesis degenerates into a generic summary. The prompt forces the model to reread the context after digesting the reports, then asks "what is the one thing a senior leader in this specific situation most needs to know?"

**Contradictions are preserved, not synthesized away.** The hard constraints section explicitly forbids smoothing over disagreements. Where reports conflict, the brief must surface the conflict and explain it — usually by naming the condition under which each finding holds.

**Inverted pyramid enforces audience separation.** A reader who stops after Level 1 has the most important thing. A reader who jumps to Level 3 is not lost. Each level is written as a standalone section, not a continuation of the previous one.

**Inference is flagged, not embedded.** Any finding that is implied but not explicitly stated in the source reports must be marked `[Inferred — not explicitly stated in sources]`. No fabrication of supporting evidence.
