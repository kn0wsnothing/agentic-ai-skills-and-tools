# The system in WAT terms

This repo is organized around the **WAT framework** — Workflows, Agents, Tools. It's a clean way to describe the system without naming a single product.

## Agents — the roles, not the brands

I run three agent roles, deliberately separated:

- **A thinking/specifying agent.** Drafting, reviewing, planning, reflection, and — increasingly — writing the spec that something else builds from. This is the one I'm in conversation with. The [`prompts/`](../prompts/) directory is mostly this layer: role/persona definitions you drop into whatever model you're driving — a thinking partner, an analysis architect, a prose editor, a coach.
- **A building agent.** Added as a distinct role once the thinking agent's output started routinely being a spec rather than a deliverable. It is faster and more literal, and it is the wrong surface for open-ended design — hand it a settled spec and it executes well, hand it an ambiguous one and it produces something confident and shallow. The division that works: one thinks and specifies, the other builds. Review crosses the line too. Before a spec is handed over, a challenger pass reads it and returns findings only, and that check can't be waived however small the change. Afterwards the work is reviewed by a side that didn't write it: the building side reviews the thinking side's specs and drafts, and code gets a fresh reviewer. A finding stops a build only when it shows the result breaks what the handoff promised; anything else becomes a follow-up.
- **A data agent.** Analysis, data pulls, structured/numeric work. Kept separate from the prose agent for two reasons: focus (a data run and a drafting session want different headspaces) and governance (the data agent operates where the data lives, under whatever rules that data carries).

The key move is that these are *roles*. Each of mine happens to be a different product right now, but the system would survive swapping any of them. If you only have one model, you can still run all three roles — just be explicit about which hat it's wearing, because the failure mode is asking the builder to think.

## Tools — your platform's capabilities

Tools are what an agent can actually *do*: read and write files, run shell commands, hit an API, search, schedule a job. The skills and prompts here assume as little as possible about this layer — mostly just file read/write and persistent storage. That's intentional. The more a workflow leans on one platform's special features, the less portable it is.

The one tool that matters most isn't flashy: it's **persistent, shared, plain-text memory**. A notes vault under version control. Both agent roles read and write it. It's the spine — see below.

## Workflows — the skills

Workflows are the [`skills/`](../skills/): structured procedures that say what happens, in what order, under what conditions. A workflow doesn't reason (that's the agent) and doesn't act directly (that's the tools) — it orchestrates. Keeping that separation is what makes a skill portable and legible.

The skills cluster into a few families:

- **Rhythm** — morning review, work-closure, day-closure, weekly review, monthly review, quarterly review. A ladder: each rung captures what the rung above it acts on, so the monthly surfaces gaps and the quarterly is what actually rewrites the goals.
- **Memory** — breadcrumb (session wrap-up + memory routing). How a session's outcomes get written back so the next one isn't cold.
- **Thinking & writing quality** — salon, prose-scrub, voice-development, the newsletter pipeline (outline → draft → package), process-notes.
- **Governance** — operating-audit, chief-of-staff-brief. The system looking at itself.

## The substrate — shared memory

Underneath all three layers is one shared memory: a structured, version-controlled, plain-text vault. It's where the agents meet. The prose agent writes a draft; the data agent's output lands beside it; a review skill reads across both; a wrap-up skill routes the durable bits into the right place. Because it's plain text under git, it's inspectable, diffable, and portable — and because both agents use the same store, they stay continuous instead of each holding a private, lossy context.

This is the part people underestimate. The agents are replaceable. The accumulating, structured memory is the asset.

Next: [one workflow walked end to end](03-a-real-workflow.md).
