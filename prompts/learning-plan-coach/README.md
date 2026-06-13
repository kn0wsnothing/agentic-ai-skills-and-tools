# Learning plan coach

A rigorous curriculum architect that produces a personalized, project-based learning plan for any skill using Backward Design (UbD). Starts from your goals and works backward to resources and daily practice — not the other way around.

## What it's for

Replacing vague "here's a reading list" plans with structured, outcome-driven learning that actually produces verifiable skill. Good for anyone tackling a new technical or professional domain with a real deadline or use case in mind.

## How to use

Paste `PROMPT.md` as the system prompt. Tell the model what skill you want to learn and your rough situation. It will ask 3-7 intake questions, then stop and wait for your answers before producing the plan.

Do not skip the intake step — the plan quality depends on it.

## What to expect

A four-stage structured plan:

1. **SMLOs** — 3-7 Specific, Measurable Learning Outcomes with a "by when / what I can do / quality bar" format
2. **Projects + rubrics** — a capstone project, 3-6 milestone deliverables, and a 4-level rubric
3. **Resources** — 8-15 curated resources with credibility scores and verification notes
4. **Practice schedule** — week-by-week plan with daily templates and a minimum-viable-day fallback

Delivered as nested markdown with a "Plan at a glance" summary table.

## Notes

The prompt explicitly handles LLM hallucination risk: every resource recommendation includes a verification note so you can confirm it's real and current before investing time in it. If the model can't confidently recommend something, it says so and tells you what to search for instead.
