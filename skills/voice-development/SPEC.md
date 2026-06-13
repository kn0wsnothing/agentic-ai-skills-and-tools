# Voice Development — Design Spec

This document explains the design decisions behind the Voice Development skill.

---

## The Problem

Writing voice is hard to develop in isolation. Most writers can't see their own patterns — they're too close to the work. And most tools offer the wrong kind of help: they improve the draft rather than developing the writer.

AI agents make this worse by default. They generate. The moment a writer leans on generated prose, voice development stops. The agent becomes a ghostwriter, and the human's voice atrophies.

This skill inverts that. The agent is a mirror, not a writer.

---

## Why "Mirror, Not Writer"

The skill's single hard constraint is: never generate prose the user would publish or claim as their own.

This isn't a limitation — it's the design. Voice development requires the writer to do the writing. The agent's value is in observation: finding patterns the writer can't see, quoting back what's working, naming what's flat and why, asking the question that gets at what they actually meant.

Rewriting for the user would short-circuit all of this. If the agent provides a "better" version, the writer accepts it — and learns nothing about their own patterns.

The socratic redirect ("what were you actually trying to say here?") is the tool. Not rewrites.

---

## The Exercise Sequence

The five exercises are sequenced deliberately. Each phase builds on the previous.

### Phase 1: Observe (months 1-2)
Freewriting + archaeology. The goal is raw material and self-knowledge. Freewriting produces unguarded prose. Archaeology analyses existing work to surface patterns the user hasn't named.

### Phase 2: Expand (months 3-4)
Imitation + constraints. The goal is range. Imitation exposes new technical moves. Constraints force the user off default patterns. Both reveal that the voice is not fixed — it's a set of choices.

### Phase 3: Deepen (months 5-6)
Socratic interrogation. The goal is conviction. Voice that sounds like the person requires the person to know what they actually think. The Socratic exercise strips hedging until the belief is visible.

### Phase 4: Codify (month 6+)
Style codification. The goal is a working reference, not a finished product. The style guide is a tool for daily writing — not a document about the writer. Re-run at months 9 and 12 as the voice evolves.

---

## File Structure

The skill reads and writes to a voice development folder. Configure `{voice_dir}` to point to it.

```
{voice_dir}/
  Voice development progress.md    # phase tracker, session log
  Voice profile.md                  # AI-tell patterns, current voice characterisation
  Personal writing style guide.md   # living style reference
  Freewriting/
    YYYY-MM-DD.md                   # per-session: original + annotations + question
  Exercises/
    Voice archaeology/
      YYYY-MM-DD.md
    Imitation/
      YYYY-MM-DD-[writer].md
    Constraints/
      YYYY-MM-DD-[constraint].md
    Socratic/
      YYYY-MM-DD-[topic].md
    Codification/
      YYYY-MM-DD.md
```

---

## The Freewriting Prompt

The freewriting prompt is a thinking prompt, not a content prompt. The distinction matters.

A **content prompt** asks the user to write about a topic: "Write about how your industry is changing." The result is usually polished-ish and generic — the writer draws on prepared thoughts.

A **thinking prompt** surfaces uncertainty or tension: "What's one thing you believed about your field five years ago that you no longer believe?" or "Describe a moment this week when you were more confident than you had any right to be." The result is often unguarded, faster, more alive.

Alive passages are the data. The agent's job is to find them and quote them back.

---

## The Archaeology Method

Voice archaeology is the most diagnostic exercise. A few design choices:

**Multiple contexts.** Voice is what persists across contexts. A single genre sample will show genre conventions as much as personal voice. Asking for a work email, a social post, a personal message, and a formal piece lets the analyst subtract out genre and find what remains.

**Quote before you summarise.** The skill mandates this repeatedly. It's easy for an analyst to say "you favour concrete language." It means almost nothing without quoting the specific passage that demonstrates it. The user needs to see their own patterns in their own words.

**The follow-up sample matters.** The second round of archaeology verifies which patterns are real and which were artifacts of the first sample. Running it once produces a hypothesis. Running it twice produces something more reliable.

---

## The Suggested Writers

The imitation list is opinionated and worth explaining:

- **Paul Graham** — the clearest model for conversational analytical prose. Short sentences, no hedging, concrete before abstract. Good first imitation for most practitioners.
- **Ed Yong** — shows how to be precise without being dry. Sensory detail in analytical writing is a learnable skill.
- **Joan Didion** — teaches by omission. The flat declarative, the withheld explanation. Hard to imitate well; the failure reveals a lot.
- **Venkatesh Rao** — permission to be weird. Dense, digressive, systems-obsessed. Useful for users who have been training themselves to be "accessible" at the cost of depth.
- **Annie Duke** — practitioner voice done right. First-person stakes, comfort with uncertainty, no pretension to academic distance.

The user should pick writers whose work they've actually read and admired. Imitation of an unfamiliar style produces generic pastiche, not useful data.

---

## Cadence and Sustainability

The skill is designed for a 12-month arc, not a 4-week sprint. Voice development is slow. The check-in cadence (once a week, 5-10 minutes) is calibrated to stay alive without becoming a burden.

The skill should never scold for missed sessions. Guilt doesn't develop voice. The check-in question "what got in the way?" is there to understand, not to assess.

The `/voice-status` command exists so the user can return after a gap without having to reconstruct context themselves. Re-entry should be frictionless.

---

## What This Skill Is Not

- It is not a grammar checker or copyeditor
- It is not a content generator
- It is not a writing coach that improves drafts
- It is not a productivity tool for producing more output faster

It is a tool for developing a writer's capacity to produce work that sounds unmistakably like them.
