# Memory Dream Cycle — Design Spec

This document explains *why* the skill works the way it does. Read this if you want to modify the dream cycle, build on it, or understand the design tradeoffs.

---

## The Problem

Memory files accumulate entropy over time:

- **Contradictions** — the same fact stated differently across files (a project marked "active" in working memory but completed in the task system)
- **Staleness** — references to completed projects, deleted files, resolved decisions, expired deadlines
- **Bloat** — working memory grows unbounded; every line costs context window budget in every session

Most agent memory systems handle the *additive* side well — capturing what happened today. Almost none handle the *subtractive* side — cleaning up what's accumulated. The dream cycle fills that gap.

---

## The Metaphor

This isn't just branding. The human memory consolidation model is a useful design guide:

| Human Sleep | Agent Memory |
|-------------|-------------|
| Daytime experience → short-term memory | Daily capture/distillation → episodic logs + working memory |
| REM sleep consolidation | Dream cycle — resolve, prune, compress |
| Long-term memory formation | Semantic/procedural files — stable, retrievable |
| Forgetting irrelevant details | Stale pruning — demoting outdated content |

The key insight: **consolidation is not the same as capture.** You need both processes, running at different cadences, with different goals. Capture is fast and additive. Consolidation is slow and subtractive.

---

## Why Tiered Memory

A single memory file doesn't scale. As it grows, every session pays the context cost of loading everything — most of which isn't relevant to the current task.

The three-tier model solves this:

1. **Working memory** (~200 lines) — loaded every session. Only contains what's needed in every context: identity, active projects, current rules, system state. Think of it as L1 cache.

2. **Semantic/procedural memory** (unbounded) — loaded on demand via search. Contains everything else: completed projects, historical decisions, detailed technical notes, people facts. Think of it as disk storage.

3. **Episodic memory** (append-only) — the immutable log. Never modified, never loaded by default. Used for cross-referencing, debugging, and contradiction resolution. Think of it as write-ahead log.

The dream cycle's job is keeping tier 1 lean by demoting content to tier 2 when it's no longer needed in every session.

---

## Why 200 Lines

The target is a balance between two pressures:

- **Too small** → the agent lacks context and makes mistakes or asks redundant questions
- **Too large** → context window waste, slower sessions, higher cost, irrelevant information competing for attention

200 lines is a starting point, not a law. Adjust based on:
- How many active projects you have (more projects = more lines needed)
- Your model's context window size (larger windows tolerate more)
- How much your agent relies on working memory vs search (better search = smaller working memory)

We recommend starting at 200 and adjusting after 2-3 cycles.

---

## Why Demotion Over Deletion

The dream cycle almost never deletes information outright. Instead, it *demotes* — moves content from working memory to semantic files where it's still searchable but doesn't consume context by default.

This is deliberate:

1. **Safety** — deletion is irreversible. Demotion is recoverable. If the dream cycle makes a bad call, the information still exists.
2. **Searchability** — demoted content is still findable via semantic search. A question about a completed project still gets answered; it just doesn't load automatically.
3. **Auditability** — demoted content includes a date stamp, so you can see when and why something was moved.

The only exception: truly obsolete information (temporary debugging notes, one-time fix instructions, information that's been superseded by a correction). These can be deleted because keeping them would be actively misleading.

---

## Why Episodic Immutability

Episodic logs are never modified. This is the strongest constraint in the system, and it's non-negotiable. Here's why:

- **Ground truth** — if working memory says one thing and episodic says another, episodic wins. This only works if episodic is trustworthy.
- **Debugging** — when something goes wrong, episodic logs are the forensic record. Modifying them destroys the evidence.
- **Contradiction resolution** — the dream cycle resolves contradictions by checking "what actually happened." That requires an unmodified record of what actually happened.

---

## Why Flag Rather Than Guess

When the dream cycle encounters a contradiction it can't confidently resolve, it flags it for human review rather than picking a side. This seems conservative, but it's correct:

- **False confidence is worse than uncertainty.** An agent that silently resolves a contradiction wrong will propagate the error into every future session. An agent that flags it creates a one-time interruption.
- **Some contradictions are contextual.** "Is this project still active?" might depend on a conversation the agent wasn't part of. Only the human knows.
- **Trust is earned incrementally.** A new dream cycle should flag aggressively. As you gain confidence in its judgment, you can reduce the flag threshold.

---

## Phase Ordering

The phases run in a specific order for a reason:

1. **Orientation first** — you need the full picture before making any changes
2. **Contradictions before pruning** — resolve conflicts so pruning operates on accurate state
3. **Pruning before consolidation** — remove stale content first so consolidation isn't compressing junk
4. **Consolidation before maintenance** — rewrite working memory so you know what was demoted
5. **Maintenance last** — clean up the files that received demoted content

Reordering the phases will produce worse results. Orientation must be first. Maintenance must be last. The middle three have a logical dependency chain.

---

## Integration Points

The dream cycle is designed to work alongside other memory processes:

| Process | Relationship |
|---------|-------------|
| **Daily capture/distillation** | Additive — captures today's events. Dream cycle runs after, does the subtractive pass. |
| **Compression detection** | Emergency response — snapshots context when nearing limits. Dream cycle is scheduled maintenance, not emergency response. Both can coexist. |
| **Morning briefing** | Consumes working memory. Dream cycle runs overnight so working memory is clean before the briefing. |
| **Semantic search** | Demoted content remains searchable. The dream cycle makes working memory leaner without losing retrievability. |

---

## Known Limitations

- **No cross-agent memory.** The dream cycle operates on one agent's memory files. If you have multiple agents sharing state, you need a coordination layer.
- **Prompt-only — no verification.** The skill relies on the agent following instructions correctly. There's no external validation that the consolidation was done right. Review the output summaries, especially in the first few cycles.
- **Source of truth dependency.** Contradiction resolution quality depends on having a reliable external source of truth. Without one, the cycle can only compare memory tiers against each other.
- **Model quality matters.** This skill requires a capable model (Opus, GPT-4 class, or equivalent). Smaller models may miss contradictions or make poor demotion decisions.
