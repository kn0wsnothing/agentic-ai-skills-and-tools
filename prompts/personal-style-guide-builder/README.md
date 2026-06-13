# Personal style guide builder

Analyzes a corpus of your own writing and produces a structured style guide — tone, sentence patterns, vocabulary — that you can hand to any LLM to get output that sounds like you, not like generic AI.

## What it's for

Solving the "AI slop" problem. Once you have a style guide grounded in your actual writing, you can paste it into any writing prompt as a constraint layer. Particularly useful for newsletter writers, bloggers, and anyone who publishes regularly and uses AI assistance.

## How to use

1. Collect ~50 samples of your own writing (newsletter issues, articles, posts — whatever represents your voice best). Paste them into the conversation.
2. Paste `PROMPT.md` as the system prompt.
3. The model will analyze the corpus and output a structured style guide with Do/Don't lists in each section.

The output is designed to be pasted directly into future writing prompts as a constraint block.

## What to expect

A 2000-word-max style guide covering:

- **Tone & voice** — primary register, emotional range, relationship with the reader
- **Sentence structure** — length, complexity, rhythm patterns
- **Vocabulary** — preferred terms, words to avoid, how you handle jargon

Every guideline is grounded in quoted evidence from your samples — no generic writing advice.

## Notes

Quality scales with sample diversity. Include your best and your roughest pieces; the contrast helps the model identify what's consistent vs. situational.
