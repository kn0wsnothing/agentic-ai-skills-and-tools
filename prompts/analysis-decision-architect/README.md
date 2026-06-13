# Analysis decision architect

A system prompt that turns a fuzzy data question into decision-grade analysis through a structured, multi-turn workflow.

## What it does

Most analysis fails not because of bad data but because the question was never tied to a decision. This prompt forces that discipline upfront and holds it through every step: intake, brief, analysis plan, grapple/interpretation loop, devil's advocate pass, stakeholder narrative, and decision log.

The workflow is designed for a human-plus-AI-tool setup where the AI handles structure, framing, and narrative — and the human uses their data tool (SQL, spreadsheets, a BI platform, another AI) for the actual data work.

## When to use it

- You have a stakeholder request that feels vague or analysis-shaped but hasn't been tied to a specific decision
- You need to structure a data question before handing it to an analyst or data tool
- You want to pressure-test an interpretation before presenting it
- You need a stakeholder-facing narrative that will survive a sceptical room

## What makes it different

**Decision-first intake.** The prompt will not let you skip past "who owns this decision, and what changes based on the answer." If there's no decision, it helps you re-scope.

**Hard turn-taking.** It does not jump ahead. Brief before plan. Plan before execution. Take before devil's advocate. Each gate requires explicit acceptance.

**Adversarial pass built in.** The devil's advocate step (Step 4d) is not optional — it fires against the accepted interpretation before any stakeholder narrative is drafted, and produces an objection map the author uses to strengthen the main argument or prepare the appendix.

**Interlocutor mode.** During the grapple loop (Step 4b), the AI holds strict discipline: it answers questions about the findings, surfaces patterns only when invited, and never interprets, drafts narrative, or suggests new data cuts. The human's thinking stays in the human's hands.

**Plain-language metric translation.** The prompt requires every technical metric name to be unpacked on first use and every headline number to be anchored in a distribution or benchmark. Numbers without context invite scepticism.

## How to use it

1. Paste the prompt as a system prompt or into a new conversation with your preferred model.
2. Describe your analysis situation — a stakeholder request, a question you're trying to answer, or existing work you want to pressure-test.
3. Follow the multi-turn workflow. The prompt will gate you at each step.

If you already have a brief or plan, say so and describe where you are in the workflow — the prompt will pick up from the appropriate step.

## Files

- `PROMPT.md` — the full system prompt

## Related

- [Decision log template](../decision-log-template/) — lightweight generic template for closing out an analysis with a decision record
