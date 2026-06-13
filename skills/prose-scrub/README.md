# Prose Scrub

A fresh-eyes scrub for stakeholder-facing prose before it goes out — the enforcement layer that catches AI tells, jargon, smuggled framing, and format failures that self-administered checklists keep missing.

Two passes: a mechanical tell-scan first, then a cold editor subagent given the draft and a reader card. Returns a line-referenced findings list plus a clean rewrite.

---

## The core problem it solves

Self-administered scrubs don't work. The model that wrote "X, not Y" is asked to catch "X, not Y" — and the generative habit is blind to its own output. Adding more rules to a checklist the drafter skips changes nothing.

The fix is process:
1. A deterministic mechanical scan that can't be talked out of a hit
2. A **cold editor subagent** that did not write the prose and has no investment in the phrasing

That isolation is the whole point. Do not shortcut it by "reviewing it yourself."

---

## Genre gate: two modes

The scrub classifies the target before running either pass, because the full apparatus is destructive applied to the wrong genre.

| Genre | Mode | What runs |
|-------|------|-----------|
| **Analysis deliverable** — brief, exec summary, insights/recommendations doc | Full scrub | Tell-scan + format mandate + all six killers |
| **Relational message** — email, Slack, talk track, stakeholder ask | Light scrub | Tell-scan only; no format restructuring, no killer #2 |

**When unsure, treat as relational.** A missed bullet costs nothing; a flattened relationship costs a lot.

---

## How it works

Three steps:

1. **Mechanical tell-scan** — run a deterministic scan against the draft (a small script or grep list you maintain). Emits `file:line: [HIGH|flag] category: "match"` for each hit. Must come back clean of HIGH-severity hits before the draft ships.

2. **Fresh-eyes editor pass** — spawn a cold Opus-tier subagent. Pass it the draft, the reader card, the genre classification, and the Step 1 scan output. Not the drafting conversation. The subagent returns a line-referenced findings list and a full clean rewrite.

3. **Gate and apply** — review findings + rewrite, approve, write. Re-run the scan on the saved file; zero HIGH hits required.

---

## The reader card

The cold editor is isolated from the *drafting turns*, not from knowing who the reader is. Before the editor pass, assemble:

- **Reader(s) and relationship** — who receives this, their role
- **Shared history / state** — what's already been agreed or parked
- **Register and sensitivities** — formality, what's politically live
- **Protected framing** — diplomatic moves to preserve: collaborative deference, air-cover language, softened asks. To a blind editor these read as "hedging"; in a relational message they are the point

For analysis deliverables the reader card is lighter (audience + register). For relational messages it is mandatory and load-bearing.

---

## The six killers

1. **"X, not Y" negation cadence** — "a genuine change, not just a strong week"; "the exception, not the rule." One contrast is fine; the reflex is the tell. Rewrite as a plain declarative.
2. **Smuggled framing** — context that states the conclusion as a bare fact before the body earns it. *(Analysis deliverables only — OFF for relational prose.)*
3. **Vague-observer attribution** — "it reads as," "that looks like," "appears to." Name the view or state the fact.
4. **Internal-analyst shorthand** — "the frame," "the lever," "the cut," "the angle." Say the thing.
5. **Journalese metaphor** — "shrinking pie," "its slice," "left on the table," "did the work," "a tide."
6. **Importance-claim without the specific** — the fact that makes it matter, not "this matters / underscores."

---

## Prerequisites

Your agent needs:

- **File read/write access** to the prose being scrubbed
- **A mechanical tell-scan** — a small script or grep list covering the six killers at minimum (see Configuration)
- **Subagent spawning** — the editor pass requires spawning a cold Opus-tier subagent
- **A style guide** — your organization's canonical voice + vocabulary rules (user-supplied)

---

## Quick start

1. **Build or adopt a tell-scan** — a script or grep list that emits `file:line: [HIGH|flag] category` hits
2. **Copy `SKILL.md`** into your agent's persistent instructions
3. **Configure the paths** in the Configuration table
4. **Invoke it** — `/prose-scrub <file>` or trigger it at the end of any analysis workflow
5. **Review findings + rewrite**, then reply with your approval to apply

---

## Configuration

| Parameter | Description |
|-----------|-------------|
| `{tell_scan}` | Path to your mechanical tell-scan script or grep list |
| `{style_guide}` | Path to your canonical voice and vocabulary guide |
| `{domain_vocab}` | Your accepted domain metric vocabulary (preserved by the editor, never relabeled) |
| `{workspace}` | Root path for your notes or project files |

---

## Best practices

**Run it cold, every time.** The subagent must not have access to the drafting conversation. Isolation from the drafting turns is non-negotiable — it's the mechanism that makes the scrub work.

**Preserve domain vocabulary.** The editor's faithful-label instruction applies to genuine misuse only. Accepted terms in your domain vocabulary canon stay exactly as written — the editor never swaps them for "plainer" substitutes it invented.

**Wording restraint and format enforcement are different axes.** "Don't over-rewrite" applies to *wording*. It does not apply to *format*: a well-written sentence inside a fact-stacking paragraph is still a wall. Break it into bullets. Preserve the words; change the shape.

**The mechanical scan is the floor, not the ceiling.** A sentence can pass the scan and still read badly. The editor delivers prose a busy reader gets in one pass, not just tell-free prose.

---

## Platform examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic (any subagent-capable agent) | [examples/generic.md](examples/generic.md) |

---

## Design rationale

See [SPEC.md](SPEC.md) for the design decisions: why the genre gate matters, how the reader card emerged from real failure modes, and why the editor subagent must be Opus-tier.
