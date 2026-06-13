# Prompt — Decision log

When a decision has been reached (or explicitly deferred), produce a decision log entry using the template below.

Fill in each field. If the decision is still pending, mark it "pending" and fill in what is known. Use placeholders only where information is genuinely unavailable — do not omit fields.

---

## Template

**Decision:** [What was decided, in one plain sentence. Or "pending — decision expected by [date]."]

**Owner:** [Name or role of the person who owns this decision and is accountable for follow-through]

**Date decided:** [Date, or "pending"]

**Context:** [2–4 sentences. Why this decision was needed, what question it closes, what analysis or discussion it was based on.]

**Options considered:** [Brief summary of the alternatives that were evaluated before this decision was made. One line per option is enough.]

**Expected impact:** [A range, not fake precision. What is expected to change, by how much, over what timeframe. Flag confidence level if uncertain.]

**Follow-up date:** [When to check whether the decision is having the expected effect]

**What will be monitored:** [Specific metric or signal + threshold that would trigger a review. "We'll keep an eye on it" is not acceptable — name the metric and the threshold.]

**What would change our mind:** [The disconfirming evidence that would cause this decision to be reversed or revisited. Be specific. If nothing could change the decision, that is worth naming too.]

**Open questions:** [Anything unresolved that this decision depends on or that could affect its implementation. If none, write "None."]

---

## Instructions for the AI

- Propose the completed log to the user before writing it anywhere.
- Do not invent owners, dates, or expected impact. If a field is genuinely unknown, mark it unknown and note what would be needed to fill it.
- "We'll look into it" is not an acceptable entry for any field. Options: answer it, frame it as a known unknown with a timeline, or flag it as a risk.
- Keep the language plain. Decision logs are read weeks or months later by people who weren't in the room.
- One log entry per decision. If a session produced multiple decisions, produce a separate entry for each.
