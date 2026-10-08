# Section 1 — BEGINNER

**Question range:** Q1–Q10 (10 questions)  
**Source:** [GenAI-Interview.pdf](../docs/GenAI-Interview.pdf), pages 3–5

## What This Section Covers

Core LLM vocabulary and mental models: what an LLM is, tokenization, embeddings, RAG, vector databases, prompt engineering, the context window, hallucination, the choice between fine-tuning / RAG / prompt engineering, and the three transformer architecture families.

**Key topics:** LLMs vs. traditional ML, Tokenization, Embeddings, RAG basics, Vector databases, Prompt engineering, Context window, Hallucination, Fine-tuning vs. RAG vs. prompting, Encoder-only / decoder-only / encoder-decoder.

## Questions

Question wording and numbering are taken directly from the source PDF. Select a question to jump to its answer.

- **[Q1.](#q1)** What is a Large Language Model and how is it different from traditional ML models?
- **[Q2.](#q2)** What is tokenization and why does it matter?
- **[Q3.](#q3)** What is an embedding and what is it used for?
- **[Q4.](#q4)** What is RAG and what problems does it solve?
- **[Q5.](#q5)** What is a vector database and how is it different from a traditional database?
- **[Q6.](#q6)** What is prompt engineering and why does it matter?
- **[Q7.](#q7)** What is the context window and why does it matter?
- **[Q8.](#q8)** What is hallucination and what causes it?
- **[Q9.](#q9)** Explain the difference between fine-tuning, RAG, and prompt engineering. When do you use each?
- **[Q10.](#q10)** What is the difference between encoder-only, decoder-only, and encoder-decoder transformer architectures?

## Questions and Answers

Answers are transcribed from the source PDF ([pages 3–5](../docs/GenAI-Interview.pdf)). Tables, callouts and code follow the PDF; only the layout is adapted to Markdown.

<a id="q1"></a>

### Q1. What is a Large Language Model and how is it different from traditional ML models?

LLMs are deep learning models (usually transformer-based) trained on vast amounts of text to understand and generate human-like language. Traditional ML models are task-specific, use hand-crafted features, and need more labeled data.

[↑ Back to question list](#questions)

<a id="q2"></a>

### Q2. What is tokenization and why does it matter?

Tokenization converts text into smaller units (tokens) that models can understand. It affects context length, cost, performance, and how text is represented by the model.

> **Example:** "Hello, world!"  
> Hello | , | world | !

[↑ Back to question list](#questions)

<a id="q3"></a>

### Q3. What is an embedding and what is it used for?

An embedding is a dense vector representation of text (or other data) in a high-dimensional space. Used for semantic search, clustering, recommendations, similarity, and as input to many NLP systems.

> **Text → [0.12, -0.45, 0.33, ...] (Embedding vector)**

[↑ Back to question list](#questions)

<a id="q4"></a>

### Q4. What is RAG and what problems does it solve?

RAG (Retrieval-Augmented Generation) retrieves relevant documents from a knowledge base and uses them as context for the LLM. Solves problems like outdated knowledge, hallucinations, and lack of domain-specific information.

> **Query → Retriever (Vector DB) → LLM → Answer**  
> **(Retriever pulls Top-K docs into the LLM's context)**

[↑ Back to question list](#questions)

<a id="q5"></a>

### Q5. What is a vector database and how is it different from a traditional database?

A vector DB stores and indexes embeddings for similarity search (e.g. cosine similarity). Traditional DBs store structured data (rows/columns) and are optimized for exact matches and transactions.

| Vector DB | Traditional DB |
|-----------|----------------|
| Finds similar vectors | Finds exact matches |

[↑ Back to question list](#questions)

<a id="q6"></a>

### Q6. What is prompt engineering and why does it matter?

Prompt engineering is the art of crafting inputs (prompts) to get better, more reliable, and more relevant outputs from an LLM.

[↑ Back to question list](#questions)

<a id="q7"></a>

### Q7. What is the context window and why does it matter?

The context window is the maximum number of tokens the model can "see" in one request (prompt + response). It limits how much information you can provide and affects cost and performance.

> **Context Window = System Prompt + User Prompt + Response**  
> **(Limited by the model — fixed capacity)**

[↑ Back to question list](#questions)

<a id="q8"></a>

### Q8. What is hallucination and what causes it?

Hallucination is when the model generates confident but false or unsupported information. Causes: missing context, ambiguous questions, over-generalization, or training-data limitations.

> **Model says: "The CEO of Apple in 2021 was \_\_\_" (incorrect / unverifiable info)**

[↑ Back to question list](#questions)

<a id="q9"></a>

### Q9. Explain the difference between fine-tuning, RAG, and prompt engineering. When do you use each?

- Prompt Engineering: No training needed. Use for general tasks, quick iteration.
- RAG: No model training. Use when you need up-to-date or private/domain data.
- Fine-tuning: Train the model on your data. Use for specialized behavior, format control, or when the model needs to "learn" your domain deeply.

| Method | Use When… |
|--------|-----------|
| Prompt Eng. | General purpose, quick wins |
| RAG | Need external / up-to-date knowledge |
| Fine-tuning | Need the model to learn domain-specific patterns |

[↑ Back to question list](#questions)

<a id="q10"></a>

### Q10. What is the difference between encoder-only, decoder-only, and encoder-decoder transformer architectures?

| Encoder-Only (e.g. BERT) | Decoder-Only (e.g. GPT) | Encoder-Decoder (e.g. T5) |
|--------------------------|-------------------------|---------------------------|
| Understands input text. Used for classification, search, embeddings. | Generates text autoregressively (one token at a time). Used for generation, chat, code. | Encoder understands input, decoder generates output. Used for translation, summarization. |

> **Note:** *Most LLMs you use daily (ChatGPT, Claude, Llama, Mistral) are Decoder-Only.*

[↑ Back to question list](#questions)

## Preparation Guidance

- Be able to answer each question in two to three sentences, then add one concrete example.
- Practice drawing the basic RAG flow (query → retriever → LLM → answer) from memory.
- Know when you would reach for prompt engineering, RAG, or fine-tuning, and be ready to justify the choice.
- These are the questions interviewers use to check foundations; shaky answers here make later answers harder to trust.

---

[🏠 Main README](../README.md) | [Section 2: Intermediate](../intermediate/README.md) ➡️
