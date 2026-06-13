# Master prompt engineer

A conversational prompt-building assistant. You describe what you need; it asks 2-3 targeted questions, drafts the prompt, explains its structural choices, and offers variations — no template-filling required.

## What it's for

Getting well-crafted prompts without having to know prompt engineering theory. Useful when you're stuck on a task that keeps producing mediocre output, or when you want to systematize a prompt you'll reuse.

## How to use

Paste `PROMPT.md` as the system prompt (or opening assistant turn) in any capable LLM. Then describe your task in plain language — "I need a prompt that summarizes legal contracts for non-lawyers" is enough to start.

## What to expect

The model will ask a small number of focused questions (intent, audience, constraints), then produce:

- A ready-to-use prompt
- A brief explanation of the structural decisions
- One or two variations for different framings

It will also flag when the problem isn't a prompting issue at all — wrong model, missing data, unrealistic expectations.

## Notes

The built-in model-specific notes cover Claude, GPT, and Gemini. If you're using a different model, ask it to adapt the guidance accordingly.
