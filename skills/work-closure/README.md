# Work Closure

An interactive end-of-workday GTD shutdown for AI agents. Collects open loops from the day, clears the inbox, routes every item to its right home, and closes the workday cleanly.

---

## How It Works

Work closure runs in three phases:

1. **Phase A — Collection** — batch-scan all open-loop sources in parallel: today's focus items, daily notes, inbox files, meeting notes, and an explicit open-loops sweep for anything still in your head
2. **Phase B — Clarify** — propose one disposition per item against your active project landscape; confirm the whole batch before any write executes
3. **Phase C — Close** — check for overwhelm signals, write a workday summary, and optionally chain into a day-close reflection

**The inbox is guaranteed empty at the end.** This is a hard constraint, not a default. If any item remains unrouted, the skill has not finished.

---

## Prerequisites

Your agent needs:

- **File read/write access** to your workspace
- **A task/project structure** with outcome-stated project notes (see Configuration)
- **An inbox** — a single canonical file plus an inbox folder
- **Today's daily note** — for focus items, notes, and the workday-close summary
- **Optional:** meeting notes folder, a log directory for agent session logs

---

## Workspace Structure

The skill uses path placeholders explained in the Configuration section below. A minimal layout looks like:

```
{workspace}/
├── {daily_notes}/YYYY-MM-DD.md          # Today's focus, notes, work closure
├── {inbox}/Inbox.md                     # Canonical inbox — emptied each run
├── {inbox}/                             # Inbox file drop folder — emptied each run
├── {projects}/Active/{project}/         # One folder per active project
│   └── {project}.md                     # Project note with outcome + task list
├── {meetings}/                          # Meeting notes (optional)
│   └── Processed/YYYY-MM/               # Processed meeting archive
└── {logs}/                              # Agent session logs (optional)
```

---

## Configuration

| Placeholder | What it points to | Example |
|-------------|------------------|---------|
| `{workspace}` | Root of your knowledge base or project directory | `~/notes`, `~/vault` |
| `{daily_notes}` | Daily notes directory | `05 Daily` |
| `{inbox}` | Inbox directory containing `Inbox.md` and file drops | `00 Inbox` |
| `{projects}` | Projects directory | `01 Projects` |
| `{areas}` | Ongoing responsibilities (non-project work) | `02 Areas` |
| `{meetings}` | Meeting notes directory | `06 Meetings` |
| `{logs}` | Agent session log directory | `80 Agent/Logs` |
| `{archive}` | Archive for completed/inactive material | `04 Archive` |

Set these once in your agent's persistent instructions or CLAUDE.md equivalent.

---

## Quick Start

1. **Set up your workspace structure** (see above)
2. **Copy `SKILL.md`** into your agent's skills or instructions directory
3. **Configure the paths** in your agent's persistent instructions
4. **Trigger at the end of your workday:** say "run work closure" or "/work-closure"
5. **The first run will feel thorough** — subsequent runs are faster once your project landscape is stable

---

## Best Practices

**Run at workday end, not mid-day.** The value is in clearing the full day's accumulation. Running earlier means running again later.

**Let the batch gate do its job.** The skill collects everything first, then asks you to confirm dispositions in one batch. Don't try to route items as they surface — the batch model is deliberate.

**Name projects as outcomes.** "Tech desk pilot" is a topic label. "Run the 90-day tech desk pilot to working v1" is a project. The skill enforces this — it's the difference between a next action landing somewhere meaningful vs. a holding folder.

**Name tasks with verbs.** Every actionable item starts with the verb: *Draft, Review, Call, Decide, Confirm.* Topic labels aren't next actions. If the verb isn't nameable, the item isn't clarified.

**The overwhelm check is real.** If your daily note contains language like "overwhelmed", "too much on my plate", "drowning", the skill won't let you close without naming one item and making one move on it. This is a feature.

**Chain into day-close.** At the end, the skill offers to run `day-closure` — the reflection skill that reads the workday summary you just wrote. The marginal cost is small. Take it.

---

## Platform Examples

| Platform | Guide |
|----------|-------|
| Claude Code | [examples/claude-code.md](examples/claude-code.md) |
| Generic / any agent | [examples/generic.md](examples/generic.md) |

---

## FAQ

**How is this different from a task manager or end-of-day review?**
It's interactive and enforced. A task manager shows you a list. This skill drives you through it — collecting open loops you didn't write down, routing each item to a specific destination, and not closing until the inbox is empty. The inbox-empties constraint is the key difference.

**What if I don't have meeting notes?**
Skip that source. The collection phase is modular — if `{meetings}/` doesn't exist or is empty, the skill moves on.

**What does "outcome-stated project" mean?**
A project name that describes the end state: "Bring on a junior analyst" instead of "Hiring". The skill uses these to route tasks correctly — it won't force-fit a task to the nearest keyword match; it checks whether the task actually moves the project's stated outcome forward.

**The batch is huge. Can I triage it?**
Yes. The gate before Phase B explicitly asks "prune, add, or ready to clarify?" — you can drop items before they enter routing.

**What if an item genuinely needs more thought?**
Use the "Unclear" disposition. The skill will offer options: new project with an outcome-stated name, route to an area hub as ongoing work, or trash. Sitting on it in the inbox is not an option.

---

## Design Rationale

See [SPEC.md](SPEC.md) for the full design rationale, including the GTD naming constraints, why the inbox-empties rule is non-negotiable, and how this fits in the broader daily rhythm (morning-review → work → work-closure → day-closure).
