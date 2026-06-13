# Prose Scrub — Generic agent

How to wire up the prose-scrub skill on any agent that can spawn a subagent and run a shell command or script. Adapt tool names and invocation patterns to your platform.

---

## Minimum requirements

Your agent needs:

- **File read/write** — to read the draft and write the approved rewrite
- **Shell or script execution** — to run the mechanical tell-scan
- **Subagent spawning** — the editor pass requires a cold Opus-tier subagent (no access to the drafting conversation)
- **Project context access** — to assemble the reader card (stakeholder notes, meeting notes, project state)

Optional but useful:
- **Multi-file read** — speeds up reader card assembly from project files

---

## What to build

### 1. A tell-scan

The skill needs a deterministic, file-level scan that emits `file:line: [HIGH|flag] category: "match"` and exits non-zero if any HIGH-severity hit is present.

Options, in order of investment:
- **Grep list** — a shell one-liner or small script using `grep -nEi` against known patterns
- **Language script** — Python/Node/Ruby script with regex patterns, more control over severity tiers
- **Full linter** — a custom linter with your organization's vocabulary canon baked in

Whatever you build, the interface is: `{tell_scan} <file>`. The skill calls it, captures stdout, and feeds it to the editor subagent.

Start minimal. Add patterns each time a tell ships that the scan missed.

### 2. A vocabulary canon

A flat file listing the accepted metric and domain terms your organization uses — the terms the editor must preserve, never relabel. One term or phrase per line, or a simple markdown list.

Example:
```
# Accepted domain vocabulary — preserve these exactly
monthly active readers
engaged reading time
conversion rate
new subscriber rate
```

Pass the path as `{domain_vocab}`. The editor prompt includes an explicit instruction to leave these terms untouched.

### 3. A style guide (optional but recommended)

A markdown file with your organization's voice rules. Pass as `{style_guide}`. Used for register calibration in the editor prompt.

---

## Loading the skill

The prose-scrub skill is natural-language instructions in `SKILL.md`. Load it by:

1. **Pasting into your system prompt** — stateless and self-contained; this works
2. **Referencing by path** — if your agent loads from a skills directory automatically
3. **Triggering by name** — if your platform supports named skills or slash commands

Replace the placeholder parameters (`{tell_scan}`, `{style_guide}`, `{domain_vocab}`, `{workspace}`) with real paths, either in your SKILL.md copy or in your system prompt:

```
When running the prose scrub, use these paths:
- {tell_scan} = scripts/prose_tells_scan.sh
- {style_guide} = docs/voice-guide.md
- {domain_vocab} = docs/canonical-vocabulary.md
- {workspace} = /path/to/your/workspace
```

---

## The cold subagent requirement

The editor pass (Step 2) must run in a subagent that has not seen the drafting conversation. This is the mechanism that makes the scrub work — not a best practice, a non-negotiable.

How to implement cold isolation depends on your platform:

| Platform | Cold isolation approach |
|----------|------------------------|
| Claude Code | `Agent` tool with `subagent_type: general-purpose` and `model: opus`; do not pass conversation history |
| OpenAI Agents SDK | Spawn a new agent with a fresh `messages` list; pass only draft + reader card + rules |
| Custom orchestrator | New session/thread with no prior messages; system prompt = editor prompt only |
| Single-agent (no spawn) | Not recommended; if unavoidable, clear the working context window and reload only the editor prompt + draft |

The subagent receives: draft text, reader card, genre classification, which killers are ON, scan output. Nothing else.

---

## Triggering

Standard trigger phrases:

- `/prose-scrub <file>`
- `scrub this before it goes out`
- `run the prose scrub on [doc]`

Or trigger automatically at the end of any analysis workflow that produces stakeholder-facing output.

---

## The gate

By default, the skill presents findings + rewrite and waits for user approval before writing. This is intentional — the rewrite touches the canonical document.

**If you want a no-gate flow** (e.g., in a fully automated pipeline), restrict the output to findings only and log them. Have a human review before any write happens downstream. Unattended rewrites on stakeholder-facing docs are a higher-risk pattern.

---

## Adapting the editor prompt

The prompt templates in SKILL.md use placeholders for reader card, target format, domain vocab, and scan output. Your orchestration layer is responsible for filling these in before spawning the subagent.

Minimum viable prompt (relational message light scrub, if you're starting simple):

```
You are a careful copy editor. You did not write this message.
Hunt ONLY these tells and fix them: "X, not Y" negation cadence,
vague-observer attribution ("it reads as", "appears to"), journalese
metaphor, importance-claims with no specific.

Reader: {paste reader card}

Scan output: {paste Step 1 output}

Return: (1) line-referenced findings list, (2) rewrite with tells fixed and
everything else intact.
```

Expand to the full template once you're running the full scrub on analysis deliverables.

---

## WAT mapping

| Layer | Implementation |
|-------|---------------|
| **Workflow** | `SKILL.md` — genre gate, 4-step scrub pipeline |
| **Agent** | Any agent with file I/O, shell access, and subagent spawning (Opus for editor subagent) |
| **Tools** | File read/write, script execution for tell-scan, subagent API |
