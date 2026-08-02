# Quarterly review — Claude Code

## Setup

### 1. Install the skill

```bash
mkdir -p {workspace}/skills/quarterly-review
cp /path/to/SKILL.md {workspace}/skills/quarterly-review/
```

### 2. Add to CLAUDE.md

```markdown
### Quarterly review
When asked to run the quarterly review (or triggered on the first of a new
quarter, after that quarter's closing monthly review has run), follow the
instructions in `skills/quarterly-review/SKILL.md`. Interactive — one step
at a time, hard gate every decision.
```

### 3. Configure your paths

Open `SKILL.md` and set `{workspace}` to your actual root knowledge-base directory. Update any path that doesn't match your structure:

| Default | Your equivalent |
|---|---|
| `{workspace}/03 Reference/Quarterly reviews/` | Where quarterly reviews live permanently (never archived) |
| `{workspace}/03 Reference/Monthly reviews/` | Where your monthly-review skill writes its notes |
| `{workspace}/02 Areas/Horizons/Goals.md` | Your goals document |
| `{workspace}/02 Areas/Horizons/Vision.md` | Your vision document |
| `{workspace}/02 Areas/{Area}/{Area}.md` | Your area hub notes (charter block target) |

### 4. Decide which optional phases apply

- **Phase E (standing-commitment checkpoint):** only wire this up if you track a long-running side commitment separately, with a lightweight decision log. Otherwise tell the agent to skip it — or just delete the phase from your copy of `SKILL.md`.
- **Phase G (audit commitments):** only applies if you also run the [operating-audit](../operating-audit/) skill. Skip otherwise.

---

## Running

### Manual (first of the quarter)

```
Run the quarterly review. Follow skills/quarterly-review/SKILL.md.
```

Before anything else, the agent checks two hard preconditions: the closing monthly review exists, and (if you use it) the operating audit ran in the first two weeks of the new quarter. If either is missing, it stops and tells you which one to run first.

Next, it confirms you're on a strong reasoning model — switch if you're not, then say "go."

**Typical session length: 60–120 minutes.** This is the most consequential review in the rhythm — it rewrites your goals document and your area charters. Budget real time for it.

### Scheduled (unattended start, interactive completion)

```bash
# Cron job — runs on the 1st of each quarter at 8am
0 8 1 1,4,7,10 * cd /path/to/workspace && claude --print --model claude-opus-4-6 \
  "Start the quarterly review. Run pre-flight only — verify the closing monthly \
   and the operating audit exist, create the review note, then stop at the \
   Phase A gate." \
  >> /var/log/quarterly-review.log 2>&1
```

The agent creates the review note and halts at the first real gate. You then pick up interactively.

---

## What the agent reads

For Q2 2026 (running July 1):

- The three monthly reviews covering April, May, and June (matched by their stored month field, not filename arithmetic)
- The previous quarterly review, if one exists
- `02 Areas/Horizons/Goals.md` and `Vision.md`
- The charter block in each `02 Areas/{Area}/{Area}.md`
- The Q2-in-window operating-audit report, if you run that skill
- Your standing-commitment project and decision log, if configured

---

## What it produces

A review note at `03 Reference/Quarterly reviews/2026-07-01.md`:

```markdown
---
type: quarterly-review
quarter: 2026-Q2
run_date: 2026-07-01
feeds_annual: true
vision_tension: "The venture keeps losing to reactive work; vision assumed it would have real runway by now."
---

# Quarterly review — 2026-Q2

## Quarter themes
- **Reactive work crowding deliberate work** — surfaced in all three monthlies (April: "day got eaten by the migration again"; May: "same pattern, different fire"; June: "I keep saying this will change and it doesn't").
- **Health holding steady** — present but not escalating; noted as a weak signal in April, absent by June.

## Areas / charter changes
- **Work:** rewritten — current attention intent lowered from "primary" to "primary, with a hard cap," reflecting the quarter's overload pattern.
- **Health:** left as is — the monthlies' rollup didn't show enough drift to justify a rewrite.
- **Creative practice:** bootstrapped — no charter existed before this run.

## Goals rewrite record
- "Ship the redesigned onboarding flow" — kept, on track per all three monthlies.
- "Write monthly" — rewritten to "Write when a real idea is ready, not on a fixed schedule" — the fixed-cadence version generated a stated-but-contradicted gap in May and June.
- "Learn woodworking" — dropped — zero occurrences across the quarter, and the user couldn't explain in 30 seconds why it still belonged.

## Venture checkpoint
Re-evaluation trigger "10 hours/month sustained for two months" did not fire — averaged 3.5 hours/month. Disposition: stays on hold, revisit at the next quarterly.

## Vision tension
The venture keeps losing to reactive work; vision assumed it would have real runway by now. Carried forward for the annual review.

## How the quarter went
Q2 was the quarter reactive work finally got named instead of just felt. The work charter rewrite is a hedge against a pattern that showed up in every single monthly this quarter — not a values shift, just an honest cap. The venture stayed exactly where it's been for two quarters running, and this time the vision-tension line says so plainly instead of hoping the next quarter fixes it on its own.

## Forward theme
Prove the work charter's cap holds for one full quarter before touching anything else — no new area commitments until that's tested.
```

Plus a breadcrumb on today's daily note:
```
09:15 — Quarterly review for 2026-Q2 complete. Goals document + 3 charters rewritten. Forward theme: prove the work cap holds. #quarterly-review
```

---

## WAT mapping

| Layer | Implementation |
|---|---|
| **Workflow** | `SKILL.md` — the 9-phase interactive review pipeline |
| **Agent** | Claude (a frontier-class model — this rewrites foundational documents) |
| **Tools** | Claude Code's file read/write, directory listing, shell access |

---

## Tips

- **Don't run this on a fast/lightweight model.** The charter and goals rewrites need real judgment about which evidence is strong versus thin — a weaker model will accept the synthesis at face value instead of pushing back in the devil's-advocate pass.
- **The goals rewrite is the point — don't rush past it.** If a gap genuinely isn't ready to be answered, capture it as a known unknown rather than skipping it silently. Deferring the whole rewrite defeats the reason monthly reviews spent three months capturing gap data.
- **Skip conditional phases cleanly.** If you don't track a standing commitment or run the operating-audit skill, tell the agent to skip Phases E and G — don't force a thin version of either just to fill the space.
- **The vision-tension line compounds.** It only pays off once a future annual review (or four accumulated quarterlies) reads it. Write it honestly even when the honest answer is "still holds" — a string of "still holds" is itself useful signal.
