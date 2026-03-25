# Memory Dream Cycle — Factory.ai

How to use the Memory Dream Cycle with [Factory.ai](https://factory.ai).

---

## Setup

### 1. Memory structure

Create the memory directory in your repository:

```bash
mkdir -p memory/{episodic,semantic,procedural}
touch MEMORY.md
```

Commit and push — Factory.ai works with your repository directly.

### 2. Add to your Drafter/Coder configuration

In your Factory.ai project setup, add the dream cycle instructions to your agent's system context or custom instructions:

```
This project uses persistent memory files. When running a memory dream cycle,
follow the instructions in skills/memory-dream-cycle/SKILL.md.
```

### 3. Copy the skill into your repo

```bash
mkdir -p skills/memory-dream-cycle
cp /path/to/SKILL.md skills/memory-dream-cycle/
git add skills/
git commit -m "Add memory dream cycle skill"
git push
```

### 4. Configure paths

Edit the Configuration table in SKILL.md to match your repository layout.

---

## Running

### Via Factory.ai Drafter

Create a task or issue that triggers the dream cycle:

```
Run the memory dream cycle following skills/memory-dream-cycle/SKILL.md.
Read MEMORY.md, run all 5 phases, and commit the changes.
```

Factory.ai's Drafter will create a PR with the memory consolidation changes, which you can review before merging.

### Automated via scheduled issues

Use GitHub Actions or a CI pipeline to create a Factory.ai-compatible issue on a schedule:

```yaml
# .github/workflows/dream-cycle.yml
name: Memory Dream Cycle
on:
  schedule:
    - cron: '0 19 * * *'  # 3am HKT = 7pm UTC
  workflow_dispatch:       # Manual trigger

jobs:
  trigger:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/github-script@v7
        with:
          script: |
            await github.rest.issues.create({
              owner: context.repo.owner,
              repo: context.repo.repo,
              title: 'Memory Dream Cycle — ' + new Date().toISOString().split('T')[0],
              body: 'Run the memory dream cycle following `skills/memory-dream-cycle/SKILL.md`. Read MEMORY.md, run all 5 phases, commit changes to a branch, and open a PR.',
              labels: ['dream-cycle', 'automated']
            });
```

---

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 5-phase consolidation pipeline |
| **Agent** | Factory.ai Drafter/Coder |
| **Tools** | Factory.ai's file access, git operations |

---

## Tips

- **Review the PR.** Factory.ai works via pull requests — you get a review step before memory changes land. This is actually a nice safety property.
- **Commit memory files.** Since Factory.ai works with git, your memory changes have full version history automatically.
- **Model selection.** If Factory.ai lets you choose the underlying model, prefer the strongest reasoning model available for dream cycles.
