# Overview — the system at a glance

Most "look what I did with AI" demos show a result. This is the system that produces them, kept inspectable.

The shape is three roles and one substrate:

```
        thinking / prose agent        data agent
              (drafting,             (analysis, pulls,
           reviewing, planning)       structured work)
                    \                    /
                     \                  /
                      v                v
                ┌───────────────────────────┐
                │      shared memory         │
                │  (notes + git, versioned)  │
                │  every agent reads & writes │
                └───────────────────────────┘
                            │
                   skills (workflows) run
                   on top of all of it
```

- A **thinking/prose agent** I drive for writing, reviewing, planning, and most day-to-day work.
- A **data agent** that owns analysis and structured data work — kept separate on purpose, partly for focus and partly for governance.
- A **shared memory** both of them read and write: a plain-text notes vault under version control. It's the single source of truth and the thing that makes the agents continuous instead of amnesiac.
- **Skills** — structured workflows (the [`skills/`](../skills/) directory) — run on top: morning and weekly reviews, session wrap-ups, prose scrubs, content pipelines.

None of this depends on a specific product. The thinking agent could be Claude Code, Cursor, or a Gemini-driven setup. The memory could be Obsidian, Logseq, or a folder of markdown. What's fixed is the **roles, the shared-memory contract, and the write policy** — not the software. That's the whole bet: build the system around durable patterns, and swap tools underneath as they change.

What makes it worth showing isn't any single trick. It's that the workflows are concrete and inspectable — you can read exactly what each one does — and that the memory compounds. The system gets more useful the longer it runs, because the agents are working against an accumulating, structured record rather than starting cold each session.

Next: [the principles underneath it](01-principles.md).
