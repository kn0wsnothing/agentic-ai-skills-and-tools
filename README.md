# Agentic AI Skills & Tools

**Agent skills that work everywhere — not just on one platform.**

Most agent skills are locked to a single platform. Claude Code has `CLAUDE.md`. Cursor has rules. Every framework has its own format. The underlying ideas are universal, but the implementations aren't portable.

This repo is different. Every skill here is a structured prompt that any AI agent can read and follow — regardless of whether you're using Claude Code, Cursor, Factory.ai, OpenAI Agents SDK, OpenCode, OpenClaw, or something we've never heard of. If your agent can read files and write files, these skills work.

---

## The WAT Framework

These skills are organized around the **WAT framework** — Workflows, Agents, and Tools. It's a three-layer mental model for structuring agentic AI systems:

| Layer | Role | Analogy |
|-------|------|---------|
| **Workflows** | Orchestration logic — what happens, in what order, under what conditions | The director |
| **Agents** | AI reasoning — receives context, makes decisions, takes actions | The actors |
| **Tools** | Capabilities the agent invokes — file access, shell, APIs, external services | The props |

Most agentic AI failures come from confusing these layers:
- An agent managing its own orchestration is doing two jobs and doing neither well
- A workflow that hard-codes tool behavior is brittle and hard to extend
- Tools that embed reasoning logic make systems opaque and unpredictable

Each skill in this repo is a **Workflow** — a structured set of instructions that any **Agent** (LLM) can follow using whatever **Tools** your platform provides (file read/write, shell access, scheduling, etc.).

Keeping these layers separate is what makes the skills portable. The workflow doesn't change. The agent and tools are whatever you've got.

> Deep dive: [What Is the WAT Framework?](https://www.mindstudio.ai/blog/what-is-wat-framework-workflows-agents-tools) by MindStudio

---

## Available Skills

| Skill | Description | Status |
|-------|-------------|--------|
| [Memory Dream Cycle](skills/memory-dream-cycle/) | Nightly memory consolidation — resolve contradictions, prune stale content, compress working memory | ✅ Ready |
| [Nightly Distillation](skills/nightly-distillation/) | Evening memory capture — review today's sessions, extract events, promote knowledge to the right tier | ✅ Ready |
| Interstitial Journal | Timestamped brain-dump capture to episodic memory and daily notes | 🔜 Coming soon |

---

## How to Use

The general pattern is the same regardless of platform:

1. **Copy the `SKILL.md`** from the skill you want into your agent's context (however your platform loads persistent instructions)
2. **Configure the file paths** — each skill uses a recommended memory structure, but you can adapt it to yours
3. **Set up scheduling** if the skill runs on a schedule (see `examples/cron-schedule.md` in each skill)
4. **Run it** — either trigger manually or let the schedule handle it

Each skill directory includes platform-specific examples showing exactly how to wire it up for Claude Code, Cursor, Factory.ai, OpenAI Agents SDK, OpenCode, and OpenClaw.

---

## Skill Format

Every skill in this repo follows the same structure:

```
skills/
└── skill-name/
    ├── README.md       # Human docs — what it is, prerequisites, install, usage, best practices
    ├── SKILL.md        # The skill itself — platform-agnostic agent instructions
    ├── SPEC.md         # Design rationale — the "why" behind the design decisions
    └── examples/       # Platform-specific adaptation guides
        ├── claude-code.md
        ├── cursor.md
        ├── factory-ai.md
        ├── openai-agents-sdk.md
        ├── opencode.md
        ├── cron-schedule.md
        └── openclaw.md
```

- **README.md** is for humans. Prerequisites, installation, configuration, best practices, FAQ.
- **SKILL.md** is for agents. The actual structured prompt your AI reads and follows. This is the portable part.
- **SPEC.md** is for the curious. Design rationale, architecture decisions, tradeoffs. Read this if you want to understand *why* the skill works the way it does, or if you want to modify it.
- **examples/** are platform-specific. Each file shows how to integrate the skill into a specific agent framework, including scheduling.

---

## Prerequisites

These skills assume your agent has:

- **File read/write access** — the agent can read and write to a local filesystem
- **Persistent storage** — files survive between sessions (this is how memory works)
- **A scheduling mechanism** — for skills that run on a schedule (cron, Task Scheduler, CI/CD, platform-native scheduling)

That's it. No special APIs, no framework-specific features, no proprietary tools.

---

## Philosophy

These skills were built for [OpenClaw](https://github.com/openclaw/openclaw), a personal AI orchestrator, and battle-tested in daily production use. We're publishing them because the patterns are universal — every agent system that maintains persistent memory faces the same problems.

We're honest about the origins. The OpenClaw example in each skill shows the original implementation. The other examples show how to adapt the same concepts to your platform. The core skill (SKILL.md) is platform-agnostic by design.

**We prioritize quality over quantity.** We'd rather have three well-documented, production-tested skills than twenty theoretical ones. Everything here has been run in anger, debugged, and iterated on.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on submitting new skills or improving existing ones.

The short version: skills must be platform-agnostic, tested in production, and follow the WAT framework. No framework-specific imports, no proprietary dependencies.

---

## License

MIT — see [LICENSE](LICENSE).
