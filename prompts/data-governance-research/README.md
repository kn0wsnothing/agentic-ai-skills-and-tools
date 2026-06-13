# Data governance research: walling proprietary data off from non-approved AI agents

A deep-research brief for an AI model to investigate architectures that keep proprietary analytics data — including aggregate metrics and narrative figures, not just raw rows — inside an approved-agent boundary while preserving a working multi-agent pipeline.

## What this does

Runs a four-phase research programme:
1. **Landscape scan** — how leaks actually happen in agentic, shared-workspace setups; what makes a vendor "approved"
2. **Per-path analysis** — five candidate architectures (redaction, task re-segmentation, vendor approval, local model, technical enforcement), each evaluated on control strength, solo-operator effort, and residual risk
3. **Synthesis** — where the evidence diverges from operator intuition; the enforcement-vs-discipline gap; the aggregate-vs-raw problem
4. **Recommendations** — comparison matrix plus documented target architecture for each viable path

## When to use it

- You run a multi-agent pipeline where some agents are approved to handle proprietary data and others are not
- You need to close a data-governance gap without a security team or enterprise budget
- You want defensible architecture options, not a single "just use X" answer

## How to use it

Paste the prompt into a deep-research capable model (one with web search and enough context to hold multiple long sources). No additional inputs needed — the scope constraints inside the prompt define the problem space.

The prompt is intentionally written for a solo practitioner operating two classes of agent: an approved data layer and a non-approved delivery/planning layer. If your setup differs, adjust the "Current setup" section before running.

## What you get back

A research report with:
- A comparison matrix across the five paths
- For each viable path: how it works, setup cost, ongoing burden, residual risk, what the non-approved agent can still do
- Explicit flags where a path depends on an assumption that may not hold (e.g., an enterprise agreement being obtainable)
- Named sources — the quality check at the end of the prompt enforces no vague claims

## Key design decisions

**Aggregates are in scope, not just raw data.** Most governance frameworks focus on row-level data. This prompt forces the research to address whether aggregate metrics and narrative figures are reconstructable from sanitized outputs — a gap most practitioners underestimate.

**Enforcement vs discipline is a first-class distinction.** "Instructed not to send" and "structurally cannot see" are not the same guarantee. The prompt requires the research to sort paths by which category they fall into.

**Solo-operator feasibility is non-negotiable.** Every recommendation must be assessable by one person. Enterprise-only solutions are explicitly excluded.
