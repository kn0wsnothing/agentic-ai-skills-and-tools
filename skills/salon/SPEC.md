# Salon — design spec

This document explains the design decisions behind the Salon skill.

---

## The problem

Conversational thinking sessions come in a handful of recognizable shapes — sitting with a hard question, working through a book or film you just encountered, following a half-formed idea, or just wanting to talk without a task attached. Most systems either force you to pick a mode up front (which breaks the conversational opening before it starts) or collapse everything into one generic "chat" mode that has no real posture at all.

Salon is one skill with one posture and several lanes, chosen silently from how you open. You never announce what kind of session this is. The skill figures it out and gets out of the way.

This design absorbed an earlier, narrower skill that handled only the single-question lane (see [open-questions](../open-questions/), kept here as a standalone option for anyone who only needs that one lane). The choreography for that lane is carried forward faithfully — the change is that it's no longer the only door in.

---

## Why one skill, not several

A separate skill per lane means the user has to know, and name, which one they want before they've finished forming the thought. That's backwards — the opening line already contains the signal ("let's work on a question" vs. "I just read this book" vs. "I've been thinking about..."). Routing on that signal, silently, means the user just starts talking.

The lanes share a posture (stay with uncertainty, one move per response, no forced resolution) and a close (auto-trace, then a translation offer). What differs is only the session-start lookup and the loose arc each lane follows. That's a small enough difference to hold as one skill with a routing table, not five skills with duplicated posture instructions that will drift out of sync with each other over time.

---

## Ideas vs. reads — an asymmetric evidence bar

Everything the agent says falls into one of two categories, and the spec deliberately holds them to different standards:

- **Ideas** are the agent's own offered reads — takes, reframes, complications. They need no source. They can be wrong, sharp, or unwelcome, and the cost of a bad one is trivial: the user discards it in a word.
- **Reads of the user** — claims about their motives, history, or relationships — need a source: something said in this session, or a quoted line in a document. Without one, it's not a statement, it's a question.

The asymmetry exists because the failure costs are asymmetric. A bad idea is cheap to discard. A false claim about someone's own life is expensive — they have to stop, correct it, and then distrust everything built on it since. Models default to stating inferences about a person in the same confident register as sourced fact; this rule forces the distinction to stay visible in the agent's own output, not just in its internal reasoning.

---

## Why "no move" has to be a valid turn

The single most common failure in a thinking-partner skill is reaching for something to say when there's nothing worth saying. A model trained toward helpfulness will manufacture a question, an observation, or worse — a reframe that sounds sharp but is manufactured (see "profundity as filler" below) — rather than sit still.

Making "no move" an explicitly valid response removes the pressure that produces this. "You already have this," "I don't know enough to say," and "say more" are complete turns. The instruction has to be explicit because it goes against the model's default grain.

---

## The "it's not that A, it's that B" tell

This construction is named specifically because it is the shape manufactured insight takes. It sounds sharp regardless of whether it's true, which is exactly what makes it dangerous — it can be generated endlessly without any underlying observation forcing it to stop. The test the skill applies — "what would falsify this?" — catches constructions that have insight's shape but no actual claim inside them. If nothing would falsify it, it isn't an idea, it's a rhetorical move wearing one.

---

## Why the translation offer never auto-writes

A thinking session produces more raw material than belongs in any one artifact — a sharpened question, a claim worth developing, a scene worth keeping as story material. The instinct to auto-capture all of it is wrong for the same reason silent auto-tagging is wrong anywhere else: it turns every conversation into an unintentional entry into a production pipeline (a fleeting note that feeds a drafting queue, for instance, when the user only meant to talk).

The fix is to separate the *housekeeping* writes (session log, daily breadcrumb — internal working memory, always written) from the *content* writes (open question, fleeting note, anecdote — offered, never assumed). The user's pick is what makes the difference between "the session happened" and "the session produced something," and that distinction should be a decision, not a default.

---

## Design tradeoffs

### Silent lane detection vs. asking upfront
Asking "which lane do you want?" would be more reliable but breaks the thing the skill exists to hold — an open conversational entry with no ceremony. A wrong initial guess self-corrects within a turn or two as the actual content of the conversation reveals itself; that's a lower cost than forcing every session to start with a menu.

### One-shot background search vs. no search at all
A single silent query at session start (open-question and idea lanes) can surface genuinely relevant adjacent material without turning the session into a research task. Running it mid-conversation, or reciting it back as a list, would make the tool visible and turn the agent into a search interface rather than a thinking partner. The rule — once, silently, held as background — is a deliberate compromise between "no context at all" and "constant research reflex."

### Faithful carry-forward of the open-question arc
The four-beat arc (pull → competing interpretations → real tension → better question) predates this skill and is kept unchanged, because it's the most load-bearing part of the deepest lane and there was no reason found to revise it during the merge.

---

## WAT mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — lane detection, base posture, per-lane arcs, auto-trace, translation offer |
| **Agent** | Any LLM capable of holding a long conversational context without drifting toward summarization |
| **Tools** | File read/write for notes and logs; optionally a semantic/concept search tool for the background-priming step |
