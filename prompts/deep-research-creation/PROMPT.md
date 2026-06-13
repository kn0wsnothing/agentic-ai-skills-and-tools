# Prompt — Deep Research Creation Prompt

Use this when asked to help craft a deep research prompt. Your job is to produce a structured, ready-to-run prompt that the user will take to an external research tool (ChatGPT Deep Research, Gemini, Perplexity, Claude, etc.). You are NOT conducting the research yourself.

---

## Step 1 — Clarify before building

Before drafting, confirm:
- **Research question:** What is the core question to answer? Is it comparison, analysis, recommendations, landscape scan, or framework-building?
- **Expert lens:** What perspective should the researcher adopt? (e.g. senior data strategist, media economist, editorial product lead)
- **Scope constraints:** Timeframe, geography, industry, audience, depth level
- **Output shape:** What does the user need at the end? (matrix, narrative, recommendations, per-entity summaries, implications for a specific context)
- **Context files:** Are there project briefs, meeting notes, or prior analysis to draw from? Read them before drafting.

If any of these are missing or ambiguous, ask before proceeding.

---

## Step 2 — Structure the prompt

Every prompt you produce must include the following sections:

### Expert lens
One sentence. Name the role and the specific expertise the researcher should embody. Be precise — not "an expert" but "a senior digital media strategist with experience in subscription product design and editorial monetisation."

### Research question
The core question in plain language. Should be answerable (not a topic), specific enough to scope the research, and directly tied to a decision or outcome the user cares about.

### Scope constraints
Explicit boundaries: timeframe, geography, industry focus, team size, infrastructure constraints, evidence standard. Include what to exclude.

### Phases
Structure the research in ordered phases. Standard arc:
1. **Landscape scan** — current state, key players, baseline
2. **Deep analysis** — investigate sub-questions systematically (name them explicitly)
3. **Synthesis** — patterns, tensions, non-obvious insights
4. **Implications / recommendations** — tied to the user's specific context

Adapt phases to the topic. Add a per-entity analysis phase when the research requires publisher-by-publisher, company-by-company, or tool-by-tool breakdowns.

### Source priority
Include a source priority block in every prompt. Adapt to the domain:

**For media / journalism topics:**
- Trade press: Press Gazette, Nieman Lab, Reuters Institute, WAN-IFRA, Digiday, INMA
- Publisher statements: earnings calls, investor presentations, official blogs, leadership interviews
- Conferences: WAN-IFRA, INMA World Congress, Digital Media Europe, Reuters Institute events
- Quote named executives where possible

**For tech / data / AI topics:**
- Trade press: Wired, MIT Technology Review, The Information, Stratechery
- Primary sources: official documentation, engineering blogs, conference talks (NeurIPS, ICML, MLOps community)
- Industry analysts: Gartner, Forrester (flag paywalled sources)

**For all topics — avoid:**
- SEO blog summaries
- Generic explainers
- Marketing agency commentary

**Always include:** "Adapt this list to the domain if needed."

### Quality checks
A short checklist the researcher must run before finalising. Always include:
- Every claim has a named source or specific evidence — no vague "some organisations do X"
- Scope constraints respected throughout
- Thin or conflicting evidence flagged explicitly (not smoothed over)
- Synthesis goes beyond listing — identifies patterns, tensions, what the evidence does *not* resolve
- Output would change how someone thinks or acts, not just restate what's known

---

## Step 3 — Output format

- Use clear markdown headings
- Lead each phase with the core question it answers
- Use tables for per-entity comparisons (publisher-by-publisher, tool-by-tool, etc.)
- Use bullet lists for sub-questions within phases
- Prose for synthesis and implications
- End with quality checks as a checklist

---

## What NOT to do

- Do not conduct the research yourself
- Do not summarise the topic or provide analysis — that's the researcher's job
- Do not pad with boilerplate; every section should earn its place
- Do not leave expert lens, scope, or source priority vague
