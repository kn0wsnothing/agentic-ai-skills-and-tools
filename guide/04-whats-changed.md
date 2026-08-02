# What changed

This system didn't arrive fully formed. Some of it started on [OpenClaw](https://github.com/openclaw/openclaw), a personal AI orchestrator, and a lot has moved since. Showing the change is half the point of this repo — a system you can watch evolve is more honest, and more useful, than a polished snapshot that hides its history.

This page is the human narrative of the evolution. The [CHANGELOG](../CHANGELOG.md) is the machine-readable version, tagged by month so you can check out any past state.

## Dropped

- **Heavy nightly memory automation.** The OpenClaw era leaned on automated overnight memory consolidation and distillation (those skills now live in [`archive/`](../archive/), honestly labelled). In practice I stopped running them — the session-close workflow does the capture I actually need, when I need it, without an opaque overnight process rewriting my memory.
- **Agent sprawl.** Earlier I ran more agents than the work justified. Fewer, clearer roles beat more agents.
- **Workflows that were ceremony.** Several review steps existed because they sounded thorough, not because they changed a decision. The reviews got shorter and the cuts were deliberate.
- **Over-specified skills.** A later, harder version of the same cut: around sixty-six lines removed across every skill I run, all of them instructions that told the model what to conclude rather than what to look at. See [under-specify on purpose](01-principles.md).
- **Two similar skills sitting side by side.** A narrow single-question thinking skill was folded into the broader conversational one rather than maintained alongside it. Two things that are almost the same is a design smell, not a richer toolkit.

## Stuck

- **Plain-text, version-controlled memory as the spine.** This was right from the start and hasn't moved. Everything else is replaceable; this isn't.
- **Propose-first writing.** Agents propose, I approve, then they write. Across every iteration this stayed.
- **The GTD-shaped rhythm** — daily and weekly reviews as the heartbeat. The implementation changed; the cadence held.
- **Honesty about origins.** Keeping the archived skills visible rather than deleting them is the same instinct that started the repo.

## New

- **The prose/data split as an explicit, governed boundary** — not just two tools, but a deliberate separation of roles, partly so sensitive data work stays where it belongs.
- **A self-auditing layer.** [`operating-audit`](../skills/operating-audit/) and [`chief-of-staff-brief`](../skills/chief-of-staff-brief/) are the system looking at itself — diagnosing friction, checking that the governance layer earns its keep instead of becoming more ceremony.
- **Tool-agnosticism as a design rule, not an accident.** Getting burned by tool churn taught me to build against roles and contracts. This whole repo is that lesson applied.
- **A third role: the builder.** Once the thinking agent's output was routinely a spec rather than a deliverable, a separate building agent earned its place. One thinks and specifies, the other builds. The handoff is deliberately manual.
- **Reflexes instead of reminders.** Some facts about an environment kept getting forgotten no matter how prominently they were written down. The fix was to stop writing them as instructions and start firing them as hooks at the moment of use — indexed to the action, not to the session. A rule that fires when it is needed beats a rule that has to be remembered.
- **Task state moved out of the notes.** The vault had been carrying task state in note text, which meant every agent parsed it slightly differently. It now lives in a small store of its own with the notes as a one-way projection: next-ness is computed rather than tagged, and blocking is a typed condition rather than a word. The notes got their job back, which is thinking.
- **Prose-scrub outgrew being a skill.** The pre-publish scrub became a standalone, register-aware engine with its own tests, so the same language rules apply whether they are invoked by a skill, a hook, or a script. The skill in this repo is still the portable version of the idea.
- **This showcase itself** — versioning the system in public so it can be inspected and so the changes are legible.

The system will keep moving. That's the design, not a disclaimer. Check the [CHANGELOG](../CHANGELOG.md) for what moved last.
