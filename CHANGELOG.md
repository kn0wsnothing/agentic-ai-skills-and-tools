# Changelog

How this system has changed over time. Newest first. Each month is tagged `vYYYY.MM` so you can check out any past state of the system.

This is the "what changed" half of the showcase — the [guide](guide/) is the current state, this is the diff.

## v2026.09 — 2026-10-10

**Review crosses the line, and four engines go public.** Covers August and September.

- **Every handoff gets a challenger pass, and review crosses roles** ([the system](guide/02-the-system.md)). Before a spec reaches the builder, a challenger reads it and returns findings only, and the author can't waive it. Finished work is reviewed by a side that didn't write it. A finding blocks only when it breaks what the handoff promised; the rest become follow-ups.
- **Four open engines** ([README](README.md#open-engines)): [prose-engine-open](https://github.com/kn0wsnothing/prose-engine-open), [information-diet-open](https://github.com/kn0wsnothing/information-diet-open), [gtd-engine-open](https://github.com/kn0wsnothing/gtd-engine-open) and [content-engine-open](https://github.com/kn0wsnothing/content-engine-open), published so anyone can run them with their own data.
- **Adopted, not ported: a six-stage method for big builds** ([what changed](guide/04-whats-changed.md)). Goal, spec, evaluation, verification, environment, guardrails, adapted from [karpathy-method-SPEC](https://github.com/batsyxxx/karpathy-method-SPEC). The original has no licence, so it isn't copied here.

## v2026.07 — 2026-08-02

**A third agent role, a principle about restraint, and an honest retirement.**

- **Added [`salon`](skills/salon/)** — one conversational thinking session covering open questions, books, films, and ideas. It absorbed the narrower `open-questions` skill rather than sitting beside it.
- **Retired `open-questions` to [`archive/`](archive/)** — the "only ship what I actually use" rule applied to my own back catalogue. Two similar-but-different skills side by side is a design smell.
- **Added [`quarterly-review`](skills/quarterly-review/)** — completing the review ladder. The monthly captures goal gaps; the quarterly is the rung that actually rewrites the goals and the area charters, using three monthlies' accumulated evidence rather than one month's mood.
- **New principle: [under-specify on purpose](guide/01-principles.md).** Audited every skill I run and cut around sixty-six forcing lines — instructions that told the model what to conclude instead of what to look at. Almost all had been added in good faith after one bad output. Also recorded the corollary: when a judgment step misfires, remove the input that misled it rather than adding a caveat that teaches it to distrust itself.
- **The system is now three roles, not two** ([the system](guide/02-the-system.md)). A building agent joined the thinking/specifying agent and the data agent, once the thinking agent's output became routinely a spec rather than a deliverable. One thinks and specifies, the other builds; the handoff is deliberately manual.
- **Recorded three structural changes in [what changed](guide/04-whats-changed.md):** reflexes that fire at the moment of use instead of instructions that have to be remembered; task state moved out of the notes into a store of its own, with the notes as a one-way projection; and the pre-publish prose scrub graduating from a skill into a standalone register-aware engine.

## v2026.06 — 2026-06-13

**Relaunch as a living showcase of my current system.**

- Reframed the repo from a small OpenClaw-era skill drop into the inspectable, versioned record of how I actually work with agentic AI now.
- Added the **Agents layer**: a new [`prompts/`](prompts/) directory for the role/persona prompts I use, alongside [`skills/`](skills/) (Workflows). README + CONTRIBUTING now cover both.
- Added a narrative [`guide/`](guide/) — the system in roles (a thinking/prose agent, a data agent, a shared memory), the principles underneath, one real workflow, and what changed since OpenClaw.
- Ported the skills and prompts I genuinely use today (recency-checked, sanitized, made platform-agnostic).
- **Honest reset:** moved the two OpenClaw-era skills I no longer run (`memory-dream-cycle`, `nightly-distillation`) to [`archive/`](archive/) and dropped the "battle-tested in daily production" claim that no longer described them.
- Started versioning with this changelog + monthly tags.
