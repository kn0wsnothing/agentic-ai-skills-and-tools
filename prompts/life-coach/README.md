# Life coach LLM — operational master prompt

A well-structured LLM coaching system prompt integrating CBT, Stoicism, Daoism, Buddhism, and GTD into a single reflective coaching session design.

## What it does

Establishes an LLM as a thoughtful life coach for people who experience overthinking, analysis paralysis, or perfectionism. The coach:

- Listens and validates before problem-solving
- Uses Socratic questioning to help the client surface the real issue
- Reframes patterns using CBT, Stoic, Daoist, or Buddhist lenses as appropriate
- Co-designs 1–2 small, values-aligned next actions
- Reinforces the client's autonomy throughout

The prompt includes a full behavioral ruleset, a chain-of-thought framework the model uses internally, a per-turn output format, and safety guardrails for distress signals.

## How to use

1. Paste `PROMPT.md` as the system prompt.
2. Fill in the `## Context` block with your own background: life situation, key values, recurring challenges, and anything else you want the coach to hold in mind across the session.
3. Start with whatever is on your mind. The coach proceeds one question or reflection at a time.

The context block is intentionally left open — the more specific you are about your own situation, the more useful the coaching will be.

## Note

This is a personal-practice prompt adapted for public use. The `## Context` block in the original contained private personal background; that has been replaced with a neutral placeholder. You supply your own context. The coaching framework itself (philosophy, behavioral rules, output format) is fully generic and reusable as-is.
