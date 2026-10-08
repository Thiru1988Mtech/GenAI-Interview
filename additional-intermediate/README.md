# Section 8 — ADDITIONAL INTERMEDIATE

**Question range:** Q59–Q74 (16 questions)  
**Source:** [GenAI-Interview.pdf](../docs/GenAI-Interview.pdf), pages 25–27

## What This Section Covers

Further intermediate topics: LangChain vs. LangGraph, needle-in-a-haystack testing, RLHF vs. DPO, chain-of-thought, embedding vs. generation models, streaming vs. batch inference, system-prompt security, rerankers, few-shot via retrieval, constitutional AI, agent debugging, conversational RAG, agent frameworks, chain-of-verification, contradictory documents, and agent memory staleness.

**Key topics:** LangChain / LangGraph, Long-context evaluation, RLHF / DPO, Chain-of-thought, Embedding vs. generation models, Streaming vs. batch inference, System-prompt security, Reranking, Few-shot via retrieval, Constitutional AI, Agent debugging, Conversational RAG, Agent frameworks (OpenAI Agents SDK, LangGraph, CrewAI), Chain-of-verification, Contradictory sources, Agent memory staleness.

## Questions

Question wording and numbering are taken directly from the source PDF. Select a question to jump to its answer.

- **[Q59.](#q59)** What is the difference between LangChain and LangGraph? When do you use each?
- **[Q60.](#q60)** What is the "needle in a haystack" test and what does it evaluate?
- **[Q61.](#q61)** What is RLHF and why did DPO largely replace PPO-based RLHF for most post-training?
- **[Q62.](#q62)** What is chain-of-thought prompting and when does it help?
- **[Q63.](#q63)** How does an embedding model differ from a generation model architecturally?
- **[Q64.](#q64)** What is the difference between streaming and batch inference for LLMs?
- **[Q65.](#q65)** What is a system prompt and what are the security implications of putting sensitive logic in it?
- **[Q66.](#q66)** What is a reranker and how does it differ from a retriever in a RAG pipeline?
- **[Q67.](#q67)** What is few-shot prompting via retrieval (also called RAG-based few-shot)?
- **[Q68.](#q68)** What is constitutional AI and how does Anthropic use it to align Claude?
- **[Q69.](#q69)** How do you debug an agent that is producing incorrect final answers despite having the correct information in its tool outputs?
- **[Q70.](#q70)** What is the difference between document Q&A and conversational RAG, and how do they require different architectures?
- **[Q71.](#q71)** What are the key differences between OpenAI Agents SDK, LangGraph, and CrewAI for production agent development?
- **[Q72.](#q72)** What is the chain-of-verification (CoVe) technique and when is it valuable?
- **[Q73.](#q73)** How do you handle a situation where a user's query spans multiple documents that contradict each other?
- **[Q74.](#q74)** What is agent memory staleness and how do you manage it?

## Questions and Answers

Answers are transcribed from the source PDF ([pages 25–27](../docs/GenAI-Interview.pdf)). Tables, callouts and code follow the PDF; only the layout is adapted to Markdown.

<a id="q59"></a>

### Q59. What is the difference between LangChain and LangGraph? When do you use each?

- LangChain: toolkit for building chains, prompts, retrieval, tools. Great for simple to moderately complex flows.
- LangGraph: graph-based orchestration for complex, stateful, cyclical agent workflows.

Use LangChain for rapid prototyping. Use LangGraph for complex agents with loops, branching, persistence.

[↑ Back to question list](#questions)

<a id="q60"></a>

### Q60. What is the "needle in a haystack" test and what does it evaluate?

Evaluates long-context retrieval of a specific fact ("needle") placed in a large irrelevant context ("haystack"). Measures recall across increasing context lengths.

[↑ Back to question list](#questions)

<a id="q61"></a>

### Q61. What is RLHF and why did DPO largely replace PPO-based RLHF for most post-training?

- RLHF: trains model using human feedback (rankings) to align behavior.
- DPO (Direct Preference Optimization): simpler, more stable, avoids reward model + PPO complexity, lower compute, fewer hyperparameters.

[↑ Back to question list](#questions)

<a id="q62"></a>

### Q62. What is chain-of-thought prompting and when does it help?

Encourages the model to reason step-by-step before answering. Helps on complex reasoning, math, logic, multi-hop problems.

[↑ Back to question list](#questions)

<a id="q63"></a>

### Q63. How does an embedding model differ from a generation model architecturally?

- Embedding model: encoder-only (e.g. BERT-style). Outputs dense vectors.
- Generation model: decoder-only or encoder-decoder. Autoregressive token generation.

[↑ Back to question list](#questions)

<a id="q64"></a>

### Q64. What is the difference between streaming and batch inference for LLMs?

- Streaming: returns tokens incrementally; lower perceived latency; holds connection open.
- Batch: processes many requests together; higher throughput; better GPU utilization; higher first-token latency.

[↑ Back to question list](#questions)

<a id="q65"></a>

### Q65. What is a system prompt and what are the security implications of putting sensitive logic in it?

System prompt sets model behavior and rules. If leaked via jailbreak / prompt injection, attackers can bypass guardrails. Don't put secrets, keys, or critical logic there.

[↑ Back to question list](#questions)

<a id="q66"></a>

### Q66. What is a reranker and how does it differ from a retriever in a RAG pipeline?

- Retriever: finds relevant docs (recall).
- Reranker: reorders candidates by relevance (using cross-encoder or LLM). Improves precision.

[↑ Back to question list](#questions)

<a id="q67"></a>

### Q67. What is few-shot prompting via retrieval (also called RAG-based few-shot)?

Retrieve example Q&A pairs similar to the user query and include them in the prompt as in-context examples.

[↑ Back to question list](#questions)

<a id="q68"></a>

### Q68. What is constitutional AI and how does Anthropic use it to align Claude?

A framework with principles and critiques where the model critiques and revises its own responses. Anthropic trains Claude using this approach alongside other alignment methods.

[↑ Back to question list](#questions)

<a id="q69"></a>

### Q69. How do you debug an agent that is producing incorrect final answers despite having the correct information in its tool outputs?

- Inspect intermediate steps and tool I/O.
- Verify prompt instructions and output format.
- Check reasoning/aggregation step.
- Add unit tests, traces, and step-level evals.

[↑ Back to question list](#questions)

<a id="q70"></a>

### Q70. What is the difference between document Q&A and conversational RAG, and how do they require different architectures?

- Document Q&A: single-turn over a static corpus.
- Conversational RAG: multi-turn with chat history, follow-ups, coreference.

> *Conversational RAG needs memory, history-aware retrieval, and query rewriting.*

[↑ Back to question list](#questions)

<a id="q71"></a>

### Q71. What are the key differences between OpenAI Agents SDK, LangGraph, and CrewAI for production agent development?

- OpenAI Agents SDK: opinionated, tight with OpenAI models/tools; fastest path to production.
- LangGraph: flexible, low-level control; best for custom complex workflows.
- CrewAI: role-based multi-agent teams; good for collaborative task execution.

[↑ Back to question list](#questions)

<a id="q72"></a>

### Q72. What is the chain-of-verification (CoVe) technique and when is it valuable?

Model generates multiple reasoning paths or answers, then verifies / cross-checks them to arrive at the most consistent answer. Valuable for high-stakes or error-prone tasks.

[↑ Back to question list](#questions)

<a id="q73"></a>

### Q73. How do you handle a situation where a user's query spans multiple documents that contradict each other?

- Retrieve widely, then rerank.
- Surface both sides with sources.
- Ask clarifying question if needed.
- Use citation + confidence, let user decide.

[↑ Back to question list](#questions)

<a id="q74"></a>

### Q74. What is agent memory staleness and how do you manage it?

Staleness = memory contains outdated facts or state. Manage by: TTLs, refresh policies, source re-checks, versioned memory, summarization with timestamps.

> *Master the fundamentals. Apply them in context. Ship reliable AI systems.*

[↑ Back to question list](#questions)

## Preparation Guidance

- Use this section as breadth practice: aim for crisp, accurate definitions plus one "when to use it" sentence.
- Be careful with fast-moving topics (frameworks, SDKs, training methods); verify current details before an interview.
- Link related ideas across sections (for example reranking with Q11 and Q66, or prompt compression with Q19 and Q48).
- Use the questions as a self-quiz: cover the answers in the PDF and check yourself afterwards.

---

⬅️ [Section 7: Model Selection & Infrastructure](../model-selection-infrastructure/README.md) | [🏠 Main README](../README.md) | [Section 9: System Design Short-Answer](../system-design/README.md) ➡️
