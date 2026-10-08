# Section 3 — ADVANCED

**Question range:** Q21–Q30 (10 questions)  
**Source:** [GenAI-Interview.pdf](../docs/GenAI-Interview.pdf), pages 9–12

## What This Section Covers

Production-grade design problems: multi-hop RAG, lost-in-the-middle, sub-800ms latency design, agent infinite-loop prevention, multi-tenancy, ambiguous queries, sampling parameters, agent evaluation, context engineering, and handling LLM API failures and rate limits.

**Key topics:** Multi-hop RAG / query decomposition, Lost-in-the-middle, Latency optimization, Agent safety (infinite loops), Multi-tenancy, Ambiguous query handling, Sampling (temperature, top-p, top-k), Agent evaluation, Context engineering, API failures and rate limits.

## Questions

Question wording and numbering are taken directly from the source PDF. Select a question to jump to its answer.

- **[Q21.](#q21)** Your production RAG system is working well on simple queries but failing on multi-hop questions. What is the architecture change?
- **[Q22.](#q22)** Describe the lost-in-the-middle problem and how you engineer around it.
- **[Q23.](#q23)** How do you design an LLM application for strict latency requirements (sub-800ms response)?
- **[Q24.](#q24)** Explain how you would prevent an AI agent from entering an infinite loop in production.
- **[Q25.](#q25)** How would you handle multi-tenancy in a RAG system where different users must only access their own documents?
- **[Q26.](#q26)** A user's query gets routed to your RAG system but the query is ambiguous. What do you do?
- **[Q27.](#q27)** What is the difference between temperature, top-p, and top-k sampling? When do you adjust each?
- **[Q28.](#q28)** How do you evaluate an agent's performance beyond just "did it complete the task?"
- **[Q29.](#q29)** What is context engineering and how is it different from prompt engineering?
- **[Q30.](#q30)** How do you handle LLM API failures and rate limits in a production application?

## Questions and Answers

Answers are transcribed from the source PDF ([pages 9–12](../docs/GenAI-Interview.pdf)). Tables, callouts and code follow the PDF; only the layout is adapted to Markdown.

<a id="q21"></a>

### Q21. Your production RAG system is working well on simple queries but failing on multi-hop questions. What is the architecture change?

Use a Multi-Hop / Decomposition + Iterative Retrieval architecture.

> **Query → Query Decomposition (LLM) → Sub-queries (1, 2, …)**  
> **→ Retrieve for each sub-query (parallel) → Synthesis/Reason (LLM) → Answer**

- Break the question into sub-questions
- Retrieve iteratively — each hop uses prior context
- Maintain a working memory / scratchpad
- Optionally build a knowledge graph or use GraphRAG for entity hops

[↑ Back to question list](#questions)

<a id="q22"></a>

### Q22. Describe the lost-in-the-middle problem and how you engineer around it.

Lost-in-the-Middle: in long contexts, LLMs tend to pay less attention to information in the middle and focus more on the beginning and end.

> **Risk:** *Important context placed in the middle gets ignored.*

> **Beginning (High attention) — Middle (Low attention) — End (High attention)**

- Put the most important info at the beginning and end
- Use contextual compression / summarization to shrink the middle
- Re-order chunks by relevance (most → least)
- Keep context-window utilization moderate (≤70–80%)

[↑ Back to question list](#questions)

<a id="q23"></a>

### Q23. How do you design an LLM application for strict latency requirements (sub-800ms response)?

| Technique | How |
|-----------|-----|
| Use faster models | Choose small / distilled models (7B, 8x7B MoE) or optimized APIs (gpt-4o-mini, Claude Haiku, etc.) |
| Reduce tokens | Short prompts, summarize context, compress docs, use few-shot sparingly |
| Optimize retrieval | Tight top-k (3–5), use embedding filters & metadata filters to cut noise |
| Parallelize | Run retrieval, reranking, tool calls in parallel where possible |
| Cache aggressively | Semantic cache for queries / responses |
| Edge / regional inference | Use nearby regions or edge inference to reduce network latency |
| Streaming + early return | Stream tokens back, return partial results quickly |
| Continuously profile | Measure every stage. Optimize the slowest component |

> **Target budget (sub-800ms):** *Retrieval < 150ms, Rerank < 100ms, LLM < 400ms, Overheads < 150ms.*

[↑ Back to question list](#questions)

<a id="q24"></a>

### Q24. Explain how you would prevent an AI agent from entering an infinite loop in production.

| Control | What it does |
|---------|--------------|
| Max steps / iterations | Set a hard limit on the number of reasoning or tool-use steps |
| Time budget | Enforce an overall timeout for the task |
| Loop detection | Track repeated states, actions, or tool calls |
| Action deduplication | Prevent identical actions being executed repeatedly |
| Guardrails / rules | Define allowed actions and stop conditions |
| Final answer requirement | Force the agent to produce a final answer after N steps |
| Human-in-the-loop fallback | Escalate to a human when stuck |

[↑ Back to question list](#questions)

<a id="q25"></a>

### Q25. How would you handle multi-tenancy in a RAG system where different users must only access their own documents?

| Control | How |
|---------|-----|
| Tenant isolation | Store tenant_id with every document and embedding |
| Filtered retrieval | Always filter by tenant_id (metadata filter) during retrieval |
| Separate indexes (option) | Per-tenant index or namespace for strong isolation |
| Row-level security | Enforce RLS at the DB / storage layer for docs & metadata |
| Audit & monitoring | Log access per tenant, monitor anomalies |

> **Golden rule:** *Never rely only on application logic — enforce isolation at the data / index layer.*

[↑ Back to question list](#questions)

<a id="q26"></a>

### Q26. A user's query gets routed to your RAG system but the query is ambiguous. What do you do?

> **Detect Ambiguity → Clarify with User → Proceed with Clarified Query**

- Detect ambiguity: low-confidence retrieval, multiple possible intents, vague / underspecified
- Clarify with user: ask targeted questions or provide options ("Do you mean … or …?", "Which time period are you referring to?")

> **If clarification fails:** *Fall back to a safe default (general answer) or route to human support.*

[↑ Back to question list](#questions)

<a id="q27"></a>

### Q27. What is the difference between temperature, top-p, and top-k sampling? When do you adjust each?

- Temperature (T): controls randomness by scaling logits before sampling.
  - Low (0–0.3): more deterministic, factual answers
  - High (0.8–1.2+): more creative, diverse outputs
- Top-p (nucleus sampling): samples from the smallest set of tokens whose cumulative probability ≥ p.
  - Good balance of creativity & coherence (typical: 0.8–0.95)
- Top-k sampling: samples only from the top-k most probable tokens.
  - Limits the candidate pool (typical: 20–50)

> **Rule of thumb:** *Use low T + top-p for factual QA. Use higher T or top-k for creative writing or brainstorming.*

[↑ Back to question list](#questions)

<a id="q28"></a>

### Q28. How do you evaluate an agent's performance beyond just "did it complete the task?"

| Task Success | Correctness | Efficiency | Robustness | Safety | User Satisfaction |
|--------------|-------------|------------|------------|--------|-------------------|
| Did it achieve the goal? | Is the final answer correct and grounded? | How many steps, tool calls, tokens used? | Handles edge cases, messy inputs? | No harmful, biased, or unsafe outputs? | User feedback, ratings, NPS, thumbs up/down? |

> **Bottom line:** *Use a mix of automatic metrics + human evaluation.*

[↑ Back to question list](#questions)

<a id="q29"></a>

### Q29. What is context engineering and how is it different from prompt engineering?

| Prompt Engineering | Context Engineering |
|--------------------|---------------------|
| Focus on how you instruct the model. Crafting prompts, examples, formats, system messages. Works on the "instructions" layer. | Focus on what context you provide. Selecting, ordering, compressing, and structuring the right information. Works on the "information" layer. |

> **Takeaway:** *Good context > perfect prompt.*

[↑ Back to question list](#questions)

<a id="q30"></a>

### Q30. How do you handle LLM API failures and rate limits in a production application?

| Control | How |
|---------|-----|
| Retry with exponential backoff | Handle 429/5xx with jittered backoff |
| Circuit breaker | Stop calling after repeated failures; fail fast, then recover |
| Fallback models | Use a smaller / cheaper model as fallback |
| Queue & throttle | Rate-limit requests, queue when needed |
| Graceful degradation | Return cached results, partial answers, or helpful error messages |
| Monitoring & alerts | Track errors, latency, rate-limit hits |

- Best practices: idempotent requests
  - Timeouts
  - Request collapsing
  - Caching
  - User-friendly errors

> **Key takeaway:** *Design for scale, design for failure, and optimize for users.*

[↑ Back to question list](#questions)

## Preparation Guidance

- Structure answers as: identify the problem, propose the architecture or controls, then state the trade-offs.
- For latency questions, practice breaking a time budget across pipeline stages.
- For safety and isolation questions, emphasize enforcing controls at the data / system layer rather than relying on prompts alone.
- Be ready to go one level deeper on any bullet you mention; advanced interviews probe follow-ups.

---

⬅️ [Section 2: Intermediate](../intermediate/README.md) | [🏠 Main README](../README.md) | [Section 4: Scenario-Based](../scenarios/README.md) ➡️
