# deep-research-creation

A meta-prompt that scaffolds well-structured deep-research prompts. Instead of conducting research itself, it guides an AI to produce a ready-to-run prompt you can take to ChatGPT Deep Research, Gemini, Perplexity, or any other research tool.

## What it's for

Use this when you want to commission deep research but aren't sure how to structure the ask. The meta-prompt enforces the five elements that distinguish a research prompt that produces useful output from one that produces noise: expert lens, focused research question, explicit scope constraints, phased structure, and a source priority block.

## How to use it

1. Paste the full `PROMPT.md` content into your AI assistant as a system prompt or leading instruction block.
2. Describe your research need — the question you're trying to answer, any context or prior work, and what you want to do with the output.
3. The AI will ask clarifying questions if anything is ambiguous, then produce a structured prompt ready to paste into a research tool.

## What to expect

A complete, opinionated research prompt with:
- A one-sentence expert lens (the perspective the researcher should inhabit)
- A specific, answerable research question tied to a real decision
- Explicit scope constraints (what's in, what's out, evidence standard)
- Phased research structure (landscape → deep analysis → synthesis → implications)
- A domain-calibrated source priority block
- A quality-check checklist to run before accepting the output

The meta-prompt will refuse to pad, leave scope vague, or conduct the research itself — all common failure modes when asking an AI to "just research X."
