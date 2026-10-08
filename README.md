# GenAI Interview Question Bank

A practical GenAI / LLM interview preparation resource, from beginner fundamentals to production-level discussions.

![Questions](https://img.shields.io/badge/questions-80-blue) ![Sections](https://img.shields.io/badge/sections-9-blueviolet) ![Format](https://img.shields.io/badge/format-Markdown%20%2B%20PDF-lightgrey)

## About

This repository contains **80 interview questions** (Q1–Q80) organized into **9 sections**, covering LLM fundamentals, RAG, agents, evaluation, security, model selection, infrastructure, coding, system design, and behavioral topics. Every section README contains the questions **and their reference answers**, transcribed from the original PDF, which remains the source of truth for all content in this repository.

**Author:** Thirumurugan Munusamy  
AI Technical Architect | PhD Researcher (AI & Data Science)  
SRM Institute of Science & Technology, Chennai, India  
[LinkedIn](https://www.linkedin.com/in/thirumurugan-munusamy-77865188/) · [GitHub](https://github.com/Thiru1988Mtech)

## Table of Contents

- [About](#about)
- [Sections](#sections)
- [Topics Covered](#topics-covered)
- [How to Use This Repository](#how-to-use-this-repository)
- [Interview Preparation Roadmap](#interview-preparation-roadmap)
- [Who Is This For?](#who-is-this-for)
- [Resource / PDF](#resource--pdf)
- [Repository Structure](#repository-structure)
- [Keywords](#keywords)
- [License and Attribution](#license-and-attribution)

## Sections

| # | Section | Questions | Count | PDF pages |
|---|---------|-----------|-------|-----------|
| 1 | [Beginner](beginner/README.md) | Q1–Q10 | 10 | 3–5 |
| 2 | [Intermediate](intermediate/README.md) | Q11–Q20 | 10 | 6–8 |
| 3 | [Advanced](advanced/README.md) | Q21–Q30 | 10 | 9–12 |
| 4 | [Scenario-Based](scenarios/README.md) | Q31–Q35 | 5 | 13–15 |
| 5 | [Coding](coding/README.md) | Q36–Q38 | 3 | 16–18 |
| 6 | [Behavioral](behavioral/README.md) | Q39–Q42 | 4 | 19–20 |
| 7 | [Model Selection & Infrastructure](model-selection-infrastructure/README.md) | Q43–Q58 | 16 | 21–24 |
| 8 | [Additional Intermediate](additional-intermediate/README.md) | Q59–Q74 | 16 | 25–27 |
| 9 | [System Design Short-Answer](system-design/README.md) | Q75–Q80 | 6 | 28–29 |
| | **Total** | **Q1–Q80** | **80** | 29 pages (including cover and contents) |



## Topics Covered

- **Fundamentals:** LLM fundamentals, tokenization, embeddings, prompt engineering, context engineering, sampling
- **Retrieval and RAG:** RAG, vector databases, hybrid search, reranking, HNSW / IVF, multi-hop RAG, GraphRAG, RAG failure modes, lost-in-the-middle, long-context evaluation
- **Fine-tuning and training:** LoRA / QLoRA, RLHF / DPO, distillation
- **Agents and frameworks:** ReAct agents, agent safety, agent evaluation, MCP, LangChain / LangGraph, DSPy
- **Evaluation:** RAGAS evaluation, LLM-as-judge, A/B testing
- **Security and privacy:** OWASP LLM security, PII, multi-tenancy
- **Serving and optimization:** vLLM, quantization, speculative decoding, streaming vs. batch inference, prompt compression, latency optimization
- **Production engineering:** API failures and rate limits, production scenarios, production architecture, scaling, LLM gateways, graceful degradation
- **Applied and professional:** Model selection, coding, behavioral questions

## How to Use This Repository

1. Start from the [Sections](#sections) table and open the section that matches your current level or interview type.
2. Read each question and try to answer it out loud or in writing *before* reading the reference answer below it.
3. Each section README lists its questions at the top and gives the reference answer under each question. The original PDF ([`docs/GenAI-Interview.pdf`](docs/GenAI-Interview.pdf)) is available for the original layout.
4. Revisit weak questions after a day or two, and connect related questions across sections.
5. For the coding questions (Q36–Q38), write the solutions yourself first, then compare with the reference code in the [Coding](coding/README.md) section.
6. For the behavioral questions (Q39–Q42), prepare your own stories; the PDF shows one example format.

## Interview Preparation Roadmap

A suggested order. Adjust it to your timeline and target role.

| Stage | Focus | Sections |
|-------|-------|----------|
| 1. Foundations | Core terminology and concepts | [Beginner](beginner/README.md) (Q1–Q10) |
| 2. Core techniques | RAG, evaluation, agents, security | [Intermediate](intermediate/README.md) (Q11–Q20), [Additional Intermediate](additional-intermediate/README.md) (Q59–Q74) |
| 3. Production depth | Latency, scaling, isolation, reliability, models and infrastructure | [Advanced](advanced/README.md) (Q21–Q30), [Model Selection & Infrastructure](model-selection-infrastructure/README.md) (Q43–Q58) |
| 4. Applied practice | Scenarios, hands-on code, and system design | [Scenario-Based](scenarios/README.md) (Q31–Q35), [Coding](coding/README.md) (Q36–Q38), [System Design](system-design/README.md) (Q75–Q80) |
| 5. Communication | Your own stories and mock interviews | [Behavioral](behavioral/README.md) (Q39–Q42) |

## Who Is This For?

- Engineers preparing for GenAI, LLM, RAG, or agent-focused interviews.
- Builders and architects who want a structured review of production concerns.
- Aspiring AI professionals learning the vocabulary and trade-offs of the field.
- Interviewers looking for a reference list of relevant topics.

## Resource / PDF

The complete question bank, with reference answers, diagrams, and code in its original layout, is available as a single PDF:

📄 **[docs/GenAI-Interview.pdf](docs/GenAI-Interview.pdf)** (29 pages)

The PDF is the source of truth. The Markdown files mirror its section structure, question wording and answers for easier browsing and searching on GitHub.

## Repository Structure

```text
GenAI-Interview/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   └── GenAI-Interview.pdf
├── beginner/
│   └── README.md
├── intermediate/
│   └── README.md
├── advanced/
│   └── README.md
├── scenarios/
│   └── README.md
├── coding/
│   └── README.md
├── behavioral/
│   └── README.md
├── model-selection-infrastructure/
│   └── README.md
├── additional-intermediate/
│   └── README.md
└── system-design/
    └── README.md
```

## Keywords

`genai` `generative-ai` `llm` `large-language-models` `rag` `retrieval-augmented-generation` `vector-database` `prompt-engineering` `ai-agents` `mlops` `interview-questions` `interview-preparation` `system-design` `langchain` `langgraph`

## License and Attribution

Released under the terms in [LICENSE](LICENSE). Compiled and shared by Thirumurugan Munusamy.
