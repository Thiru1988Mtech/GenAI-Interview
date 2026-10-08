# Section 7 — MODEL SELECTION & INFRASTRUCTURE

**Question range:** Q43–Q58 (16 questions)  
**Source:** [GenAI-Interview.pdf](../docs/GenAI-Interview.pdf), pages 21–24

## What This Section Covers

Choosing and operating models and supporting infrastructure: open-weight vs. frontier models, quantization, latency vs. throughput, speculative decoding, HNSW vs. IVF, prompt compression, PII handling, DSPy, GraphRAG, zero/one/few-shot prompting, A/B testing, distillation, structured outputs, metadata filtering, attention sinks, and prompt versioning.

**Key topics:** Open-weight vs. frontier models, Quantization, Latency vs. throughput, Speculative decoding, HNSW / IVF, Prompt compression, PII, DSPy, GraphRAG / knowledge graphs, Zero-, one- and few-shot prompting, A/B testing, Distillation, Structured outputs, Metadata filtering, Attention sink, Prompt versioning.

## Questions

Question wording and numbering are taken directly from the source PDF. Select a question to jump to its answer.

- **[Q43.](#q43)** When would you choose a 13B open-weight model over a frontier API model like GPT-5?
- **[Q44.](#q44)** What is quantization and how does it affect LLM inference?
- **[Q45.](#q45)** What is the difference between latency and throughput in LLM serving, and how do you optimize each?
- **[Q46.](#q46)** What is speculative decoding and when should you use it?
- **[Q47.](#q47)** Explain the difference between HNSW and IVF indexing in vector databases. When does each perform better?
- **[Q48.](#q48)** What is prompt compression and how does it reduce cost without sacrificing quality?
- **[Q49.](#q49)** How do you handle PII (Personally Identifiable Information) in an LLM application?
- **[Q50.](#q50)** What is DSPy and how does it differ from traditional prompt engineering?
- **[Q51.](#q51)** What is a knowledge graph and when does GraphRAG outperform standard vector RAG?
- **[Q52.](#q52)** What is the difference between zero-shot, one-shot, and few-shot prompting?
- **[Q53.](#q53)** How would you implement A/B testing for LLM prompt changes in production?
- **[Q54.](#q54)** What is model distillation and when is it used?
- **[Q55.](#q55)** Explain the concept of structured outputs and why they matter for production systems.
- **[Q56.](#q56)** What is retrieval with metadata filtering and when is it essential?
- **[Q57.](#q57)** What is the attention sink phenomenon and how does it affect very long context inference?
- **[Q58.](#q58)** How do you version and manage prompts in production?

## Questions and Answers

Answers are transcribed from the source PDF ([pages 21–24](../docs/GenAI-Interview.pdf)). Tables, callouts and code follow the PDF; only the layout is adapted to Markdown.

<a id="q43"></a>

### Q43. When would you choose a 13B open-weight model over a frontier API model like GPT-5?

- Data privacy / compliance (no data leaves your infra)
- Cost at scale (predictable, one-time infra cost vs. usage fees)
- Offline / air-gapped environments
- Customizability (fine-tune, modify, add tools/features)
- Lower latency for local use cases
- Good enough quality for your task (domain-specific)
- You have MLOps capability to run and maintain it

[↑ Back to question list](#questions)

<a id="q44"></a>

### Q44. What is quantization and how does it affect LLM inference?

Quantization reduces the precision of model weights (e.g. FP32 → INT8/INT4) to save memory and speed up inference.

> **FP16 (16-bit) → INT4 (4-bit)**

- Effects:
  - Memory usage ↓ (4–8× less)
  - Throughput / faster inference ↑
  - Cost ↓ (smaller GPUs / more tokens per $)
  - Slight quality drop (INT4 > INT8 > FP16 in loss)

[↑ Back to question list](#questions)

<a id="q45"></a>

### Q45. What is the difference between latency and throughput in LLM serving, and how do you optimize each?

| Latency (time to first token / response time) | Throughput (tokens/sec or requests/sec) |
|-----------------------------------------------|------------------------------------------|
| Affects single-user experience. Optimize by: faster model, shorter context, quantization, KV-cache, speculative decoding, continuous batching, better infra (GPU/TPU) | Affects total capacity. Optimize by: batching, larger context parallelism, tensor / pipeline parallelism, kv-cache reuse, vLLM / TGI, autoscaling |

[↑ Back to question list](#questions)

<a id="q46"></a>

### Q46. What is speculative decoding and when should you use it?

Speculative decoding uses a small draft model to generate multiple tokens ahead, which the large model verifies in parallel.

- Use when:
  - You need lower latency for long outputs
  - You have a good small draft model (e.g. 1–7B)
  - You can trade a bit more compute for big speedups (1.5–3×)

[↑ Back to question list](#questions)

<a id="q47"></a>

### Q47. Explain the difference between HNSW and IVF indexing in vector databases. When does each perform better?

| HNSW (Hierarchical Navigable Small World) | IVF (Inverted File Index) |
|-------------------------------------------|---------------------------|
| Graph-based index; excellent recall; low latency for search; high memory usage; good for dynamic datasets; best for high-recall + low-latency requirements (top-k search) | Cluster-based (k-means + inverted lists); lower memory usage; faster build, better for large scale; recall depends on nprobe; best for very large datasets (millions–billions) where memory is limited and slight latency trade-off is acceptable |

> **Rule of thumb:** *HNSW for quality & speed, IVF for scale & memory efficiency.*

[↑ Back to question list](#questions)

<a id="q48"></a>

### Q48. What is prompt compression and how does it reduce cost without sacrificing quality?

Reduce prompt tokens while keeping the essential information.

- Summarization of long docs / chat history
- Extractive compression (keep key sentences/facts)
- Remove redundant / low-importance tokens
- Use semantic filtering + structured representations (tables, JSON)

> *Fewer input tokens → lower cost + faster latency while preserving the signal the model needs.*

[↑ Back to question list](#questions)

<a id="q49"></a>

### Q49. How do you handle PII (Personally Identifiable Information) in an LLM application?

- Detect: use PII detection (regex + NER models)
- Minimize: don't collect what you don't need
- Mask / Redact: replace PII with tags (e.g. [NAME], [EMAIL])
- Encrypt: at rest and in transit
- Isolate: private VPC / on-prem / dedicated deployment
- Audit: log access, retain policies, user consent
- Delete: data-retention policies + right to be forgotten

[↑ Back to question list](#questions)

<a id="q50"></a>

### Q50. What is DSPy and how does it differ from traditional prompt engineering?

DSPy (Declarative Self-improving Prompting): a framework to program LM behaviors as modules with signatures (inputs/outputs) and automatically optimize prompts using feedback/data.

| Traditional Prompt Engineering | DSPy |
|--------------------------------|------|
| Manual prompts; ad-hoc iteration; hard to scale / compose | Declarative (signatures & modules); auto-optimization (teleprompting); reusable, composable |

[↑ Back to question list](#questions)

<a id="q51"></a>

### Q51. What is a knowledge graph and when does GraphRAG outperform standard vector RAG?

Knowledge Graph (KG): entities (nodes) + relationships (edges) with structured, explicit semantics.

- GraphRAG outperforms when:
  - Queries need multi-hop reasoning across relationships
  - Data is highly structured (people, orgs, products, law, medical)
  - Need explanations with provenance via connected facts
  - Handling large schemas / evolving relationships

[↑ Back to question list](#questions)

<a id="q52"></a>

### Q52. What is the difference between zero-shot, one-shot, and few-shot prompting?

| Zero-shot | One-shot | Few-shot |
|-----------|----------|----------|
| No examples. Just instruction + input. Generalization relies on model priors. | One example + input. Sets the pattern. | Several examples + input. Better performance on complex tasks. |

[↑ Back to question list](#questions)

<a id="q53"></a>

### Q53. How would you implement A/B testing for LLM prompt changes in production?

1. Define metric(s): e.g. task success, CSAT, conversion, latency, cost
2. Create variants: Prompt A (control) vs. Prompt B (treatment)
3. Traffic split: randomly assign % of users/requests
4. Log everything: prompts, inputs, outputs, latency, tokens, cost
5. Evaluate with stats: significance tests, guardrail metrics
6. Rollout: gradual (e.g. 10% → 50% → 100%) with kill switch
7. Monitor: continually watch dashboard + user feedback

[↑ Back to question list](#questions)

<a id="q54"></a>

### Q54. What is model distillation and when is it used?

Model distillation trains a smaller "student" model to mimic the outputs of a larger "teacher" model.

> **Teacher (Model) —knowledge transfer→ Student (Model, smaller)**

- Use when:
  - You need lower latency / cost
  - Deploy on edge or smaller GPUs
  - Protect IP (replace proprietary model with a smaller one)
  - Maintain quality with good compression

[↑ Back to question list](#questions)

<a id="q55"></a>

### Q55. Explain the concept of structured outputs and why they matter for production systems.

Structured outputs ensure the model returns data in a predefined format (JSON, schema, function call, XML).

- Why it matters:
  - Reliable parsing & automation
  - Less post-processing & fewer errors
  - Enforces contracts for downstream systems
  - Improves safety and consistency

[↑ Back to question list](#questions)

<a id="q56"></a>

### Q56. What is retrieval with metadata filtering and when is it essential?

Use metadata (e.g. tenant_id, doc_type, date, language) to filter candidates before/while retrieving.

- Essential when:
  - Multi-tenant isolation (user/org boundaries)
  - Legal / compliance constraints (region, sensitivity)
  - Time-aware info (latest docs only)
  - Domain-specific subsets (product, department)

[↑ Back to question list](#questions)

<a id="q57"></a>

### Q57. What is the attention sink phenomenon and how does it affect very long context inference?

Attention sink: the model over-attends to early tokens (or special tokens) and under-weights the middle of long contexts.

> **Impact:** *Important info in the middle gets ignored → worse answers.*

- Mitigation:
  - Put critical info at the beginning or end
  - Use chunking + retrieval
  - Use models with better long-context handling
  - Use attention-sink tokens (e.g. "sink" tokens / repeat summary)

[↑ Back to question list](#questions)

<a id="q58"></a>

### Q58. How do you version and manage prompts in production?

- Store prompts in a versioned repository (Git or Prompt DB)
- Use semantic versioning (e.g. v1.2.0)
- Track metadata: description, owner, created_at, model, variables
- Use templates with variables & defaults
- A/B test and promote: dev → staging → prod
- Rollback capability
- Monitoring: track performance per prompt version

> **Dev (v1.3.0) → Staging (v1.3.0) → Prod (v1.2.0)**

[↑ Back to question list](#questions)

## Preparation Guidance

- Most questions here are trade-off questions; practice answering with "it depends on X, so I would choose Y when Z".
- Know the headline numbers and rules of thumb quoted in the PDF, and be ready to explain where they come from.
- Be able to connect infrastructure choices (index type, quantization, batching) to user-facing outcomes (latency, cost, quality).
- Expect follow-ups on operational concerns such as rollout, monitoring, and rollback.

---

⬅️ [Section 6: Behavioral](../behavioral/README.md) | [🏠 Main README](../README.md) | [Section 8: Additional Intermediate](../additional-intermediate/README.md) ➡️
