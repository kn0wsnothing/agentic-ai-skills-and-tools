# Agentic AI Skills & Tools

**How I actually work with agentic AI — the real, evolving system, made inspectable.**

Most "here's a cool thing I did with AI" posts show you a result, not a system. This repo is the opposite: it's the working set of skills, prompts, and patterns I use day to day, kept portable so anyone can read them and adapt them — and versioned so you can watch the system change over time.

Nothing here is locked to one platform. Claude Code has `CLAUDE.md`, Cursor has rules, every framework has its own format — but the underlying instructions are universal. If your agent can read and write files, these work, whether you're on Claude Code, Cursor, Factory.ai, the OpenAI Agents SDK, OpenCode, Gemini, or something I've never heard of.

> This is the substrate behind my newsletter and my [/ai page](https://knowsnothing.org/ai). For the narrative — how the pieces fit, what changed, why — start with [the guide](guide/). For the system as it stands today and what changed last, see [CHANGELOG.md](CHANGELOG.md).

---

## The WAT framework

Everything here is organized around the **WAT framework** — Workflows, Agents, and Tools. A three-layer mental model for structuring agentic AI systems:

| Layer | Role | Analogy |
|-------|------|---------|
| **Workflows** | Orchestration logic — what happens, in what order, under what conditions | The director |
| **Agents** | AI reasoning — the role/persona that receives context, makes decisions, takes action | The actors |
| **Tools** | Capabilities the agent invokes — file access, shell, APIs, external services | The props |

Most agentic AI failures come from confusing these layers:
- An agent managing its own orchestration is doing two jobs and doing neither well
- A workflow that hard-codes tool behavior is brittle and hard to extend
- Tools that embed reasoning logic make systems opaque and unpredictable

Keeping the layers separate is what makes the system portable. The workflow doesn't change. The agent and the tools are whatever you've got.

This repo carries two of the three layers as content:
- **`skills/`** — the **Workflows**: structured instructions any agent can follow.
- **`prompts/`** — the **Agents**: role/persona definitions you drop into whatever model you're driving.

The **Tools** layer is your platform's (file read/write, shell, scheduling, MCP servers) — the skills and prompts assume as little about it as possible.

> Deep dive: [What Is the WAT Framework?](https://www.mindstudio.ai/blog/what-is-wat-framework-workflows-agents-tools) by MindStudio

---

## The system

The narrative companion to the catalog lives in [`guide/`](guide/) — how I run this in practice, the principles underneath it, one real workflow walked end to end, and what changed as the system evolved. It's described in **roles, not products**: a thinking/prose agent, a data agent, and a shared memory both read and write. My particular tools are just one instantiation; swap in yours.

---

## Skills (Workflows)

Structured workflows I actually run. Each is portable to any agent with file access.

**Rhythm** — the cadence that keeps the system current

| Skill | What it does |
|-------|--------------|
| [morning-review](skills/morning-review/) | Interactive morning GTD review — calendar, inbox, open-loops sweep, set the day's focus |
| [work-closure](skills/work-closure/) | End-of-workday shutdown — collect loops, sync focus, empty the inbox, triage gate |
| [day-closure](skills/day-closure/) | Short reflective end-of-day close with a forward signal |
| [weekly-review](skills/weekly-review/) | Full GTD weekly review — get clear, get current, get creative |
| [monthly-review](skills/monthly-review/) | Monthly reflective review — themes, goals-vs-actual, altitude check |
| [quarterly-review](skills/quarterly-review/) | Quarterly review — area charters and goals rewritten from three monthlies' gap data |

**Memory**

| Skill | What it does |
|-------|--------------|
| [breadcrumb](skills/breadcrumb/) | Session wrap-up — log the session, breadcrumb the daily note, route durable memory |

**Thinking & writing quality**

| Skill | What it does |
|-------|--------------|
| [salon](skills/salon/) | One conversational thinking session — open questions, books, films, ideas |
| [prose-scrub](skills/prose-scrub/) | Fresh-eyes scrub before publish — AI tells, reader card, clean rewrite |
| [voice-development](skills/voice-development/) | Long-term writing-voice coaching — exercises plus a periodic check-in |
| [process-notes](skills/process-notes/) | Develop fleeting notes into durable notes, one section at a time |
| [newsletter-outline](skills/newsletter-outline/) | Build an outline through framing questions and source notes |
| [newsletter-draft](skills/newsletter-draft/) | Draft a piece in chunks from an approved outline |
| [newsletter-package](skills/newsletter-package/) | Package a finished draft for publishing |

**Governance** — the system looking at itself

| Skill | What it does |
|-------|--------------|
| [operating-audit](skills/operating-audit/) | Honest periodic audit of how you actually work — not a cheerleader |
| [chief-of-staff-brief](skills/chief-of-staff-brief/) | Pre-review sensing brief — nudges, park questions, allocation read |

---

## Prompts (Agents)

Role/persona prompts I actually use — drop them into whatever model you're driving.

**Thinking & PKM**

| Prompt | What it's for |
|--------|---------------|
| [sit-with-the-question](prompts/sit-with-the-question/) | A thinking partner that helps you sit with an open question instead of rushing to answer |
| [work-through-it](prompts/work-through-it/) | A thinking partner that reasons a problem through with you |
| [main-note-thinking-session](prompts/main-note-thinking-session/) | Socratic development of a durable note |
| [pkm-system-architect](prompts/pkm-system-architect/) | Design a human + agent personal-knowledge system from scratch |
| [staying-above-the-algorithm](prompts/staying-above-the-algorithm/) | Career self-discovery — map automation risk, find your human leverage |

**Research**

| Prompt | What it's for |
|--------|---------------|
| [deep-research-creation](prompts/deep-research-creation/) | A meta-prompt that scaffolds a strong deep-research prompt |
| [deep-research-ai-writing-tells](prompts/deep-research-ai-writing-tells/) | Research the patterns that mark writing as AI-generated |
| [deep-research-writing-voice](prompts/deep-research-writing-voice/) | Research finding and developing a distinctive writing voice |
| [deep-research-notebooklm](prompts/deep-research-notebooklm/) | Research getting the most out of NotebookLM |
| [deep-research-prompting-across-models](prompts/deep-research-prompting-across-models/) | Compare prompting across frontier model families |
| [deep-research-vibe-coding](prompts/deep-research-vibe-coding/) | Research AI-assisted prototyping for non-coders |

**Writing & learning**

| Prompt | What it's for |
|--------|---------------|
| [master-prompt-engineer](prompts/master-prompt-engineer/) | A conversational prompt-building assistant |
| [personal-style-guide-builder](prompts/personal-style-guide-builder/) | Build a style guide from your own writing samples |
| [learning-plan-coach](prompts/learning-plan-coach/) | A backward-designed learning plan for any skill |
| [60-day-knowledge-plan](prompts/60-day-knowledge-plan/) | A 60-day plan to learn a topic — books, video, schedule |
| [vibe-coding-coach](prompts/vibe-coding-coach/) | A beginner-to-builder AI coding mentor |
| [end-of-day-wrap](prompts/end-of-day-wrap/) | A minimal end-of-day summary plus tomorrow's priorities |

**Analysis & systems**

| Prompt | What it's for |
|--------|---------------|
| [analysis-decision-architect](prompts/analysis-decision-architect/) | Turn a fuzzy data question into a decision-first analysis |
| [decision-log-template](prompts/decision-log-template/) | A decision log that stands on its own |
| [data-governance-research](prompts/data-governance-research/) | Research keeping proprietary data away from non-approved agents |
| [plan-mode](prompts/plan-mode/) | A hard plan-mode constraint — specify only, never execute |
| [multi-agent-analysis-retrospective](prompts/multi-agent-analysis-retrospective/) | A four-pass retro for a multi-agent analysis run |
| [dataops-to-aiops-synthesis](prompts/dataops-to-aiops-synthesis/) | Synthesize multiple research reports into one brief |

**Personal practice** — adapt with your own context

| Prompt | What it's for |
|--------|---------------|
| [i-ching-reflection](prompts/i-ching-reflection/) | A non-mystical I Ching reflection coach (right-conduct, not fortune-telling) |
| [junzi-ethic-study](prompts/junzi-ethic-study/) | A Confucian *junzi* study-practice prompt |
| [life-coach](prompts/life-coach/) | A CBT / Stoic / Daoist life-coaching prompt — bring your own background |
| [financial-advisor](prompts/financial-advisor/) | A fiduciary financial-planning prompt — bring your own situation |

---

## How to use

The pattern is the same regardless of platform:

1. **Copy the `SKILL.md`** (or `PROMPT.md`) into your agent's context, however your platform loads persistent instructions.
2. **Configure the file paths** — each item assumes a recommended structure you can adapt to yours.
3. **Set up scheduling** if it runs on a schedule (see `examples/cron-schedule.md` where relevant).
4. **Run it** — trigger manually or let the schedule handle it.

Each skill directory includes platform-specific examples showing how to wire it up.

---

## Format

**Skills (Workflows):**
```
skills/
└── skill-name/
    ├── README.md       # Human docs — what it is, prerequisites, install, usage
    ├── SKILL.md        # The portable part — platform-agnostic agent instructions
    ├── SPEC.md         # Design rationale — the "why"
    └── examples/       # Platform-specific adaptation guides (≥2 platforms)
```

**Prompts (Agents):**
```
prompts/
└── prompt-name/
    ├── README.md       # What it's for, how to use it, what to expect
    └── PROMPT.md       # The portable system/role prompt itself
```

`SKILL.md` / `PROMPT.md` are for agents — the portable part. `README.md` is for humans. `SPEC.md` is for the curious.

---

## Prerequisites

These assume your agent has **file read/write access**, **persistent storage** (files survive between sessions — this is how memory works), and, for scheduled skills, **a scheduling mechanism** (cron, Task Scheduler, CI/CD, platform-native). No special APIs, no framework-specific features, no proprietary tools.

---

## Honest about origins

Some of this started life on [OpenClaw](https://github.com/openclaw/openclaw), a personal AI orchestrator. My system has moved on since — the OpenClaw-era skills that I no longer run live in [`archive/`](archive/), labelled as legacy rather than dressed up as current. Everything in `skills/` and `prompts/` is something I actually use now.

**Quality over quantity.** I'd rather publish a handful of things I run in anger, debugged and iterated on, than twenty theoretical ones.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). The short version: platform-agnostic, used in practice (not theorized), WAT-aware, self-contained, well-documented. No framework-specific imports, no proprietary dependencies.

---

## License

MIT — see [LICENSE](LICENSE).
