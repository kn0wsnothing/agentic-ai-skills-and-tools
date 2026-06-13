# Plan Mode

A hard-constraint system prompt that locks an agent into specification-only mode: it thinks, clarifies, and documents — but does not act, execute, or modify anything until explicitly released.

## What this does

Enforces a clean separation between the planning phase and the execution phase of any agentic task. The agent:
- Asks all clarifying questions upfront in a single batch
- Maps edge cases, failure modes, dependencies, and risks
- Produces a structured plan document with goal, current state, proposed changes, execution steps, rollback, out-of-scope items, and open questions

It will not write a file, run a command, or make any change — even a small one — until you explicitly exit plan mode.

## When to use it

- Any task where acting on a misunderstanding is costly or hard to reverse
- System changes, infrastructure modifications, refactors across many files
- Any time you want a second opinion before an agent touches something real
- Onboarding an agent to a complex system: get the plan out of it first, review it, then let it run

## How to use it

Prepend this prompt to your system prompt or first message before describing the task. The agent will acknowledge plan mode and ask its clarifying questions before producing any output.

To exit plan mode: tell the agent explicitly ("exit plan mode", "go ahead", "execute"). The prompt does not define what happens after exit — that is intentional. You pair this with your normal execution instructions once the plan is confirmed.

## The hard stop rule

The prompt includes an explicit override refusal. If you tell the agent "just do this one small thing", it will decline and re-anchor to plan mode. This is the point. The value is in the constraint being unconditional.

## Output format

The plan is produced as a structured document with eight sections. Adapt the frontmatter block to whatever format your system uses (YAML, TOML, none). The section structure is the part that matters — it forces completeness before execution begins.

## Key design decisions

**Single-batch clarification.** All questions come at once. No back-and-forth clarification drip that lets the agent start building a mental model of a partial spec. You answer everything, then the plan is written.

**Rollback is mandatory.** Every plan must include how to undo it. If the agent cannot write a rollback section, that is a signal the plan is not ready to execute.

**Open questions are a first-class output.** Unresolved decisions don't get smoothed over — they surface as a named list at the end of the plan. Execution cannot begin until those are answered.
