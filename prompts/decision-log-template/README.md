# Decision log template

A lightweight prompt for closing out a piece of analysis or decision-making with a structured decision record.

## What it does

Produces a single-page decision log covering what was decided, who owns it, what was expected, what will be monitored, and — critically — what would change the decision. Forces the author to name a specific metric and threshold rather than leaving follow-up vague.

## When to use it

- After completing an analysis and reaching a conclusion
- When a decision is made in a meeting and needs to be written down before it goes stale
- When explicitly deferring a decision (pending logs are valid — they capture what's known and what's needed)
- As the closing step of any structured analysis workflow

## The key fields

Most decision logs capture what was decided. This one also captures:

- **What will be monitored** — a named metric and a threshold, not "we'll keep an eye on it"
- **What would change our mind** — the specific disconfirming evidence that would reverse the decision; if nothing could change it, that's worth naming
- **Options considered** — a brief record of what else was evaluated, so future readers understand why this path was chosen

## How to use it

Paste `PROMPT.md` as a system prompt or into a new conversation. Describe the decision that was made (or needs to be documented) and the context around it. The AI will draft the log and propose it before writing it anywhere.

If the decision is still pending, the prompt handles that too — fill in what's known and mark the rest pending.

## Files

- `PROMPT.md` — the template and instructions

## Related

- [Analysis decision architect](../analysis-decision-architect/) — the full multi-step workflow this decision log closes out
