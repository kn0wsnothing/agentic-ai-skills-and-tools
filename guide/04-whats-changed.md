# What changed

This system didn't arrive fully formed. Some of it started on [OpenClaw](https://github.com/openclaw/openclaw), a personal AI orchestrator, and a lot has moved since. Showing the change is half the point of this repo — a system you can watch evolve is more honest, and more useful, than a polished snapshot that hides its history.

This page is the human narrative of the evolution. The [CHANGELOG](../CHANGELOG.md) is the machine-readable version, tagged by month so you can check out any past state.

## Dropped

- **Heavy nightly memory automation.** The OpenClaw era leaned on automated overnight memory consolidation and distillation (those skills now live in [`archive/`](../archive/), honestly labelled). In practice I stopped running them — the session-close workflow does the capture I actually need, when I need it, without an opaque overnight process rewriting my memory.
- **Agent sprawl.** Earlier I ran more agents than the work justified. Fewer, clearer roles beat more agents.
- **Workflows that were ceremony.** Several review steps existed because they sounded thorough, not because they changed a decision. The reviews got shorter and the cuts were deliberate.

## Stuck

- **Plain-text, version-controlled memory as the spine.** This was right from the start and hasn't moved. Everything else is replaceable; this isn't.
- **Propose-first writing.** Agents propose, I approve, then they write. Across every iteration this stayed.
- **The GTD-shaped rhythm** — daily and weekly reviews as the heartbeat. The implementation changed; the cadence held.
- **Honesty about origins.** Keeping the archived skills visible rather than deleting them is the same instinct that started the repo.

## New

- **The prose/data split as an explicit, governed boundary** — not just two tools, but a deliberate separation of roles, partly so sensitive data work stays where it belongs.
- **A self-auditing layer.** [`operating-audit`](../skills/operating-audit/) and [`chief-of-staff-brief`](../skills/chief-of-staff-brief/) are the system looking at itself — diagnosing friction, checking that the governance layer earns its keep instead of becoming more ceremony.
- **Tool-agnosticism as a design rule, not an accident.** Getting burned by tool churn taught me to build against roles and contracts. This whole repo is that lesson applied.
- **This showcase itself** — versioning the system in public so it can be inspected and so the changes are legible.

The system will keep moving. That's the design, not a disclaimer. Check the [CHANGELOG](../CHANGELOG.md) for what moved last.
