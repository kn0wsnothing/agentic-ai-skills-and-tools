# Deep Research Prompt — Prompting Patterns Across Frontier Model Families

## Expert lens
A senior AI engineering lead with hands-on production experience prompting and orchestrating multiple frontier model families (Anthropic, OpenAI, Google, Zhipu, Moonshot, Alibaba), who has read the official prompting guides, system cards, and red-team reports for each, and has built model-routing infrastructure where the same task is dispatched to different models with different prompt scaffolds.

## Research question
What are the operational prompting differences — instruction style, system-prompt structure, reasoning elicitation, tool-use scaffolding, output formatting, context window strategy, refusal behavior, and known failure modes — between Claude 4.x, GPT-5 / o-series, Gemini 2.5–3, GLM-4.6/5, Kimi K2/K2.5, and Qwen3 / Qwen3-Max, such that a single meta-prompt can route and adapt instructions to extract each model's strengths while compensating for its weaknesses?

## Scope constraints
- **Timeframe:** Documentation, papers, and field reports from January 2025 onward. Older material only when it remains the canonical guide (e.g., Anthropic prompt engineering docs that have not been superseded).
- **Models in scope:** Anthropic Claude 4.x family; OpenAI GPT-5 series and o3/o4-class reasoning models; Google Gemini 2.5 Pro, 2.5 Flash, and Gemini 3 if released; Zhipu GLM-4.6 and GLM-5 (English + Chinese); Moonshot Kimi K2 and K2.5; Alibaba Qwen3 series (incl. Qwen3-Max, Qwen3-Coder, Qwen3-Thinking variants).
- **Out of scope:** Image/video generation models, embedding models, fine-tuning, RLHF research, and pre-2025 model versions unless directly contrasted to current behavior.
- **Use cases in scope:** coding/agentic, long-context document analysis, structured/JSON output, tool calling, writing/editing, multi-step reasoning, role/persona adherence.
- **Evidence standard:** Prefer (1) official model-maker documentation and prompting guides, (2) system cards and technical reports, (3) named-author engineering blogs from teams shipping with these models in production, (4) reproducible benchmark or eval reports. De-prioritize anonymous Reddit/Twitter folklore unless corroborated.
- **Geography:** Global, but explicitly include Chinese-language sources for GLM, Kimi, and Qwen (Zhipu blog, Moonshot docs, Alibaba / Tongyi / Qwen team write-ups, Chinese AI dev community) — these models are under-documented in English.

## Phases

### Phase 1 — Per-model deep profile
For each of the six model families, answer:
- **Official prompting guidance.** What does the model maker explicitly recommend? Cite the doc and version.
- **System prompt behavior.** How is the system prompt weighted vs. user turn? Does it support persistent instructions, role headers, XML tags, or specific delimiters?
- **Reasoning elicitation.** Does the model have native reasoning/thinking mode? How is it triggered? Does explicit chain-of-thought help, hurt, or get ignored? For reasoning models, do you instruct *less*?
- **Instruction-following style.** Does it prefer terse imperatives, structured XML, markdown, JSON schema, examples-first, or constitution-style rules? What's the failure mode when you mismatch?
- **Tool use / agentic behavior.** Native tool-calling format, parallel tool calls, retry behavior, hallucinated tool calls, recovery from tool errors.
- **Long context.** Effective context window vs. nominal. Where does attention degrade ("lost in the middle"). Recommended chunking, retrieval, or restating patterns.
- **Output formatting.** JSON-mode reliability, markdown rendering quirks, propensity to add disclaimers/preamble, ability to follow strict format constraints.
- **Refusal & alignment surface.** Topics that trigger refusal, jailbreak resistance, sycophancy tendency, willingness to disagree with user.
- **Known failure modes.** Repetition, drift on long generations, format leakage, language switching (esp. GLM / Kimi / Qwen between English and Chinese), confabulation patterns.
- **Cost / latency profile.** Token economics that change prompt design (e.g., is it cheap enough to send long few-shot examples?).

Deliver Phase 1 as **per-model sections** with the same sub-headings so they're directly comparable.

### Phase 2 — Cross-model comparative matrix
Produce a matrix where rows are the dimensions from Phase 1 and columns are the six model families. Cells should be short, operational ("uses XML tags, explicit `<thinking>`", "JSON mode reliable", "degrades past ~80k tokens"), not prose paragraphs. Flag cells where evidence is thin.

### Phase 3 — Prompting pattern taxonomy
Identify the prompting patterns that **transfer** across all six families vs. those that are **model-specific**. For each pattern, note:
- Pattern name and description
- Which models it helps, which it hurts, which are neutral
- Concrete before/after example

Patterns to investigate explicitly: XML-tag scaffolding, role/persona priming, few-shot examples (and how many), explicit chain-of-thought instructions in reasoning vs. non-reasoning models, output schema specification, negative instructions ("do not..."), self-critique loops, deferred reasoning ("think step by step before answering"), structured output via JSON schema vs. prose-then-parse.

### Phase 4 — Strengths / weaknesses synthesis
For each model family, write a short prose section: **"Use this model when..."** and **"Avoid or compensate when..."**. Tie to verifiable evidence from Phase 1, not vibes. Surface tensions (e.g., a model that's strong at coding but weak at long-context retrieval, or strong at tool use but prone to over-agreement with the user).

### Phase 5 — Implications for a master meta-prompt
Given a meta-prompt that routes to or adapts for whichever model is being used, what:
- **Shared spine** can be model-agnostic? (instruction style, format expectations)
- **Conditional branches** must vary by model? (reasoning trigger, tool-call format, system prompt structure)
- **Routing heuristics** suggest which model to pick for which task type?
- **Compensating instructions** can paper over each model's known weaknesses (e.g., explicit "do not preface with disclaimers" for sycophantic models)?

End with a one-page "operator's cheat sheet" — the most leveraged 5–7 prompting moves per model.

## Source priority

**Tier 1 — primary, must consult:**
- Anthropic prompt engineering docs, Claude 4 release notes, Claude system cards
- OpenAI prompting guide, GPT-5 system card, o-series reasoning model docs, OpenAI cookbook
- Google AI / Gemini prompting guide, Gemini 2.5 / 3 technical reports, Vertex AI documentation
- Zhipu GLM-4.6 / GLM-5 official documentation and model cards (Chinese sources acceptable and expected)
- Moonshot Kimi K2 / K2.5 official docs, Moonshot blog (Chinese sources acceptable)
- Alibaba Qwen team docs, Qwen3 / Qwen3-Max / Qwen3-Coder model cards, Tongyi blog, QwenLM GitHub READMEs and technical reports

**Tier 2 — credible field reports:**
- Engineering blogs from teams shipping with multiple model families in production (e.g., Cursor, Cognition, Sourcegraph, Vercel, Replit, Notion, Linear engineering blogs)
- Named-author posts from prompt engineers and AI infra leads (Simon Willison, Hamel Husain, Eugene Yan, Jason Liu, Maxime Labonne, Anthropic / OpenAI applied AI engineers)
- AI Engineer Summit talks, AIE.world, NeurIPS / ICLR workshops with operational content

**Tier 3 — supporting:**
- Reproducible eval / benchmark reports (LMSYS, Artificial Analysis, LiveBench, SWE-bench Verified, Aider polyglot leaderboard)
- Long-context evals (RULER, NIAH, LongBench, Loong)
- Substantive comparative write-ups in The Information, Stratechery, Latent Space podcast notes

**For Chinese models — also:**
- Zhihu deep-dive posts, WeChat 公众号 from Zhipu / Moonshot / Qwen researchers, Chinese AI developer community (jiqizhixin / 机器之心, Synced, InfoQ China) — translate and quote where useful

**Avoid:**
- SEO listicles ("top 10 prompts for ChatGPT")
- Marketing agency content
- Unsourced LinkedIn opinion posts
- Generic "prompt engineering" courses without model-specific content
- Pre-2025 prompting tips presented as still current without verification

Adapt this list to the domain if needed.

## Quality checks
- [ ] Every model-specific claim cites a named source — official doc, named engineer's post, or specific eval — with a date
- [ ] All six model families have equal depth in Phase 1 (no thin coverage of GLM, Kimi, or Qwen due to English-source bias)
- [ ] Differences between **reasoning** and **non-reasoning** variants within the same family are made explicit (Claude with vs. without extended thinking; GPT-5 vs. o-series; Gemini Flash vs. Pro; Qwen3 base vs. Qwen3-Thinking)
- [ ] Where evidence is thin or model-makers contradict community practice, this is flagged — not smoothed over
- [ ] The comparative matrix in Phase 2 is genuinely operational — a reader can act on a cell, not just nod at it
- [ ] Phase 3 distinguishes patterns that transfer from those that are model-specific, with examples
- [ ] Phase 5 implications are specific enough to write a meta-prompt from — names the conditional branches, not just "adapt as needed"
- [ ] No claim relies on a single anonymous social-media post; corroboration required for folkloric tips
- [ ] Output would change how the user constructs a prompt, not merely restate what each model maker says about itself
