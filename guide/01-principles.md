# Principles

The mental models the system is built on. The tools change; these don't.

## Clarity is upstream of volume

AI makes it cheap to generate. That's the trap. More output doesn't mean more progress — it usually means more to sort through. The leverage is in getting the thinking clear *before* you let the machine multiply it. Most of my workflows are designed to force a moment of clarity (what's the actual question, what's the decision) before any generation happens. The skill that drafts is downstream of the skill that frames.

## The accumulation engine is separate from, and upstream of, the production engine

There are two engines. The **accumulation engine** is everything that builds up understanding over time: notes, reviews, captured ideas, the running record. The **production engine** is what ships: posts, drafts, analyses, decisions. The mistake is running only the production engine — generating constantly, accumulating nothing. When the accumulation engine runs well, production becomes a byproduct. You're not staring at a blank page; you're drawing from a stocked one.

## Inspectable workflows beat chatting with a bot

A chat is ephemeral and unrepeatable. A workflow is a written-down procedure you can read, critique, version, and improve. Every skill in this repo is a structured prompt, not a clever one-off conversation. That's what makes the system legible — to me, and to anyone I hand it to. If you can't read what a workflow does, you can't trust it or improve it.

## Tools are interchangeable; the contract is not

I assume every specific tool will be replaced. So the system is designed against **roles** and a **shared-memory contract**, never against a product's features. A skill says "write to the daily note," not "call this app's API." That's why these port: the workflow is the same whether the agent is Claude, Gemini, or something newer, and whether the memory is Obsidian or a folder of files.

## Keep humans on the judgment, give agents the mechanical parts

The split that works: agents handle volume, recall, drafting, and structure; I keep the judgment, the taste, and the decisions. Several skills enforce this directly — they propose and wait, rather than write silently. The point of the system isn't to remove me from the loop. It's to spend my attention only where attention actually matters.

## Under-specify on purpose

The instinct when a skill misbehaves is to add a line telling it what to do instead. Do that enough times and the skill stops being a procedure and becomes a script that tells the model what to conclude before it has looked at anything. I audited every skill I run and cut around sixty-six of these — lines that pre-loaded a verdict, mandated a shape, or asked the model to distrust its own reading. Almost all of them had been added in good faith after a single bad output.

The replacement rule is narrower than it sounds: say what to look at and what the gate is; do not say what will be found. And when a judgment step is misfiring, the fix is usually to remove the input that produced the bad judgment, not to add a caveat teaching the step to doubt itself. A caveat is a second layer of reasoning that has to be right about the first one, which is a harder problem than the original.

## A failure written down is worth more than a success

Most of these skills carry a short record of how they broke before — the slug that should never be a number, the line that doubled a bullet, the gate that got skipped. Encoding hard-won failures into the workflow is how the system stops repeating them. That's the compounding: not just accumulated notes, but accumulated lessons.

Next: [how this maps to Workflows, Agents, and Tools](02-the-system.md).
