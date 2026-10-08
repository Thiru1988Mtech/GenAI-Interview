# Section 6 — BEHAVIORAL

**Question range:** Q39–Q42 (4 questions)  
**Source:** [GenAI-Interview.pdf](../docs/GenAI-Interview.pdf), pages 19–20

## What This Section Covers

Experience-based questions about improving an AI system, trading off quality against cost, managing a production incident, and staying current in the field.

**Key topics:** Impact and ownership, Quality vs. cost trade-offs, Incident management, Continuous learning.

## Questions

Question wording and numbering are taken directly from the source PDF. Select a question to jump to its answer.

- **[Q39.](#q39)** Tell me about a time you improved the performance of an AI system you owned.
- **[Q40.](#q40)** Describe a time you had to make a tradeoff between model quality and cost in production.
- **[Q41.](#q41)** Tell me about a production AI incident you managed. What happened and what did you change?
- **[Q42.](#q42)** How do you stay current in this field?

## Questions and Answers

Answers are transcribed from the source PDF ([pages 19–20](../docs/GenAI-Interview.pdf)). Tables, callouts and code follow the PDF; only the layout is adapted to Markdown.

<a id="q39"></a>

### Q39. Tell me about a time you improved the performance of an AI system you owned.

**Situation:**

Our RAG chatbot had low answer accuracy (~62%) and users often said answers were incomplete.

**Action:**

- Analyzed failures: low retrieval recall + poor chunking
- Switched from fixed 500-token chunks to hierarchical chunking (sections + paragraphs)
- Added a cross-encoder reranker (bge-reranker-base)
- Improved query rewriting with MyBE + multi-query
- Added citation requirement in the prompt
- Built an eval set and tracked RAGAS metrics

**Result:**

- Answer correctness 62% → 86%
- Hallucination rate 28% → 9%
- User satisfaction 3.2/5 → 4.5/5
- Support tickets tied to wrong answers ↓ 60%

> **Key takeaway:** *Data + retrieval quality + evaluation = biggest lever.*

[↑ Back to question list](#questions)

<a id="q40"></a>

### Q40. Describe a time you had to make a tradeoff between model quality and cost in production.

**Context:**

A summarization feature was costing ~$6K/month on GPT-4. Usage was high, but we needed to optimize.

| Option A (High Quality) GPT-4 | Option B (Cost Optimized) GPT-3.5-turbo + Rerank |
|-------------------------------|---------------------------------------------------|
| Best quality, handles edge cases, high cost, slower | 5× cheaper, faster, slightly lower fluency, needs prompt tuning |

**What I did:**

- Used GPT-3.5-turbo with an improved prompt + length control
- Added a low-cost filter (LLM-as-judge) to catch bad outputs
- Escalated only complex inputs to GPT-4 (10–15% of traffic)

**Outcome:**

- Cost reduced by ~72% ($6K → $1.7K/month)
- Quality impact minimal (GPT-4.3 → 4.1/5)
- Latency improved ~35%

> **Lesson:** *Optimize for the 80% first, and route the rest.*

[↑ Back to question list](#questions)

<a id="q41"></a>

### Q41. Tell me about a production AI incident you managed. What happened and what did you change?

**Incident:**

Users reported the chatbot giving confident but incorrect answers after a data-source update.

**Impact:**

- ~18% of answers contained outdated information
- Trust and CSAT dropped
- Support volume increased

**Root Cause:**

Our ingestion job partially failed, so the index had a mix of old and new documents. We had no freshness checks.

**What I did:**

- Rolled back to last known-good index
- Added data freshness validation + alerts
- Implemented source rebuild (versioning), not incremental patch
- Added a "last updated" date to answers
- Built a canary and end-to-end detail regression checks

**Outcome:**

- Issue resolved in ~2 hours
- No repeat incidents since implementing safeguards
- CSAT recovered within a week

**What I changed long-term:**

Better monitoring, data validation, and evals in CI/CD for our RAG pipeline.

[↑ Back to question list](#questions)

<a id="q42"></a>

### Q42. How do you stay current in this field?

- Daily Learning (30 mins): read research papers / arXiv summaries; follow key blogs (OpenAI, Anthropic, LangChain, Vercel AI, Hugging Face, Pinecone)
- Hands-on Practice: build small projects and experiment with new tools / models; reproduce open-source projects and learn by doing
- Community: engage in Discords / r/MachineLearning / r/LocalLLaMA; contribute to open source & share learnings
- Newsletters: subscribe to TLDR AI, Import AI, Latent Space, The Batch; skim weekly to catch trends and new releases
- Continuous Reflection: keep a learning journal / changelog; run postmortems on wins & failures
- Conferences & Talks: watch talks from NeurIPS, ICML, ACL, PyData, etc.; attend virtual meetups and webinars

> **Mindset:** *"Stay curious, ship often, and keep learning in public."*

> *Behavioral is about impact, ownership, and continuous growth. Show how you think, build, learn, and make things better.*

[↑ Back to question list](#questions)

## Preparation Guidance

- Prepare your own stories; the PDF shows one example structure (situation, action, result), not answers to copy.
- Use real numbers from your experience wherever you can.
- Include what you learned and what you changed afterwards, not only what went well.
- Practice telling each story in about two minutes.

---

⬅️ [Section 5: Coding](../coding/README.md) | [🏠 Main README](../README.md) | [Section 7: Model Selection & Infrastructure](../model-selection-infrastructure/README.md) ➡️
