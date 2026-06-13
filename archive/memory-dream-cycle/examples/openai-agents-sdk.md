# Memory Dream Cycle — OpenAI Agents SDK

How to use the Memory Dream Cycle with the [OpenAI Agents SDK](https://github.com/openai/openai-agents-python).

---

## Setup

### 1. Memory structure

Create the memory directory wherever your agent stores persistent state:

```bash
mkdir -p memory/{episodic,semantic,procedural}
touch MEMORY.md
```

### 2. Create a dream cycle agent

Define a dedicated agent for the dream cycle. The SKILL.md content becomes the agent's instructions:

```python
from agents import Agent, Runner
from pathlib import Path

# Load the skill as agent instructions
skill_instructions = Path("skills/memory-dream-cycle/SKILL.md").read_text()

dream_cycle_agent = Agent(
    name="memory-dream-cycle",
    instructions=skill_instructions,
    model="gpt-4o",  # Use the strongest model available
    tools=[
        # Add your file read/write tools here
        read_file_tool,
        write_file_tool,
        list_directory_tool,
    ],
)
```

### 3. Define file tools

The dream cycle needs file read/write access. Define these as function tools:

```python
from agents import function_tool
import os

@function_tool
def read_file(path: str) -> str:
    """Read the contents of a file."""
    with open(path, 'r') as f:
        return f.read()

@function_tool
def write_file(path: str, content: str) -> str:
    """Write content to a file, creating it if it doesn't exist."""
    os.makedirs(os.path.dirname(path), exist_ok=True)
    with open(path, 'w') as f:
        f.write(content)
    return f"Written {len(content)} bytes to {path}"

@function_tool
def list_directory(path: str) -> str:
    """List files in a directory."""
    entries = os.listdir(path)
    return "\n".join(entries)

@function_tool
def file_exists(path: str) -> bool:
    """Check if a file exists."""
    return os.path.exists(path)
```

### 4. Configure paths

Update the Configuration table in the skill instructions (or prepend configuration to the instructions string):

```python
config_prefix = """
## Configuration (for this deployment)
- WORKING_MEMORY_PATH: ./MEMORY.md
- SEMANTIC_DIR: ./memory/semantic/
- PROCEDURAL_DIR: ./memory/procedural/
- EPISODIC_DIR: ./memory/episodic/
- TARGET_LINES: 200
- SOURCE_OF_TRUTH_PATH: (none)

"""

dream_cycle_agent = Agent(
    name="memory-dream-cycle",
    instructions=config_prefix + skill_instructions,
    model="gpt-4o",
    tools=[read_file, write_file, list_directory, file_exists],
)
```

---

## Running

### Manual trigger

```python
import asyncio

async def run_dream_cycle():
    result = await Runner.run(
        dream_cycle_agent,
        "Run the memory dream cycle now."
    )
    print(result.final_output)

asyncio.run(run_dream_cycle())
```

### Scheduled

Use any Python scheduler. Example with `schedule`:

```python
import schedule
import time
import asyncio

def trigger_dream_cycle():
    asyncio.run(run_dream_cycle())

# Run daily at 3am
schedule.every().day.at("03:00").do(trigger_dream_cycle)

while True:
    schedule.run_pending()
    time.sleep(60)
```

Or use system-level scheduling (see [cron-schedule.md](cron-schedule.md)).

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` loaded as agent instructions |
| **Agent** | OpenAI Agents SDK `Agent` instance (GPT-4o or better) |
| **Tools** | Custom `function_tool` wrappers for file I/O |

---

## Tips

- **Use GPT-4o or better.** The dream cycle requires strong reasoning for contradiction detection. Smaller models will miss things.
- **Keep tools simple.** The dream cycle only needs file read, write, list, and exists. Don't add unnecessary tools.
- **Log the output.** Capture `result.final_output` and store it for auditing. The dream cycle reports what it changed — that report is valuable.
- **Consider a handoff pattern.** If the dream cycle finds issues it can't resolve, it flags them in `## Needs review`. You could use the Agents SDK's handoff feature to route these to a human-review agent or notification system.
