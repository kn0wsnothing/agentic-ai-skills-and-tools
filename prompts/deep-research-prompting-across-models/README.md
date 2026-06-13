# deep-research-prompting-across-models

A ready-to-run deep research prompt for mapping operational prompting differences across six frontier model families — Claude, GPT-5/o-series, Gemini 2.5+, GLM, Kimi, and Qwen — with the goal of building a meta-prompt that routes and adapts to each model's strengths.

## What it's for

Use this when you're working across multiple model families and need to know — operationally, not in principle — how to prompt each one differently. The research covers system prompt handling, reasoning elicitation, instruction-following style, tool use, long-context behavior, output formatting, refusal surfaces, and known failure modes, then synthesizes those into transferable patterns and a meta-prompt design.

This prompt is particularly useful for teams building model-routing infrastructure, engineers who need to re-prompt the same task for different models, or anyone who wants to understand what actually differentiates frontier models at the prompting level rather than the benchmark level.

## How to use it

Paste `PROMPT.md` directly into a deep research tool (ChatGPT Deep Research, Gemini Deep Research, Perplexity, etc.) as the research prompt. The scope is January 2025 onward and covers six named model families — adjust the models-in-scope list if the landscape has shifted since the prompt was written.

Note: the prompt explicitly requires Chinese-language sources for GLM, Kimi, and Qwen. If your research tool doesn't handle Chinese sources well, flag this gap and supplement with the named English-language engineering blogs listed in the source priority block.

## What to expect

A five-phase research output:
1. Per-model deep profiles with consistent sub-headings across all six families (directly comparable)
2. A cross-model comparative matrix — operational cells, not prose summaries
3. A taxonomy of prompting patterns that transfer vs. those that are model-specific, with before/after examples
4. Strengths/weaknesses synthesis for each family, tied to evidence
5. Implications for building a meta-prompt — named conditional branches, routing heuristics, compensating instructions, and a one-page operator's cheat sheet

The quality checks enforce equal depth across all six families (no thin GLM/Kimi/Qwen coverage due to English-source bias) and require that Phase 5 be specific enough to actually write a meta-prompt from.
