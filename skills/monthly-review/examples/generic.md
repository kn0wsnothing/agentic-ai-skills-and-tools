# Monthly Review — Generic (Any LLM Agent)

This guide shows how to run the Monthly Review skill on any LLM agent — a custom GPT, a LangChain/LangGraph agent, a LlamaIndex workflow, an Agents SDK runner, or any framework that can load a system prompt and read/write files.

---

## What the Agent Needs

- **File read/write access** — the skill reads daily notes and writes a review document
- **Directory listing** — to enumerate projects, areas, and the daily note window
- **A persistent knowledge base** — daily notes, a goals file, an active projects list
- **A capable model** — the two-pass theme extraction and devil's advocate analysis require real editorial judgment; use the strongest model your budget allows

---

## Minimal File Structure

The skill expects this layout, but every path is configurable:

```
{workspace}/
├── 01 Projects/
│   ├── Active/          # One subdirectory or file per project
│   ├── On hold/
│   └── Someday-Maybe/
├── 02 Areas/            # One subdirectory or file per area
├── 03 Reference/
│   └── Monthly reviews/ # Where review notes are written
├── 05 Daily/
│   └── YYYY-MM-DD.md    # Daily notes — the primary source material
└── (optional) memory/
    └── episodic/        # YYYY-MM-DD.md episodic logs
```

You don't need all of this to start. The minimum viable setup: daily notes + a goals file + an active projects list. Everything else degrades gracefully.

---

## Loading the Skill

### Option A: System prompt injection

Load `SKILL.md` as (or into) the system prompt at the start of the review session.

```python
from pathlib import Path

skill = Path("skills/monthly-review/SKILL.md").read_text()

messages = [
    {"role": "system", "content": skill},
    {"role": "user", "content": "Run the monthly review for last month."}
]
```

### Option B: Tool-called instruction load

If your agent uses a tool-calling architecture, implement a `load_skill` tool that reads the SKILL.md file and returns its contents to the agent as context.

### Option C: Custom GPT / assistant

Paste the full contents of `SKILL.md` into your assistant's system instructions. Update `{workspace}` to a description of your file paths that the assistant can refer to.

---

## File Tools Required

The skill needs these operations. Implement them as tools in your agent framework:

```python
def read_file(path: str) -> str:
    """Read the full contents of a file."""
    ...

def write_file(path: str, content: str) -> None:
    """Write content to a file (overwrite)."""
    ...

def append_to_file(path: str, content: str) -> None:
    """Append content to a file."""
    ...

def list_directory(path: str) -> list[str]:
    """List files/subdirectories in a directory."""
    ...

def file_exists(path: str) -> bool:
    """Check if a file exists."""
    ...

def get_file_mtime(path: str) -> str:
    """Return last modified date (YYYY-MM-DD) for a file."""
    ...
```

---

## Running

### Manual trigger

```python
result = agent.run("Run the monthly review. Follow SKILL.md.")
```

The agent will ask for confirmation at every phase gate. This is by design — the review is interactive. Your agent loop needs to handle multi-turn back-and-forth.

### Multi-turn loop (example)

```python
messages = [{"role": "system", "content": skill}]
messages.append({"role": "user", "content": "Run the monthly review."})

while True:
    response = llm.complete(messages)
    messages.append({"role": "assistant", "content": response})
    
    # Check if the agent is at a gate (waiting for user input)
    if response.strip().endswith("?"):
        user_input = input("> ")
        messages.append({"role": "user", "content": user_input})
    else:
        break  # Review complete
```

### Scheduled start

```python
import schedule

def start_monthly_review():
    messages = [
        {"role": "system", "content": skill},
        {"role": "user", "content": "Start the monthly review. Run pre-flight and Phase A only. Stop at the first gate."}
    ]
    response = agent.run(messages)
    notify_user("Monthly review ready — pick it up when you're ready.", preview=response)

# Run at 8am on the 1st of each month
schedule.every().month.do(start_monthly_review)
```

---

## LangGraph Example

```python
from langgraph.graph import StateGraph, END
from typing import TypedDict, List

class ReviewState(TypedDict):
    messages: List[dict]
    phase: str
    complete: bool

def agent_node(state: ReviewState) -> ReviewState:
    response = llm.invoke(state["messages"])
    state["messages"].append({"role": "assistant", "content": response.content})
    
    # Gate detection — skill always ends gate questions with "?"
    at_gate = response.content.strip().endswith("?")
    state["complete"] = not at_gate and "Monthly review for" in response.content
    return state

def human_node(state: ReviewState) -> ReviewState:
    # Pause for human input at each gate
    user_input = interrupt("Waiting for your response...")
    state["messages"].append({"role": "user", "content": user_input})
    return state

graph = StateGraph(ReviewState)
graph.add_node("agent", agent_node)
graph.add_node("human", human_node)
graph.add_conditional_edges("agent", lambda s: END if s["complete"] else "human")
graph.add_edge("human", "agent")
graph.set_entry_point("agent")

app = graph.compile(checkpointer=memory_saver, interrupt_before=["human"])
```

---

## Path Configuration

Replace `{workspace}` throughout `SKILL.md` with your actual paths. If your structure differs from the defaults, update the relevant phase steps. The skill is designed to degrade gracefully — phases that reference paths you don't have (e.g., episodic memory, a vision file) can be skipped by telling the agent "I don't have this, skip it."

Key paths to configure:

```
Daily notes:        {workspace}/05 Daily/YYYY-MM-DD.md
Goals file:         {workspace}/02 Areas/Horizons/Goals.md
Active projects:    {workspace}/01 Projects/Active/
Review output:      {workspace}/03 Reference/Monthly reviews/
Episodic logs:      {workspace}/memory/episodic/ (optional)
```

---

## WAT Mapping

| Layer | Implementation |
|---|---|
| **Workflow** | `SKILL.md` — the 10-phase interactive review pipeline |
| **Agent** | Your LLM agent (strongest available model recommended) |
| **Tools** | File read/write, directory listing, file mtime — 6 simple operations |

---

## Tips

- **This review is interactive by design.** Every phase ends with a gate question that requires user input. Don't try to run it fully unattended — the value comes from the human-in-the-loop decisions at each phase.
- **Model quality matters here.** The two-pass theme extraction (Pass 1A + 1B) and gap analysis (2A + 2B) involve nuanced editorial judgment. Weaker models will miss self-corrections, over-promote weak signals, and write evaluative prompts despite the explicit rules against it.
- **User-named priors are the correction mechanism.** After the re-read table in Phase B, the agent asks if anything is jumping out. This is where you name the off-record activity the agent can't see — conversations, thinking, reading that wasn't journalled. Feed it here; it corrects the AI's analysis before it runs.
- **The forward signal is the deliverable.** The entire review builds toward a single theme or question for next month. If it comes out vague or goal-shaped, the review didn't work — push back.
