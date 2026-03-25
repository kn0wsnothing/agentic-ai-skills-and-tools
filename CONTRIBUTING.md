# Contributing

We welcome contributions — new skills, improvements to existing ones, platform examples, and documentation fixes.

## Quality Bar

Every skill in this repo must meet these criteria:

1. **Platform-agnostic.** The core SKILL.md must work with any agent that has file access. No framework-specific imports, no proprietary tool calls, no assumed runtime.
2. **Tested in production.** You've actually run this skill with a real agent, not just theorized about it. Include a note about your testing in the PR.
3. **WAT-aware.** The skill clearly separates workflow logic (what to do) from agent reasoning (how to decide) and tool invocations (what to call). If you're not sure which layer something belongs to, it probably needs restructuring.
4. **Self-contained.** Everything needed to use the skill is in its directory. No implicit dependencies on other skills or external configs.
5. **Well-documented.** README for humans, SKILL.md for agents, SPEC.md for the curious. All three.

## Skill Format

Follow the standard directory structure:

```
skills/
└── your-skill-name/
    ├── README.md       # Human docs
    ├── SKILL.md        # Agent-readable instructions (the portable part)
    ├── SPEC.md         # Design rationale
    └── examples/       # Platform-specific guides (at least 2 platforms)
```

## Submitting a Skill

1. Fork the repo
2. Create your skill directory under `skills/`
3. Include at least: README.md, SKILL.md, SPEC.md, and 2+ platform examples
4. Open a PR with a description of what the skill does, where you've tested it, and why it's useful

## Improving Existing Skills

Bug fixes, documentation improvements, and new platform examples are all welcome. For significant changes to a skill's core logic (SKILL.md), please open an issue first to discuss the approach.

## Style

- Write clearly. No jargon without explanation.
- Be opinionated. "Here's the best way to do this" is better than "there are many approaches."
- Be concise. Respect the reader's time.
- No badge soup, corporate language, or filler.
