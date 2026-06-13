# Deep research: walling proprietary data off from non-approved AI agents

## Expert lens
Adopt the perspective of a data governance and AI security architect who advises
organisations on safely adopting third-party agentic AI tools — with specific expertise
in data loss prevention, vendor data-handling terms (data processing agreements,
zero-retention), and keeping proprietary analytics data inside an approved-model
boundary.

## Research question
What are the viable architectures for guaranteeing that no proprietary analytics data —
including aggregate metrics and findings, not only raw rows — ever reaches a non-approved
third-party AI model, while preserving a working multi-agent pipeline in which an
approved-model agent extracts and processes the data and a non-approved agent is still
used for planning and stakeholder-facing delivery?

The answer must resolve the central tension: the stakeholder-delivery step currently
runs on a non-approved model and is precisely where the proprietary numbers are handled.
So each architecture must state explicitly what role, if any, the non-approved agent
retains once it can no longer see any of the proprietary figures.

## Scope constraints
- **Operator:** a single practitioner, not a security team. Solutions must be runnable
  and maintainable by one person; exclude anything assuming a dedicated security
  operations team, IT department, or enterprise data-loss-prevention budget.
- **Current setup (to be preserved, not replaced wholesale):**
  - An approved-model agent — runs models your organisation has approved; in-bounds;
    proprietary data (raw and aggregate) is permitted here. This is the data extraction
    and processing layer.
  - A non-approved agentic tool — currently used for planning and stakeholder-facing
    delivery. Out-of-bounds for any proprietary data.
  - Data resides across your local environment and a remote environment; a shared
    knowledge workspace is synced across both and is read and written by every agent —
    treat the shared workspace as a potential leak surface.
- **Sensitive data definition:** all proprietary numbers, including aggregate metrics,
  findings, and narrative figures — not just raw row-level data. Also includes
  proprietary schema and unpublished business context.
- **Approved-vs-non-approved is a hard governance line, not a preference:** the driver is
  a policy or contractual obligation. Treat "is this vendor/model approved?" as the binary
  that determines whether data may flow to it.
- **Local / self-hosted components** may be considered as supporting mitigations, but the
  research should not assume the operator wants to abandon the current tools.
- **Exclude:** generic "is AI safe" commentary, consumer privacy tips, and anything that
  only works at large-enterprise scale.

## Phases

### Phase 1 — Landscape scan
Core question: how do organisations and individual practitioners keep proprietary data
out of non-approved third-party AI agents today?
- The specific risk profile of agentic tools that can read local files, run shell
  commands, and traverse a shared workspace — how data leaks in these tools in practice
  (context windows, tool outputs, file reads, telemetry).
- The concept of an "approved model / approved vendor" boundary: how organisations define
  it, and what typically moves a vendor across that line.
- Baseline mechanisms: data classification, redaction / tokenization, egress controls,
  sandboxing / permissioning, and contractual controls (zero-retention, data processing
  agreements).

### Phase 2 — Per-path deep analysis
For EACH candidate architecture below, produce a structured analysis: how it works, what
it requires to stand up, ongoing maintenance burden for a solo operator, residual leak
risk, and — critically — what role the non-approved agent retains under it. Add or merge
paths if the evidence suggests better ones.

- **Path A — Redaction / placeholder templating.** The non-approved agent works only on
  de-identified structure (narrative scaffolds, prose with numbers masked or replaced by
  placeholders); the approved tooling reinserts real figures locally. Examine tooling
  (named-entity and number redaction libraries), reliability, and failure modes (numbers
  leaking through paraphrase, ratios reconstructable from context).
- **Path B — Task re-segmentation.** Migrate all data-touching work, including
  stakeholder delivery, onto the approved tooling; restrict the non-approved agent to
  genuinely data-free tasks (system design, templates, planning that names no figures).
  Examine where the clean cut line actually falls and whether "planning" can be truly
  data-free.
- **Path C — Bring the non-approved agent into approved bounds.** What it would take for
  the currently non-approved vendor to become approved: enterprise / zero-data-retention
  agreements, data processing addenda, private or dedicated deployment options,
  certifications (e.g. SOC 2, ISO 27001) and regional data-residency considerations.
- **Path D — Local / self-hosted approved model for delivery.** Run delivery-grade
  generation on a local or self-hosted model that qualifies as approved. Examine
  viability, quality trade-offs, and hardware constraints on a single workstation or
  modest server.
- **Path E — Technical enforcement of the boundary.** Sandboxing, filesystem and
  permission scoping, network egress filtering, and workspace separation so the
  non-approved agent is structurally unable to read proprietary data files — even if
  instructed to. Contrast enforcement ("cannot see") with discipline ("instructed not to
  send").

### Phase 3 — Synthesis
Core question: what does the evidence reveal that a list of options does not?
- Where leaks actually originate in agentic, multi-agent, shared-workspace setups —
  versus where operators assume they do.
- The enforcement-vs-discipline gap: which paths give hard guarantees and which rely on
  the operator never making a mistake.
- The aggregate-vs-raw problem: whether masking raw numbers is enough when aggregates and
  ratios are themselves sensitive and reconstructable.
- The shared knowledge workspace as a cross-agent contamination channel.

### Phase 4 — Implications and recommendations
Deliver, for this specific solo multi-agent setup:
- A comparison matrix across the paths (control strength, solo-operator effort, cost,
  residual risk, impact on current workflow, what the non-approved agent can still do).
- For each viable path, a documented target architecture and the trade-offs it carries —
  not a single winner, but defensible options with their conditions for being the right
  choice.
- Explicit flags where a path only holds under specific assumptions (e.g. an approved
  enterprise agreement being obtainable).

## Source priority
This is a data-governance / AI-security / practical-tooling topic.
- **Primary sources:** official vendor data-handling documentation (enterprise /
  zero-retention / data-processing terms), agentic-tool security and sandboxing /
  permission documentation, and the operator-facing security model of agentic tools.
- **Standards and frameworks (practical level):** SOC 2, ISO 27001, and data-processor
  concepts under major privacy regimes — only as they bear on the approval decision.
- **Analysts / trade:** enterprise AI-adoption and data-loss-prevention guidance (flag
  paywalled sources), reputable security-engineering writing.
- **Practitioner sources:** real implementations of redaction / tokenization, local-model
  serving stacks, and egress filtering.
- **Avoid:** SEO blog summaries, generic "AI privacy" explainers, vendor marketing copy.
- Adapt this list to the domain if needed.

## Quality checks
Before finalising, confirm:
- [ ] Every claim has a named source or specific evidence — no vague "many organisations".
- [ ] Each path states explicitly what the non-approved agent can and cannot do under it.
- [ ] The aggregate-vs-raw distinction is addressed, not collapsed into "redact the data".
- [ ] Enforcement guarantees are distinguished from discipline-based controls.
- [ ] Solo-operator feasibility is assessed for every recommendation — nothing assumes a team.
- [ ] Thin or conflicting evidence (especially on vendor approval routes) is flagged, not smoothed.
- [ ] The output changes how the operator would architect this — it doesn't just restate the risk.
