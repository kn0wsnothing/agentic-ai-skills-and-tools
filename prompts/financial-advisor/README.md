# Financial advisor LLM — personal planning copilot

A structured system prompt for running an LLM as an ongoing personal financial planning copilot. Covers budgeting, investment strategy, scenario planning, and long-term goal management.

## What it does

Establishes an LLM as a fee-only fiduciary-style financial advisor. Across multiple sessions it helps you:

- Build and refine a monthly budget and savings plan
- Design an investment strategy aligned with your risk tolerance and time horizons
- Identify spending optimization opportunities
- Plan for education, retirement, and major purchases
- Model scenarios ("Can I afford to reduce hours in 3 years?")

Each conversation follows a consistent structure: scope clarification, step-by-step analysis, concrete recommendations with explicit assumptions, risk/downside framing, and practical implementation steps. Sessions close with a "Next 3 actions" list.

## How to use

1. Paste `PROMPT.md` as the system prompt.
2. Fill in the `## Context` block with your own financial situation: jurisdiction, household composition, major assets/liabilities, income stability, and key goals.
3. The model's first reply will restate its role, ask up to 5 high-leverage questions, and suggest 2–3 ways to start.

The more specific your context block, the more directly applicable the recommendations will be.

## Note

This is a personal-practice prompt adapted for public use. The original `<context>` block contained private personal and financial details (residence, family composition, specific assets and expenses); that has been replaced entirely with a neutral placeholder. You supply your own financial situation. The advisor framework, task structure, and style guidelines are fully generic and reusable as-is.

**Disclaimer:** This prompt produces educational output from an LLM, not licensed financial advice. Always verify jurisdiction-specific tax and regulatory assumptions with a qualified professional.
