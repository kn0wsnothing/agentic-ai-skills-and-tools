# Contributing

Contributions welcome — new skills, new prompts, improvements to existing ones, platform examples, and documentation fixes.

This repo carries two of the three WAT layers as content: **skills** (Workflows) and **prompts** (Agents). They share a quality bar and differ only in format.

## Quality bar

Every skill *and* prompt must meet these criteria:

1. **Platform-agnostic.** The core `SKILL.md` / `PROMPT.md` must work with any agent that has file access. No framework-specific imports, no proprietary tool calls, no assumed runtime.
2. **Used in practice.** You've actually run it with a real agent, not just theorized about it. Note your testing in the PR.
3. **WAT-aware.** Keep the layers separate — a skill is workflow logic (what to do, in what order); a prompt is agent reasoning (a role/persona). If something embeds tool behavior or mixes the two, it needs restructuring.
4. **Self-contained.** Everything needed is in its directory. No implicit dependencies on other items or external configs.
5. **No private data.** Nothing organization-internal, no real names, internal URLs/IDs, credentials, metrics, or personal context. Strip it or templatize it before it ships.
6. **Well-documented.** README for humans; `SKILL.md`/`PROMPT.md` for agents; `SPEC.md` for the curious (skills).

## Skill format (Workflows)

```
skills/
└── your-skill-name/
    ├── README.md       # Human docs
    ├── SKILL.md        # Agent-readable instructions (the portable part)
    ├── SPEC.md         # Design rationale
    └── examples/       # Platform-specific guides (at least 2 platforms)
```

## Prompt format (Agents)

```
prompts/
└── your-prompt-name/
    ├── README.md       # What it's for, how to use it, what to expect
    └── PROMPT.md       # The portable system/role prompt itself
```

A prompt is lighter than a skill: no orchestration steps, no `SPEC.md` required. Platform examples are optional (most prompts are paste-into-any-chat).

## Submitting

1. Fork the repo
2. Create your directory under `skills/` or `prompts/`
3. Skills: include README.md, SKILL.md, SPEC.md, and 2+ platform examples. Prompts: include README.md and PROMPT.md.
4. Open a PR describing what it does, where you've used it, and why it's useful

## Improving Existing Skills

Bug fixes, documentation improvements, and new platform examples are all welcome. For significant changes to a skill's core logic (SKILL.md), please open an issue first to discuss the approach.

## Style

- Write clearly. No jargon without explanation.
- Be opinionated. "Here's the best way to do this" is better than "there are many approaches."
- Be concise. Respect the reader's time.
- No badge soup, corporate language, or filler.
