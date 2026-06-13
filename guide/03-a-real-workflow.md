# A real workflow, end to end

Abstract principles are easy to nod along to. Here's one concrete loop, start to finish, so you can see how the roles and the shared memory actually interact. This is an outline of the path; the individual skills carry the detail.

## The loop: an idea becomes a published piece

**1. Capture (accumulation engine).**
A stray thought lands as a fleeting note in the shared memory. No structure, no pressure — just get it down. This is the cheapest, highest-leverage habit in the whole system. ([content capture / process-notes territory](../skills/process-notes/).)

**2. Develop (thinking agent + memory).**
Later, the [`process-notes`](../skills/process-notes/) workflow turns that fleeting note into a durable one through a Socratic, one-section-at-a-time session. The agent asks; I answer; the note gets sharper. The original moves to "processed." Nothing is generated *for* me — the thinking stays mine, the agent just keeps me honest and moving.

**3. Frame before drafting (clarity upstream of volume).**
When a note is ready to become a piece, [`newsletter-outline`](../skills/newsletter-outline/) runs framing questions *first* — what's the actual claim, who's it for, what would make it land — and only then builds an outline from the relevant notes. No drafting until the frame is clear. This is the principle made mechanical.

**4. Draft (thinking agent, in chunks).**
[`newsletter-draft`](../skills/newsletter-draft/) writes the piece section by section against the approved outline, with me in the loop on each chunk. It doesn't run ahead. Each approved section is saved to the shared memory immediately.

**5. Scrub (quality gate).**
Before anything ships, [`prose-scrub`](../skills/prose-scrub/) runs a fresh-eyes pass: a mechanical scan for AI tells, then a cold editor given a "reader card" (who's the audience, what's the relationship). It returns line-referenced findings and a clean rewrite. This is the guardrail that keeps generated prose from reading like generated prose.

**6. Package & ship (production engine).**
[`newsletter-package`](../skills/newsletter-package/) handles the pre-publish checklist — headline, subtitle, tags, image, metadata — and the piece goes out.

**7. Close the loop (memory).**
At the end of the session, [`breadcrumb`](../skills/breadcrumb/) writes a one-line entry to the daily note, a fuller session log, and routes any durable lessons into long-term memory. Next time, the system starts warm.

## What to notice

- **The data agent never touched this loop** — it's a prose loop. A different loop (an analysis, a decision) would route through the [`analysis-decision-architect`](../prompts/analysis-decision-architect/) prompt and the data agent instead. Same memory, different role in the driver's seat.
- **Generation was the small part.** Most of the workflow is framing, judgment gates, and routing outcomes back to memory. The drafting — the bit people think of as "using AI" — is one step in seven.
- **Every step read from or wrote to the same shared memory.** That's what makes it a system and not a sequence of disconnected chats.

Next: [what changed to get here](04-whats-changed.md).
