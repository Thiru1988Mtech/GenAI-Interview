# Section 9 — SYSTEM DESIGN SHORT-ANSWER

**Question range:** Q75–Q80 (6 questions)  
**Source:** [GenAI-Interview.pdf](../docs/GenAI-Interview.pdf), pages 28–29

## What This Section Covers

Short-form system design: handling traffic spikes without overwhelming a model provider, stakeholder dashboards, preventing stale RAG answers, pgvector vs. a dedicated vector database, LLM gateways vs. orchestration frameworks, and graceful degradation when the LLM API is unavailable.

**Key topics:** Traffic spikes and backpressure, Product metrics and dashboards, Data freshness, pgvector vs. dedicated vector DBs, LLM gateways vs. orchestration frameworks, Graceful degradation.

## Questions

Question wording and numbering are taken directly from the source PDF. Select a question to jump to its answer.

- **[Q75.](#q75)** How would you design a GenAI system to handle traffic spikes without overwhelming the model provider?
- **[Q76.](#q76)** What metrics would you put on a GenAI product dashboard for a non-technical stakeholder?
- **[Q77.](#q77)** How do you prevent your RAG system from returning stale information?
- **[Q78.](#q78)** When would you use pgvector instead of a dedicated vector database like Qdrant?
- **[Q79.](#q79)** What is the difference between an LLM gateway and an orchestration framework?
- **[Q80.](#q80)** How do you build a GenAI feature that needs to degrade gracefully when the LLM API is unavailable?

## Questions and Answers

Answers are transcribed from the source PDF ([pages 28–29](../docs/GenAI-Interview.pdf)). Tables, callouts and code follow the PDF; only the layout is adapted to Markdown.

<a id="q75"></a>

### Q75. How would you design a GenAI system to handle traffic spikes without overwhelming the model provider?

- Queue & rate limit: use a request queue (e.g. Redis/SQS) and enforce concurrency & rate limits.
- Backpressure: return "queued" status / ETA for non-urgent requests.
- Load shedding: drop or defer low-priority traffic during spikes.
- Caching: semantic cache for answers; prompt + response cache where possible.
- Model tiering: route to smaller/cheaper/faster models when under pressure.
- Autoscale workers: scale out stateless workers that pull from the queue.
- Circuit breaker: open when error rate/latency crosses thresholds; fail fast or downgrade.
- Pre-compute & warm: pre-embed, pre-rank, and keep hot caches warm.
- Multi-provider fallback: failover to secondary provider if primary is throttling/down.

[↑ Back to question list](#questions)

<a id="q76"></a>

### Q76. What metrics would you put on a GenAI product dashboard for a non-technical stakeholder?

- User Impact: active users, queries/conversations, tasks completed.
- Success Rate: % successful answers, task completion rate.
- User Satisfaction: thumbs up/down %, CSAT.
- Time to Value: avg. time to first good answer.
- Cost: total cost, cost per 1K queries.
- Quality (high level): accuracy/helpfulness trend (using evals or feedback).
- Reliability: uptime, error rate.
- Adoption: feature usage, repeat users.
- Top Queries/Topics: what users ask most.

[↑ Back to question list](#questions)

<a id="q77"></a>

### Q77. How do you prevent your RAG system from returning stale information?

- Fresh data pipeline: ingest on change (CDC, webhooks) or frequent scheduled syncs.
- Document versioning: store versions with timestamps and source URLs.
- Recency-aware retrieval: boost recent docs; filter by date when relevant.
- TTLs: set expiration for cached results and embeddings if content is time-sensitive.
- Source citation: always show source + last updated date.
- Validation step: for critical facts, verify against authoritative APIs/databases.
- Monitoring: track data freshness log (source update → index update).

[↑ Back to question list](#questions)

<a id="q78"></a>

### Q78. When would you use pgvector instead of a dedicated vector database like Qdrant?

**Use pgvector when:**

- Your data already lives in Postgres.
- You need strong consistency + ACID transactions with vectors.
- Dataset is small-to-medium (up to ~10M vectors depending on hardware).
- You need simple ops (one database to manage).
- You require SQL joins + filters with vector search.

**Use a dedicated DB (e.g. Qdrant) when:**

- You have very large scale (10M+ to billions of vectors).
- You need advanced ANN performance, sharding, replication.
- You need hybrid dense/sparse, payload filtering at scale, or multi-tenancy features.

[↑ Back to question list](#questions)

<a id="q79"></a>

### Q79. What is the difference between an LLM gateway and an orchestration framework?

| | LLM Gateway | Orchestration Framework |
|---|-------------|-------------------------|
| Purpose | Manage access to LLMs. | Build and manage complex LLM workflows/agents. |
| What it does | Routing, load balancing, rate limiting, caching, logging, cost control, model fallbacks. | Chains, agents, tools, memory, state, retries, conditional logic, human-in-the-loop. |
| Scope | Closer to infrastructure / platform. | Application-level logic and flow. |
| Examples | LiteLLM, Portkey, AI Gateway, Kong AI Gateway | LangGraph, LlamaIndex, CrewAI, Microsoft Autogen |

> **In practice:** *Use a gateway for reliability/cost/control. Use an orchestration framework to build the app.*

[↑ Back to question list](#questions)

<a id="q80"></a>

### Q80. How do you build a GenAI feature that needs to degrade gracefully when the LLM API is unavailable?

- Detect early: health checks and circuit breaker.
- Fallback hierarchy:
  - Use cached answer (semantic/response cache).
  - Use a smaller/cheaper local or alternate model.
  - Use rules/templates/FAQ for simple queries.
  - Show helpful message with option to retry.
- Partial functionality: keep non-LLM features working (search, browse, uploads).
- Queue for later: accept request and process when service is back.
- Communicate clearly: inform users about degraded mode and expected behavior.
- Log & alert: track failures and user impact.

> *Design for scale. Measure what matters. Degrade gracefully.*

[↑ Back to question list](#questions)

## Preparation Guidance

- Open with requirements and constraints, then walk through components in request order.
- Name failure modes explicitly and say how the system behaves under each.
- Mention observability (metrics, logs, alerts) in every design answer.
- Keep answers structured: these are short-answer prompts, so aim for clear, prioritized lists rather than long essays.

---

⬅️ [Section 8: Additional Intermediate](../additional-intermediate/README.md) | [🏠 Main README](../README.md)
