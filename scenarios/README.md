# Section 4 — SCENARIO-BASED

**Question range:** Q31–Q35 (5 questions)  
**Source:** [GenAI-Interview.pdf](../docs/GenAI-Interview.pdf), pages 13–15

## What This Section Covers

Open-ended production scenarios: diagnosing a drop in user satisfaction, moving an LLM application on-premises, building a Q&A system over a 500-page legal document, debugging an agent that sends duplicate emails, and reducing RAG cost.

**Key topics:** Production debugging, On-premises deployment, Legal-document Q&A design, Idempotency and agent reliability, Cost optimization.

## Questions

Question wording and numbering are taken directly from the source PDF. Select a question to jump to its answer.

- **[Q31.](#q31)** You shipped a RAG feature last week. This week, user satisfaction dropped 20%. The model and index have not changed. What do you investigate?
- **[Q32.](#q32)** An enterprise client wants to deploy your LLM application on-premises because of data privacy requirements. What changes?
- **[Q33.](#q33)** You are given a 500-page legal document and asked to build a Q&A system that lawyers can use. What do you build?
- **[Q34.](#q34)** Your agent is sending duplicate emails to customers. How do you diagnose and fix it?
- **[Q35.](#q35)** How would you reduce the cost of a RAG system that currently spends $8,000/month on LLM API calls?

## Questions and Answers

Answers are transcribed from the source PDF ([pages 13–15](../docs/GenAI-Interview.pdf)). Tables, callouts and code follow the PDF; only the layout is adapted to Markdown.

<a id="q31"></a>

### Q31. You shipped a RAG feature last week. This week, user satisfaction dropped 20%. The model and index have not changed. What do you investigate?

- Query distribution shift (sources / APIs)
- Upstream data changes (processing / pipelines)
- Retrieval quality drop (recall / precision)
- Chunking / prompt / formatting errors
- Latency / timeouts / errors
- UI / UX or answer-presentation issues
- User request inspected (new users / org?)

| How |
|-----|
| Compare analytics before vs. after |
| Log traces, and errors |
| Sample bad queries and compare retrieved context vs. answers |
| Check user feedback / abandon rate |
| Run evals on a held-out set |

> **Goal:** *Find what changed in reality, not in code.*

[↑ Back to question list](#questions)

<a id="q32"></a>

### Q32. An enterprise client wants to deploy your LLM application on-premises because of data privacy requirements. What changes?

| Area | Key Changes |
|------|-------------|
| Model | Local inference: use on-prem LLMs (e.g. Llama, Mistral) via vLLM / cLM |
| Data storage | All embeddings, data, and logs remain on-prem |
| Self-hosted retrieval | Vector DB (Qdrant/Weaviate), reranker, monitoring, observability |
| Networking | Private VPC / VLAN, firewalls, no outbound internet (or allowlist only) |
| Security & compliance | SSO / SAML integration, encryption at rest / in transit, audit logs |
| DevOps | On-prem CI/CD, model updates, backups, disaster recovery |

> **Trade-off:** *Higher infra cost, more responsibility, potentially smaller models.*

[↑ Back to question list](#questions)

<a id="q33"></a>

### Q33. You are given a 500-page legal document and asked to build a Q&A system that lawyers can use. What do you build?

| 1. Ingestion | 2. Chunking | 3. Indexing | 4. Retrieval | 5. Generation | 6. UX for Lawyers |
|--------------|-------------|-------------|--------------|---------------|-------------------|
| Convert PDF → text; OCR if scanned; extract metadata (sections, clauses, page numbers) | Semantic chunking; ~300–800 tokens with overlap; preserve clause structure | Create embeddings; store in vector DB; store metadata (section, page, doc id) | Top-k semantic search; metadata filters (dates, doc id); rerank results | Grounded answer w/ citations; include page/section references; "I don't know" if insufficient context | Clean Q&A interface; citations link to sections/pages; follow-up questions; export / share answers |

> **Nice-to-have:** *Version & updates; highlighted citations; follow-up questions; search within a section.*

> *Great systems are built by diagnosing the right problem and optimizing continuously.*

[↑ Back to question list](#questions)

<a id="q34"></a>

### Q34. Your agent is sending duplicate emails to customers. How do you diagnose and fix it?

**Diagnose:**

- Check logs: tool calls, inputs, outputs, timestamps
- Look for retries / timeouts causing re-execution
- Verify idempotency: does the email tool prevent duplicates?
- Check agent / state management: is the agent remembering it already sent the email?
- Review prompts & instructions for unclear guidance
- Check external systems (webhook retries, queue)

**Fix:**

- Make email tool idempotent (require a unique request id / idempotency key)
- Write confirmations to durable storage (DB) and check before sending
- Add guardrails in prompt: "Do not send the same email twice"
- Use a human-in-the-loop for critical actions
- Add dedupe window (e.g. don't send same email within N hours)
- Add monitoring & alerts for duplicate actions

[↑ Back to question list](#questions)

<a id="q35"></a>

### Q35. How would you reduce the cost of a RAG system that currently spends $8,000/month on LLM API calls?

| 1. Reduce Calls | 2. Improve Retrieval | 3. Optimize Prompts | 4. Model Strategy | 5. Operational |
|-----------------|----------------------|---------------------|-------------------|----------------|
| Add semantic cache for similar queries; batch or dedupe requests where possible | Better chunking & fewer irrelevant tokens; metadata filters to narrow top-k (e.g. 10 → 5) | Shorter prompts (summarize/compress context); remove verbose examples / templates | Use smaller / cheaper models for most tasks; distill or fine-tune model per use case; real-time not required for all flows | Monitor cost per feature / stage; set token budgets; use streaming to cut off failing/long calls early |

> *Track $/query continuously and optimize the most expensive components first.*

> *Great systems are built by diagnosing the right problem and optimizing continuously.*

[↑ Back to question list](#questions)

## Preparation Guidance

- Scenario questions reward a clear method: clarify, form hypotheses, investigate in order, fix, then prevent recurrence.
- State your assumptions out loud before proposing a design.
- Quantify where you can (cost per query, latency budget, satisfaction metrics).
- Close each answer with the trade-offs and how you would monitor the result.

---

⬅️ [Section 3: Advanced](../advanced/README.md) | [🏠 Main README](../README.md) | [Section 5: Coding](../coding/README.md) ➡️
