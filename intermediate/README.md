# Section 2 — INTERMEDIATE

**Question range:** Q11–Q20 (10 questions)  
**Source:** [GenAI-Interview.pdf](../docs/GenAI-Interview.pdf), pages 6–8

## What This Section Covers

Building and evaluating real RAG and agent systems: naive-RAG failure modes, hybrid search, parameter-efficient fine-tuning (LoRA / QLoRA), RAGAS evaluation, the ReAct pattern, LLM-as-judge, OWASP LLM security, MCP, prompt compression, and vLLM serving.

**Key topics:** RAG failure modes, Hybrid search (BM25 + semantic), LoRA / QLoRA, RAGAS evaluation, ReAct agents, LLM-as-judge and its biases, OWASP LLM Top 10, MCP (Model Context Protocol), Prompt compression, vLLM / high-throughput serving.

## Questions

Question wording and numbering are taken directly from the source PDF. Select a question to jump to its answer.

- **[Q11.](#q11)** Walk me through the failure modes of a naive RAG system and how you fix each one.
- **[Q12.](#q12)** What is hybrid search and when is it better than pure semantic search?
- **[Q13.](#q13)** Explain the difference between LoRA and QLoRA. When would you choose one over the other?
- **[Q14.](#q14)** What are the RAGAS metrics and what does each one tell you about your RAG system?
- **[Q15.](#q15)** What is the ReAct agent pattern and how does it work?
- **[Q16.](#q16)** What is LLM-as-judge and what are its known biases?
- **[Q17.](#q17)** What is the OWASP LLM Top 10 and which are most relevant to an engineer building a production RAG system?
- **[Q18.](#q18)** What is MCP (Model Context Protocol) and what problem does it solve?
- **[Q19.](#q19)** What is prompt compression and how does it reduce cost without sacrificing quality?
- **[Q20.](#q20)** How does vLLM enable high-throughput LLM serving?

## Questions and Answers

Answers are transcribed from the source PDF ([pages 6–8](../docs/GenAI-Interview.pdf)). Tables, callouts and code follow the PDF; only the layout is adapted to Markdown.

<a id="q11"></a>

### Q11. Walk me through the failure modes of a naive RAG system and how you fix each one.

| Failure Mode | How to Fix |
|--------------|------------|
| Poor retrieval (irrelevant docs) | Better embeddings, reranker, hybrid search |
| Missing context (relevant docs not retrieved) | Increase recall, query rewrite, hybrid search |
| Bad chunking (too big / small, splits meaning) | Semantic chunking, smaller overlapping chunks |
| Hallucinations / unsupported answers | Stricter prompting, include citations, confidence check |
| High latency | Caching, smaller model, retrieve less |
| High cost | Reduce context, compression, cheaper model |
| Data leakage / wrong source control | Proper authZ, metadata filters, tenant isolation |

[↑ Back to question list](#questions)

<a id="q12"></a>

### Q12. What is hybrid search and when is it better than pure semantic search?

Hybrid search combines keyword search (BM25) + semantic (vector) search and merges the results.

- Better when: Query has exact terms / codes / names / numbers
- Spelling variations
- Acronyms / shorthand
- Mix of lexical + conceptual meaning

> **Query**  
> ↓  
> **Keyword Search (BM25) + Semantic Search (Embeddings)**  
> ↓  
> **Merge & Rank → Top Results**

[↑ Back to question list](#questions)

<a id="q13"></a>

### Q13. Explain the difference between LoRA and QLoRA. When would you choose one over the other?

| LoRA | QLoRA |
|------|-------|
| Low-Rank Adaptation | Quantized LoRA |
| Adds trainable rank-decomposition matrices to attention/FFN layers | Base model loaded in 4-bit with double quantization |
| Base model kept in full precision (e.g. FP16/BF16) | LoRA adapters are trainable (usually FP16) |
| Higher quality, higher memory | Much lower memory, slightly slower training |

> **Choose:** *LoRA when you have enough GPU memory and want max quality. QLoRA when you have limited GPU memory and need cost-efficient training.*

[↑ Back to question list](#questions)

<a id="q14"></a>

### Q14. What are the RAGAS metrics and what does each one tell you about your RAG system?

| Metric | What it tells you |
|--------|-------------------|
| Faithfulness | Are the answers factually consistent with the retrieved context? |
| Answer Relevance | How relevant is the answer to the question? |
| Context Precision | How much of the retrieved context is actually relevant? |
| Context Recall | How much of the relevant information was retrieved? |
| Answer Correctness | Is the final answer correct (vs. ground truth)? |

[↑ Back to question list](#questions)

<a id="q15"></a>

### Q15. What is the ReAct agent pattern and how does it work?

ReAct = Reasoning + Acting. The agent alternates between thinking (reasoning) and taking actions (using tools), observing the results in between.

> **Thought (Reason) → Action (Use Tool) → Observation (Result)**  
> **→ Thought (Reason) → … → Final Answer**

Works as a loop until the agent has enough information to answer.

[↑ Back to question list](#questions)

<a id="q16"></a>

### Q16. What is LLM-as-judge and what are its known biases?

LLM-as-judge uses an LLM to evaluate outputs (e.g. relevance, helpfulness). It's useful but not perfect.

- Known biases:
  - Self-preference bias (favors similar style)
  - Position bias (prefers first/last option)
  - Verbosity bias (prefers longer answers)
  - Lack of real-world grounding

[↑ Back to question list](#questions)

<a id="q17"></a>

### Q17. What is the OWASP LLM Top 10 and which are most relevant to an engineer building a production RAG system?

1. Prompt Injection
2. Sensitive Info Disclosure
3. Supply Chain Vulnerabilities
4. Data & Model Poisoning
5. Improper Output Handling
6. Excessive Agency
7. System Prompt Leakage
8. Vector & Embedding Weaknesses
9. Misinformation
10. Unbounded Consumption

> **Most relevant for RAG engineers:** *#1 Prompt Injection, #2 Sensitive Info Disclosure, #4 Data & Model Poisoning, #8 Vector & Embedding Weaknesses — they directly impact retrieval, content safety, and multi-tenant data integrity.*

[↑ Back to question list](#questions)

<a id="q18"></a>

### Q18. What is MCP (Model Context Protocol) and what problem does it solve?

> **MCP Host (LLM App) ⇄ Standardized protocol for context**  
> **⇄ Data Sources: Tools/APIs, Vector DBs, File Systems**

Problem it solves: different integrations lead to fragmented code. MCP standardizes how tools, contexts, and resources are exposed to models — easier, safer, and reusable.

[↑ Back to question list](#questions)

<a id="q19"></a>

### Q19. What is prompt compression and how does it reduce cost without sacrificing quality?

Prompt compression reduces token count while keeping the essential meaning.

- Summarization of long docs / chat history
- Removing redundant context (keep key facts only)
- Semantic filtering — structured formats (tables, JSON)

> **Result:** *Fewer input tokens reduce cost while preserving the signal.*

[↑ Back to question list](#questions)

<a id="q20"></a>

### Q20. How does vLLM enable high-throughput LLM serving?

- PagedAttention: efficient memory management for long contexts
- Continuous Batching: groups incoming requests dynamically to keep GPUs fully utilized
- Optimized KV Cache: better cache reuse & memory efficiency
- Tensor Parallelism & Multi-GPU: scales across GPUs with minimal overhead
- OpenAI-compatible API: drop-in replacement

> **Requests → vLLM Engine (Continuous Batching) → Higher Throughput / Lower Latency**

> **Key takeaway:** *Build systems that are relevant, reliable, safe, and efficient.*

[↑ Back to question list](#questions)

## Preparation Guidance

- For each RAG failure mode, practice naming the symptom, the likely cause, and the fix.
- Be clear on what each RAGAS metric measures and which part of the pipeline it points to.
- Know the OWASP LLM Top 10 entries and be ready to explain which matter most for a production RAG system, and why.
- Pair every technique with its trade-off (for example LoRA vs. QLoRA on memory and quality).

---

⬅️ [Section 1: Beginner](../beginner/README.md) | [🏠 Main README](../README.md) | [Section 3: Advanced](../advanced/README.md) ➡️
