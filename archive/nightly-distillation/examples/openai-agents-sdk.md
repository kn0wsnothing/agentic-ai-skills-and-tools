# Nightly Distillation — OpenAI Agents SDK

## Setup

Same memory structure as the [Memory Dream Cycle](../../memory-dream-cycle/examples/openai-agents-sdk.md).

### Create a distillation agent

```python
from agents import Agent, Runner
from pathlib import Path

skill_instructions = Path("skills/nightly-distillation/SKILL.md").read_text()

distillation_agent = Agent(
    name="nightly-distillation",
    instructions=skill_instructions,
    model="gpt-4o",
    tools=[read_file, write_file, list_directory, file_exists],
)
```

Use the same file tools as the dream cycle agent.

## Session Discovery

The Agents SDK doesn't have a built-in session history API. You'll need to implement session discovery based on your setup:

**Option A: Log-based discovery.** Write session summaries to a log file during the day, then read the log during distillation.

```python
@function_tool
def get_today_sessions() -> str:
    """Read today's session log for distillation."""
    today = datetime.now().strftime("%Y-%m-%d")
    log_path = f"memory/staging/raw-sessions-{today}.md"
    if os.path.exists(log_path):
        return Path(log_path).read_text()
    return "No sessions logged today."
```

**Option B: Conversation passthrough.** At the end of each agent run, append a summary to a staging file. The distillation agent reads all staging files.

## Running

```python
async def run_distillation():
    result = await Runner.run(
        distillation_agent,
        "Run the nightly distillation now."
    )
    print(result.final_output)
```

Schedule with `schedule`, `APScheduler`, or system cron. See [cron-schedule.md](cron-schedule.md).

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` loaded as agent instructions |
| **Agent** | OpenAI Agents SDK `Agent` instance |
| **Tools** | Custom `function_tool` wrappers for file I/O + session discovery |
