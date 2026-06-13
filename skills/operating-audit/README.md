# Operating Audit

A behavioral audit skill for AI agents. Extracts evidence from your actual notes, logs, and project files, then diagnoses friction — not a cheerleader. Outputs ranked changes with adoption cost, a dogfood check on the governance layer itself, and the one thing you're avoiding.

Quarterly cadence or on demand.

---

## How it works

Four steps run in sequence:

1. **Evidence pipeline** — extract telemetry, daily notes, session logs, and active project state over the audit window. Never analyze from memory — extract first.
2. **Diagnosis** — find the friction: loops that never close, stated intent vs. actual, repeated deferrals, process without value, waiting-for rot.
3. **Deliverable** — honest read, ranked findings with citations, changes to try with adoption cost, the one thing being avoided.
4. **Close the loop** — on accepted changes, implement in-session. Log the audit.

---

## Stance (non-negotiable)

**Analyst, not cheerleader.** No validation, no hedging.

**Volume is not slippage.** Build output and system work are craft, not findings. The auditable questions are routing (did margin go where intent earmarked it?) and payback (do builds' `unblocks:` claims cash out?). Never pathologize metawork volume itself.

**Evidence-first.** Every finding names dates, files, or dashboard rows. Cut anything not tied to cited evidence.

**If the honest read is "mostly working," say that.** Partial credit is stated plainly. Don't invent problems.

**Dogfood clause.** The audit evaluates the governance layer against its own stated value — the same standard it applies to everything else. Any governance component that can't show concrete decision-changing instances across two consecutive audits gets a deletion recommendation.

---

## Prerequisites

Your agent needs:

- **File read/write access** to your workspace
- **Daily notes** — one file per day, covering the audit window
- **Session or activity logs** — dated records of what the agent actually did
- **Active project files** — current next actions, waiting-fors, session log recency
- **A governance layer** (optional but expected) — health dashboard, process briefs, activity ledger, or whatever structure you use to govern how you work. The audit evaluates this too.
- **A health-check script or aggregator** (optional) — if you have one, the audit runs it first for fresh telemetry

---

## The dogfood clause

Every governance component — dashboard, briefs, triage gate, ledger — gets evaluated against one question: did it change a concrete decision in this window? A nudge sent, a project parked, an item dropped, a build redirected.

Two consecutive audits with no decision-changing instance → recommendation to delete that component. The governance layer lives under the same rules it enforces.

---

## Evidence window

Default: since the last audit (approximately 12 weeks). On first run, use whatever window your notes cover — even a few weeks produces useful signal.

---

## Quick start

1. **Copy `SKILL.md`** into your agent's persistent instructions
2. **Configure the paths** (see Configuration below)
3. **Trigger manually** — say "operating audit" or "audit how I work"
4. **Review the deliverable** — correct citations, push back on readings
5. **Accept or reject changes** — implement accepted ones in-session

---

## Configuration

| Parameter | Example | Description |
|-----------|---------|-------------|
| `{workspace}` | `~/notes/` | Root of your notes or PKM workspace |
| `{daily_notes}` | `{workspace}/daily/` | Daily note files (YYYY-MM-DD.md) |
| `{logs}` | `{workspace}/agent-logs/` | Agent session log files |
| `{projects}` | `{workspace}/projects/active/` | Active project notes |
| `{governance}` | `{workspace}/system/` | Governance layer files (dashboards, briefs, ledger) |
| `{health_script}` | _(optional)_ | Path to a health-check or aggregator script |
| `{audit_window_weeks}` | `12` | How far back to look (default: since last audit) |

---

## Output structure

```
1. The honest read      — one paragraph, what's working
2. The friction         — 3–6 findings with dated citations
3. Changes to try       — 3–5, ranked by leverage, each with evidence + adoption cost
4. The one thing you're avoiding — exactly one, the sharpest evidence-backed tension
```

The deliverable is shown in-session first. Vault write only on explicit request.

---

## Platform examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic (any file-access agent) | [examples/generic.md](examples/generic.md) |

---

## FAQ

**How is this different from a retrospective prompt?**
Three things: it extracts evidence before analyzing (never from memory), it evaluates your governance layer against its own stated value (dogfood clause), and it ends with exactly one thing you're avoiding — not a list of vague suggestions.

**What if the honest read is "things are mostly working"?**
That's the output. The skill says so plainly. The test is whether the findings are grounded — not whether there are enough of them to seem rigorous.

**Do I need a governance layer for the audit to run?**
No. The evidence pipeline and diagnosis run on daily notes, logs, and project files alone. If you have a governance layer, the audit evaluates it too. If you don't, that section is skipped.

**How often should I run this?**
Quarterly is the intended cadence — long enough for real patterns to emerge, short enough to catch friction before it compounds. On-demand runs are also valid when something feels stuck.

---

## Design rationale

See [SPEC.md](SPEC.md) for the design decisions: why evidence extraction comes before analysis, why volume is explicitly excluded from findings, the dogfood clause rationale, and the "one thing avoided" close.
