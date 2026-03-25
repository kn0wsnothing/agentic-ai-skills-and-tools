# Nightly Distillation — Factory.ai

## Setup

Same memory structure as the [Memory Dream Cycle](../../memory-dream-cycle/examples/factory-ai.md). Memory files must be committed to your repo.

## Session Discovery

Factory.ai works via issues and PRs. The distillation's "sessions" are today's completed issues, merged PRs, and Drafter conversations. The skill reviews these to extract what happened.

Add to the distillation prompt:

```
For session discovery, review:
- Issues closed today
- PRs merged today
- Any Drafter/Coder conversations from today
Extract substantive events from each.
```

## Running

### Via scheduled issue

```yaml
# .github/workflows/distillation.yml
name: Nightly Distillation
on:
  schedule:
    - cron: '0 13 * * *'  # 9pm HKT = 1pm UTC (adjust to your timezone)
  workflow_dispatch:

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
              title: 'Nightly Distillation — ' + new Date().toISOString().split('T')[0],
              body: 'Run the nightly distillation following `skills/nightly-distillation/SKILL.md`. Review today\'s sessions, capture to episodic memory, promote knowledge, and commit changes.',
              labels: ['distillation', 'automated']
            });
```

## WAT Mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — the 6-phase capture pipeline |
| **Agent** | Factory.ai Drafter/Coder |
| **Tools** | Factory.ai's file access, git operations, issue/PR history |
