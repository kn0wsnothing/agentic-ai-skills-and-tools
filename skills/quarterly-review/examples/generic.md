# Quarterly review — generic (any LLM agent)

This guide shows how to run the Quarterly Review skill on any LLM agent — a custom GPT, a LangChain/LangGraph agent, a LlamaIndex workflow, an Agents SDK runner, or any framework that can load a system prompt and read/write files.

---

## What the agent needs

- **File read/write access** — the skill reads three monthly reviews and writes/rewrites a goals document, area charters, and a quarterly review note
- **Directory listing** — to enumerate areas and locate the right monthly reviews for the quarter
- **A persistent knowledge base** — monthly review notes, a goals document, area hub notes
- **A frontier-class model** — the charter and goals rewrites require real editorial judgment across contested evidence; this is not the skill to run on a cost-optimized model

---

## Minimal file structure

The skill expects this layout, but every path is configurable:

```
{workspace}/
├── 01 Projects/
│   └── Active/                     # standing-commitment project, if you use Phase E
├── 02 Areas/
│   ├── {Area}/{Area}.md            # charter block target, one per area
│   └── Horizons/
│       ├── Goals.md                # rewritten in Phase D
│       └── Vision.md               # touched read-only in Phase F
├── 03 Reference/
│   ├── Monthly reviews/            # primary input — three notes per quarter
│   └── Quarterly reviews/          # this skill's output, kept permanently
└── (optional) memory/
    └── episodic/                   # a distilled close-out entry
```

You don't need all of this to start. The minimum viable setup: three completed monthly reviews + a goals document + area notes. The optional phases (standing-commitment checkpoint, audit-commitments review) degrade gracefully — skip them if you don't have the inputs they need.

---

## Loading the skill

### Option A: System prompt injection

```python
from pathlib import Path

skill = Path("skills/quarterly-review/SKILL.md").read_text()

messages = [
    {"role": "system", "content": skill},
    {"role": "user", "content": "Run the quarterly review for last quarter."}
]
```

### Option B: Tool-called instruction load

If your agent uses a tool-calling architecture, implement a `load_skill` tool that reads `SKILL.md` and returns its contents as context.

### Option C: Custom GPT / assistant

Paste the full contents of `SKILL.md` into your assistant's system instructions. Update `{workspace}` to a description of your file paths the assistant can refer to, and note in your instructions which of the two optional phases (E and G) actually apply to your setup.

---

## File tools required

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
```

---

## Running

### Manual trigger

```python
result = agent.run("Run the quarterly review. Follow SKILL.md.")
```

The agent asks for confirmation at every decision gate — this is by design. Pure phase-transitions ("ready for the next phase?") are announce-and-continue and don't require a turn from the user; decision gates (every rewrite, every disposition, every output) do. Your agent loop needs to handle multi-turn back-and-forth and distinguish the two.

### Multi-turn loop (example)

```python
messages = [{"role": "system", "content": skill}]
messages.append({"role": "user", "content": "Run the quarterly review."})

while True:
    response = llm.complete(messages)
    messages.append({"role": "assistant", "content": response})

    # Decision gates in this skill end with a "?" — phase-transition
    # announcements do not. Only pause for the former.
    if response.strip().endswith("?"):
        user_input = input("> ")
        messages.append({"role": "user", "content": user_input})
    else:
        break  # Announce-and-continue — loop again without user input
```

### Scheduled start

```python
import schedule

def start_quarterly_review():
    messages = [
        {"role": "system", "content": skill},
        {"role": "user", "content": (
            "Start the quarterly review. Verify preconditions (closing "
            "monthly exists; operating audit ran in-window if you use it), "
            "create the review note, then stop at the first real gate."
        )}
    ]
    response = agent.run(messages)
    notify_user("Quarterly review ready — pick it up when you're ready.", preview=response)

# Run at 8am on the 1st of January, April, July, October
schedule.every().quarter.do(start_quarterly_review)  # pseudocode — use a real cron/quarter scheduler
```

---

## Path configuration

Replace `{workspace}` throughout `SKILL.md` with your actual paths.

```
Monthly reviews:     {workspace}/03 Reference/Monthly reviews/
Goals document:      {workspace}/02 Areas/Horizons/Goals.md
Vision document:     {workspace}/02 Areas/Horizons/Vision.md
Area hub notes:      {workspace}/02 Areas/{Area}/{Area}.md
Review output:       {workspace}/03 Reference/Quarterly reviews/
```

---

## WAT mapping

| Layer | Implementation |
|---|---|
| **Workflow** | `SKILL.md` — the 9-phase interactive review pipeline |
| **Agent** | Your LLM agent (frontier-class model required — not a cost-optimized tier) |
| **Tools** | File read/write, directory listing — 5 simple operations |

---

## Tips

- **This is the highest-stakes review in the rhythm — protect the model-quality gate.** Don't let a scheduler or automation silently downgrade the model for this one to save cost.
- **Distinguish decision gates from phase-transition announcements.** Every rewrite, disposition, and output needs the user's explicit input; pure "ready to move on?" transitions don't. Getting this wrong either annoys the user with pointless pauses or — worse — auto-commits a charter or goals-document change without arbitration.
- **Conditional phases need an explicit skip, not a silent guess.** If your agent can't tell whether you track a standing commitment or run an operating audit, ask once at the start of the session rather than assuming.
- **The line-by-line goals arbitration is not optional.** A single "approve the whole document" gate defeats the purpose — each goal's disposition carries different evidence and deserves its own moment of judgment.
