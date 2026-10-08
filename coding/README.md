# Section 5 — CODING

**Question range:** Q36–Q38 (3 questions)  
**Source:** [GenAI-Interview.pdf](../docs/GenAI-Interview.pdf), pages 16–18

## What This Section Covers

Hands-on coding tasks: calling an LLM API with exponential backoff on rate-limit errors, a minimal RAG pipeline over a list of documents, and an LLM tool / function definition with argument validation.

**Key topics:** Retry with exponential backoff and jitter, Minimal RAG pipeline (embed, retrieve, generate), Tool / function definitions with validation.

## Questions

Question wording and numbering are taken directly from the source PDF. Select a question to jump to its answer.

- **[Q36.](#q36)** Write a function that calls the OpenAI API with exponential backoff on rate-limit errors.
- **[Q37.](#q37)** Write a minimal RAG pipeline that takes a list of text documents and a query.
- **[Q38.](#q38)** Write a function definition for an LLM that searches a database and handles argument validation.

## Questions and Answers

Answers are transcribed from the source PDF ([pages 16–18](../docs/GenAI-Interview.pdf)). Tables, callouts and code follow the PDF; only the layout is adapted to Markdown.

<a id="q36"></a>

### Q36. Write a function that calls the OpenAI API with exponential backoff on rate-limit errors.

```python
import openai
import time
import random
from openai import OpenAI
from openai.error import RateLimitError, APIError, Timeout

client = OpenAI()

def call_openai_with_backoff(messages, model="gpt-4o-mini",
                             max_retries=6, base_delay=1.0, max_delay=60.0):
    attempt = 0
    while True:
        try:
            response = client.chat.completions.create(
                model=model,
                messages=messages,
                timeout=30,
            )
            return response
        except (RateLimitError, Timeout) as e:
            attempt += 1
            if attempt > max_retries:
                raise RuntimeError(f"Max retries ({max_retries}) exceeded. Last error: {e}")
            delay = min(max_delay, base_delay * (2 ** (attempt - 1)))
            jitter = random.uniform(0, delay * 0.1)
            print(f"Rate limit. Retry in {delay + jitter:.2f}s (attempt {attempt})")
            time.sleep(delay + jitter)
        except APIError as e:
            # Non-rate-limit API errors: don't retry
            raise e
```

- Retries on RateLimitError and Timeout
- Exponential backoff: base_delay × 2^(attempt-1)
- Adds jitter after base_delay ranges
- Stops after max_retries

[↑ Back to question list](#questions)

<a id="q37"></a>

### Q37. Write a minimal RAG pipeline that takes a list of text documents and a query.

```python
from typing import List
from openai import OpenAI
import numpy as np

client = OpenAI()
EMBED_MODEL = "text-embedding-3-small"
CHAT_MODEL = "gpt-4o-mini"
TOP_K = 4

# 1) Embed documents
def embed(texts: List[str]) -> np.ndarray:
    res = client.embeddings.create(model=EMBED_MODEL, input=texts)
    return np.array([d.embedding for d in res.data])

# 2) Retrieve top-k by cosine similarity
def retrieve(query, doc_embeddings, docs, top_k=TOP_K):
    q_emb = embed([query])[0]
    sims = doc_embeddings @ q_emb / (
        np.linalg.norm(doc_embeddings, axis=1) * np.linalg.norm(q_emb) + 1e-8)
    top_idx = np.argsort(-sims)[:top_k]
    return [docs[i] for i in top_idx]

# 3) Generate answer
def rag_query(query: str, docs: List[str]) -> str:
    doc_embeds = embed(docs)
    context_docs = retrieve(query, doc_embeds, docs)
    context = "\n\n".join(context_docs)
    system = ("You are a helpful assistant. Answer using ONLY the context. "
              "If the answer isn't in the context, say 'I don't know'.")
    resp = client.chat.completions.create(
        model=CHAT_MODEL,
        messages=[
            {"role": "system", "content": system},
            {"role": "user", "content": f"Context:\n{context}\n\nQuestion: {query}"},
        ],
        temperature=0,
    )
    return resp.choices[0].message.content
```

> **Minimal by design:** *No external index, in-memory search, easy to extend to a persistent vector DB (Qdrant, Pinecone) and reranking.*

[↑ Back to question list](#questions)

<a id="q38"></a>

### Q38. Write a function definition for an LLM that searches a database and handles argument validation.

```python
from typing import Optional, Literal, Dict, Any
from pydantic import BaseModel, Field, validator

class SearchDBArgs(BaseModel):
    table: str = Field(..., description="Table to search, e.g. 'customers'")
    query: str = Field(..., description="Search text or SQL WHERE-style condition")
    limit: int = Field(10, ge=1, le=50)
    filters: Optional[Dict[str, Any]] = None
    sort_by: Optional[str] = None

    @validator("table")
    def table_must_be_allowed(cls, v):
        allowed = {"customers", "orders", "products"}
        if v not in allowed:
            raise ValueError(f"table must be one of {allowed}")
        return v

def search_database(args: SearchDBArgs) -> dict:
    # ... validated args are safe to use in a parameterized query
    return {"rows": [], "count": 0}

# Example tool schema passed to the LLM
tool_schema = {
    "name": "search_database",
    "parameters": SearchDBArgs.schema(),
}
```

- Key points: validates inputs with Pydantic
- Prevents SQL injection via parameterized queries
- Restricts table to an allowlist
- Bounds limit (1–50)
- Returns structured JSON the LLM can read

[↑ Back to question list](#questions)

## Preparation Guidance

- The reference solutions below are reproduced exactly as they appear in the PDF (pages 16-18), including its imports and library calls.
- Practice writing each solution from scratch, without looking, in a plain editor.
- Be ready to explain design choices: which errors to retry, how similarity is computed, why arguments are validated.
- Check SDK and library versions before running any snippet; client libraries change over time (for example, the OpenAI Python SDK exception imports differ between major versions).

---

⬅️ [Section 4: Scenario-Based](../scenarios/README.md) | [🏠 Main README](../README.md) | [Section 6: Behavioral](../behavioral/README.md) ➡️
