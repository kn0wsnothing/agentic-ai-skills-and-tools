# Quarterly review

A structured strategic review skill for AI agents. Runs at the start of a new quarter, after the monthly review that closes it, to turn three months of accumulated signal into two of the most consequential rewrites in a personal system: the goals document and the area charters.

This is **where reflection turns into action.** Monthly reviews capture gaps and drift without editing anything above project altitude. Quarterly is where those gaps get acted on.

---

## What it does

1. **Cross-monthly synthesis** — two-pass AI analysis (synthesis + devil's advocate) across the quarter's three monthly reviews, surfacing which themes and goal gaps *persisted or escalated* rather than appearing once
2. **Area charter rewrite** — a short, load-bearing "purpose / scope / current attention intent" block per area, rewritten (or bootstrapped) one area at a time, gated
3. **Goals-document rewrite** — using three months of accumulated gap data, walked line by line: keep, rewrite, add, or drop each goal, with the user arbitrating every line
4. **Optional: standing-commitment checkpoint** — if you track a long-running side commitment (a venture, a big personal project) separately so it doesn't get crowded out by day-to-day urgency, check its trajectory against the quarter's lived allocation
5. **Vision touch** — read-only check of whether the lived quarter still matches your longer-horizon vision; captures a tension line for a future annual-level review, never edits vision itself
6. **Optional: audit-commitments review** — if you run the operating-audit skill, walk through which of its accepted changes actually got adopted
7. **Outputs** — a "how the quarter went" paragraph plus one forward theme that becomes standing intent for your weekly review's forward-signal step

---

## When to trigger

- The first day of a new quarter, after that quarter's closing monthly review has run
- Triggers: "quarterly review", "/quarterly-review", "let's do the quarterly", "first of the quarter review"

---

## Prerequisites

- `{workspace}/` — your root knowledge-base directory
- The quarter's three completed monthly reviews (see [monthly-review](../monthly-review/))
- A goals document and a vision document the agent can read and, for goals, rewrite
- Area hub notes the agent can add or update a charter block in
- A quarterly review note template (or adapt the structure in SPEC.md)
- **A frontier-class reasoning model.** This skill rewrites your goals document and area charters — run it on your strongest available model, not a fast/lightweight tier.
- Optional: an operating-audit report from earlier in the quarter, if you run that skill
- Optional: a standing-commitment project and decision log, if you track one

---

## Memory structure

```
{workspace}/
├── 01 Projects/
│   └── Active/                    # standing-commitment project, if any
├── 02 Areas/
│   └── {Area}/{Area}.md           # charter block rewritten here, per area
│   └── Horizons/
│       ├── Goals.md               # rewritten in Phase D
│       └── Vision.md              # touched read-only in Phase F
├── 03 Reference/
│   ├── Monthly reviews/           # this skill's primary input
│   └── Quarterly reviews/         # review notes written here
└── 90 Memory/
    └── episodic/                  # optional: a distilled close-out entry
```

Adapt paths to your own structure — see SPEC.md for customization guidance.

---

## Platform examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic (any LLM agent) | [examples/generic.md](examples/generic.md) |

---

## Design rationale

See [SPEC.md](SPEC.md) for the altitude framework, why goals get rewritten here and not monthly, and customization points.

---

## Pair with

- **monthly-review** — surfaces the gap data this skill acts on. Monthly captures; quarterly acts.
- **operating-audit** — optional input; its accepted changes get reviewed for adoption in Phase G.
- **weekly-review** — reads this skill's forward theme as standing intent for its own forward-signal step.
- **A longer-horizon annual review**, if you run one — reads this skill's captured vision-tension line as one of its primary inputs.
