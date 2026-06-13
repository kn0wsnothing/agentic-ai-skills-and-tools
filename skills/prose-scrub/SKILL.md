# Prose Scrub

You are running the Prose Scrub — a two-pass enforcement layer for stakeholder-facing prose. Your job: find and remove AI tells, jargon, smuggled framing, and format failures, then produce a clean rewrite. A cold subagent does the judgment pass; you orchestrate.

---

## Why two passes (read once)

Self-administered scrubs don't work. The model that wrote "X, not Y" is asked to catch "X, not Y" — and the generative habit is blind to its own output. The fix is process: a deterministic mechanical floor plus a **cold editor subagent** that did not write the prose and has no investment in the phrasing. Do not shortcut the cold pass by reviewing it yourself.

---

## Step 0 — Genre gate and reader card

Do this before running either pass.

### Genre classification

Classify the target:

- **Analysis deliverable** (exec summary, brief, insights or recommendations doc): **full scrub.** Format mandate ON, all six killers ON.
- **Relational message** (email, Slack, talk track, stakeholder ask): **light scrub only.** Tell-scan for genuine AI tells; no format restructuring, no killer #2. Leave the shape alone.
- **Essay / other prose:** tells only, no format mandate.

**When unsure whether a doc is relational, treat it as relational.** A missed bullet costs nothing; a flattened relationship costs a lot.

If a target arrives labeled "analysis deliverable" but is actually a relational message (an email, a talk track), downgrade and say so.

### Reader card

The cold editor is isolated from the *drafting turns*, not from knowing who the reader is. Assemble from the project — not from the drafting conversation:

- **Reader(s) and relationship** — who receives this, their role
- **Shared history / state** — what's already been agreed or parked
- **Register and sensitivities** — formality, what's politically live
- **Protected framing** — moves to preserve: collaborative deference, air-cover language, softened asks, shared-history callbacks. To a blind editor these read as "hedging"; they are the point. Do not flag or strip them.

For analysis deliverables the reader card is lighter (audience + register). For relational messages it is mandatory and load-bearing.

---

## Step 1 — Mechanical tell-scan (deterministic floor)

Run your tell-scan against the target file:

```bash
{tell_scan} "<path to draft>"
```

It should emit `file:line: [HIGH|flag] category: "match" — hint` for each hit and exit non-zero if any HIGH-severity tell is present. Capture the output — it seeds Step 2.

For a **relational message**, ignore format flags (`fact-stack`, `paragraph-wall`). Act only on tell flags (cadence, vague-observer, journalese). The HIGH-severity tell gate still applies.

---

## Step 2 — Fresh-eyes editor pass (judgment)

Spawn a **cold** editor subagent at Opus tier. Pass it:

- The draft text
- The reader card (from Step 0)
- The genre and which rules are ON
- The Step 1 scan output
- **Not** the drafting conversation

Pick the prompt template by genre.

### Full scrub (analysis deliverable)

> You are a ruthless copy editor. You did **not** write the document below and have no attachment to its phrasing. Your only job is to find and fix the prose failures, then rewrite it clean.
>
> Reader card (who this is for — preserve what it protects): {paste reader card — readers, relationship, shared history, register, protected-framing list}. The protected-framing items are NOT tells. Do not flag or strip collaborative deference, air cover, ownership language, or shared-history callbacks. Fix the prose for this reader, not an imaginary generic one.
>
> Target format: {paste the format spec for the relevant document type — BLUF first, bold claim-leads, fact-stacking paragraphs become bullet lists, ≤2 non-lead sentences per insight paragraph, etc.}.
>
> The six recurring killers to hunt: {paste the six killers below}.
>
> Also apply these judgment checks: fix manufactured "we" in solo analysis (the analyst's own actions are "I", not "we"); fix unfaithful metric labels (but preserve your domain vocabulary canon — accepted terms stay exactly as written, never relabeled to "plainer" substitutes); flag undefined corpus or method terms for inline definition.
>
> PRESERVE domain vocabulary: the accepted terms in {domain_vocab} stay exactly as written. The faithful-label fixes apply only to genuine misuse, never to accepted funnel-stage or measurement labels.
>
> Structural mandate (enforce it): any paragraph carrying ≥2 discrete facts becomes a bulleted or numbered list with bold leads. No insight paragraph beyond the bold claim-lead may exceed 2 sentences (anticipated-question answers and context sections excepted). A clean sentence inside a fact-stacking paragraph is still a format failure — restructure it. "Don't over-rewrite" applies to wording, not format.
>
> A mechanical scan already flagged these lines (fix every one): {paste Step 1 output}.
>
> Return exactly three things:
> 1. **Structure map** — per section: heading, paragraph count, max sentences in any non-lead paragraph, bullet count. Flag every section with a non-lead paragraph over 2 sentences or with fact-stacking prose and no bullets.
> 2. **Findings** — a line-referenced list: `line N — <tell> — <why it's a tell>`. Include judgment tells the scan can't catch (smuggled framing, weak metaphor, buried lede, jargon the reader can't parse).
> 3. **Rewrite** — the full document in the target format: scannable, plain English, every killer removed, every fact-stacking paragraph turned into bullets, every number preserved exactly. Do not invent numbers or claims. Do not soften an honest limit — state it straight in a "Yes, but." block.
>
> Plain readability is the bar, not merely tell-free prose. Read every sentence as if saying it to a colleague out loud; if it's awkward, rephrase it.

### Light scrub (relational message)

> You are a careful copy editor. You did **not** write the message below. Hunt ONLY the genuine AI tells and fix them in place: "X, not Y" cadence, vague-observer attribution ("it reads as," "appears to"), journalese metaphor, and importance-claims with no specific. Everything else is light touch.
>
> Reader card (who this is for): {paste reader card}. This is a **relational message** — its job is to carry an ask and a relationship, not to score on format. Do NOT bullet-ify paragraphs, do NOT impose a structural template or a word cap, do NOT flag or strip collaborative deference, air cover, softened asks, or shared-history callbacks. Those are the point. The format flags in the scan output are noise here; act only on genuine tell flags.
>
> A mechanical scan already flagged these lines: {paste Step 1 output}.
>
> Return two things:
> 1. **Findings** — a line-referenced list of genuine tells only.
> 2. **Rewrite** — the same message with those tells fixed and everything else (tone, structure, framing, length) left intact. If a sentence already reads like something a person would say to this reader, leave it.

---

## The six killers

1. **"X, not Y" negation cadence** — "a genuine change, not just a strong week"; "the exception, not the rule"; "rather than," "more than a." One contrast is fine; the reflex is the tell. Rewrite as a plain declarative.
2. **Smuggled framing** — context that states the conclusion as a bare fact before the body earns it; a bottom line that asserts a read a later section walks back. *(Analysis deliverables only — OFF for relational prose.)*
3. **Vague-observer attribution** — "it reads as," "that looks like," "appears to." Name the view or state the fact.
4. **Internal-analyst shorthand as a bare noun** — "the frame," "the lever," "the cut," "the angle," "the engine." Say the thing.
5. **Journalese metaphor reaching for vividness** — "shrinking pie," "its slice," "left on the table," "did the work," "a tide."
6. **Importance-claim without the specific** — the fact that makes it matter, not "this matters / underscores."

---

## Step 3 — Reconcile and present (gate)

Read the subagent's findings and rewrite. Sanity-check: every number and claim preserved, no fabricated evidence, honest limits still present, format on target. Present findings + rewrite to the user. Wait for approval before writing.

---

## Step 4 — Apply and re-scan

On approval, write the rewrite. Re-run Step 1 on the saved file. It must come back with **zero HIGH-severity hits**. A remaining hit means the rewrite reintroduced a tell — fix and re-scan until clean. For relational messages the zero-HIGH-tell bar still holds; format flags stay ignored.

---

## Configuration

| Parameter | Description |
|-----------|-------------|
| `{tell_scan}` | Path to your mechanical tell-scan script or grep list |
| `{style_guide}` | Path to your canonical voice and vocabulary guide |
| `{domain_vocab}` | Your accepted domain metric vocabulary file |
| `{workspace}` | Root path for your notes or project files |

---

## Model

Run the orchestration on the session's normal model. **The editor subagent must be Opus-tier** — this is fresh-eyes delegation for quality, not token savings. Do not route it to a smaller model.

---

## Notes

- This skill does not change interpretation. It fixes how the prose reads, never what it claims. If the editor believes a claim is wrong (not just badly phrased), it surfaces that as a finding — it does not silently rewrite the read.
- The scrub does not relocate caveats. They stay where the document's conventions put them (a "Yes, but." block inline, or a supporting section at the back). The scrub ensures they're stated plainly, not buried or softened.
- Wording restraint and format enforcement are different axes. "Don't over-rewrite" applies to wording. A clean sentence inside a fact-stacking paragraph is still a wall — breaking it into bullets is mandatory, not "churning."
