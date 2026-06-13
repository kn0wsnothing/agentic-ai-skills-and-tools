# Nightly Distillation — OpenClaw

This is where the skill was built. OpenClaw's multi-session architecture makes it the ideal platform for distillation — the agent can discover and review all of today's sessions across channels.

## Setup

OpenClaw's workspace already supports the memory layout:

```
~/.openclaw/workspace/
├── MEMORY.md
└── memory/
    ├── episodic/
    ├── semantic/
    ├── procedural/
    └── staging/
```

### Create the cron job

The distillation is a cron prompt — no scripts. Use the SKILL.md content as the payload:

```bash
openclaw cron add \
  --name "nightly-distillation" \
  --schedule "0 21 * * *" \
  --tz "Your/Timezone" \
  --session-target isolated \
  --model "anthropic/claude-sonnet-4-6" \
  --timeout 600 \
  --message "$(cat skills/nightly-distillation/SKILL.md)"
```

### Session discovery on OpenClaw

OpenClaw has a native session list API. Add this to the beginning of the prompt:

```
For session discovery, use sessions_list(activeMinutes=1440, messageLimit=3) to find
today's sessions. Filter to sessions starting with `agent:main:` — skip private and
cron sessions. Pull history from each substantive session with sessions_history(limit=30).
```

### Configure delivery

```
delivery:
  mode: announce
  channel: discord
  to: YOUR_CHANNEL_ID
```

## Running

### Manual

In any OpenClaw session:

```
Run the nightly distillation. Follow skills/nightly-distillation/SKILL.md.
```

### Via cron

Runs automatically. Check status:

```bash
openclaw cron list
```

## How It Works in Production

Our setup:

- **Schedule:** Daily at 9:00 PM HKT
- **Model:** Claude Sonnet (fast enough for capture; Opus reserved for dream cycle reasoning)
- **Timeout:** 600 seconds (can be slow with many sessions)
- **Delivery:** Summary posted to #general on Discord
- **Session discovery:** Uses `sessions_list` to find all `agent:main:` sessions from the last 24 hours
- **Vault integration:** Promotes people facts and project decisions to Obsidian vault notes (platform-specific — not in the generic SKILL.md)

The distillation runs at 9pm. The dream cycle runs at 3am. By morning, memory is both complete and clean.

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | SKILL.md content → cron job prompt |
| **Agent** | Isolated Sonnet session (spawned by cron) |
| **Tools** | OpenClaw's session list/history API, file read/write, exec |
