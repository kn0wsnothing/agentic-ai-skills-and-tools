# Prose Scrub — Design spec

Design decisions behind the Prose Scrub skill and why they're made the way they are.

---

## The problem

Analysis deliverables accumulate a predictable set of prose failures. Not random ones — the same six patterns, every time: negation cadence ("X, not Y"), smuggled framing, vague-observer attribution, internal-analyst shorthand, journalese metaphor, importance-claims without the specific. Style guides name them. They ship anyway.

The failure is not insufficient rules. It's insufficient process. The same model that generated the tell is asked to catch it, in the same pass, with no structural separation between author and editor. Generative habits are blind to their own output.

---

## Why a mechanical scan first

The mechanical scan (a script or grep list you maintain) serves one purpose: a deterministic floor that can't be reasoned around. It catches the unambiguous hits — exact phrase matches, known cadence patterns — before the judgment pass begins.

The scan is not the ceiling. A sentence can pass it and still read badly. But a draft that fails the scan does not go to the editor; fixing the mechanical hits first means the editor's attention goes to judgment calls, not obvious misses.

The scan exits non-zero if any HIGH-severity tell is present. The zero-HIGH gate is the non-negotiable floor. The skill is not done until re-scan confirms clean.

---

## Why a cold editor subagent

The generative habit problem is solved by structural isolation, not better prompting. The editor subagent is cold: it receives the draft and the rules, not the drafting conversation. It has no investment in the phrasing it's reviewing.

"Cold to the drafting turns" does not mean "blind to the reader." That's the second failure mode this design was built to prevent.

---

## Why the reader card is load-bearing

A cold editor with no audience model optimises for the mechanical rules and produces over-literal, tone-deaf prose. This failure showed up in practice: a relational message run through the full analysis apparatus came back with collaborative deference stripped ("hedging"), softened asks removed ("smuggled framing"), and the relationship gone. The edit was technically clean. It was also unusable.

The reader card solves this by separating the two kinds of isolation:

- **Isolated from drafting turns** — the editor doesn't know what phrasing was considered and rejected
- **Not isolated from the reader** — the editor knows who receives this and what the message needs to accomplish

The reader card is assembled from the project (stakeholder notes, recent meeting notes, project state) — not from the drafting conversation. That sourcing is intentional.

---

## Why the genre gate exists

The full scrub apparatus — format mandate, bullet restructuring, killer #2 (smuggled framing) — was designed for analysis deliverables. Applied to a relational message, it's destructive:

- BLUF and bullet-ification flatten the conversational register that makes a message land
- Killer #2 flags diplomatic framing as analytical overreach — but in a relational message, that framing is the point
- Word caps strip context that the relationship depends on

The genre gate is a mandatory pre-step. Without it, the scrub is as likely to damage as improve.

**When in doubt, treat as relational.** The asymmetry is intentional. A missed tell in an email is a minor rough edge. A flattened relational message can damage a working relationship.

---

## Why protected framing is called out explicitly

"Smuggled framing" (killer #2) and "diplomatic framing" look identical to a model with no reader model. Both soften claims. Both hedge conclusions. The distinction is:

- Smuggled framing: the analysis asserts a conclusion in context that the body hasn't earned
- Diplomatic framing: the message adjusts its directness to the relationship and what's politically live

The reader card's protected-framing list makes this distinction explicit. The editor is told: these specific moves are NOT tells. Collaborative deference, air-cover language, softened asks — preserve them exactly.

---

## Why the editor must be Opus-tier

Smuggled framing and metaphor-in-context require judgment the mechanical scan can't do. "Shrinking pie" is a tell; "the category is genuinely shrinking" might not be, depending on what came before. The editor needs to read the argument, not just the sentence.

The other reason: this is quality delegation, not token-saving delegation. The normal routing heuristic (delegate to a smaller model to reduce cost) inverts here — the editor pass is the most judgment-intensive step in the skill, and it runs on the best available model.

---

## Why format enforcement and wording restraint are different axes

A recurring failure mode in practice: "don't over-rewrite" was read as "leave the paragraphs alone." The editor preserved clean sentences but left them stacked in walls. The walls shipped.

The spec is explicit: wording restraint applies to wording. A clean sentence inside a fact-stacking paragraph is still a format failure. Format enforcement (breaking paragraphs into bullets, enforcing the two-sentence non-lead limit) is mandatory and not subject to the "don't over-rewrite" constraint.

---

## The domain vocabulary problem

Faithful-label fixes and domain vocabulary canon are in tension. The principle "use plain English labels that match what the metric measures" is correct in the general case. But every domain has accepted shorthand — terms that the audience already uses, that carry specific technical meaning, that would become *vaguer* if swapped for "plainer" alternatives.

The solution: the editor is given an explicit vocabulary canon (user-supplied). Terms in the canon are preserved on sight. The faithful-label swaps apply only to genuine misuse — a metric used to describe something it doesn't measure — never to the accepted terms themselves.

This requires the user to maintain a vocabulary canon file. That's intentional. The editor can't know which terms are canon without being told.

---

## Known limitations

- **The tell-scan is only as good as you make it.** A minimal grep list catches the obvious cadence patterns. A thorough script catches format failures, analyst voice, and density signals. The skill works at either level — but a better scan means less work for the editor.
- **Genre classification can be wrong.** If an analysis deliverable is mislabeled as relational (or vice versa), the scrub runs in the wrong mode. The orchestrator is responsible for getting the classification right; when uncertain, default to relational.
- **The reader card requires project context.** If no project notes, stakeholder maps, or meeting notes exist, the reader card is thin. A thin reader card produces a less calibrated editor pass. Invest in project notes.
- **No interpretation.** The scrub fixes how the prose reads, not what it claims. A factual error in the analysis is a finding for the user, not a silent rewrite.
