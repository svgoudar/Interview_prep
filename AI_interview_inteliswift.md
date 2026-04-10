# 🧠 SECTION 1: AGENTIC AI / LLM SYSTEM DESIGN QUESTIONS

### Q1.

**“Tell me about a time when you designed and implemented an agentic AI workflow using LLMs like OpenAI or Bedrock. What was the workflow?”**

**What they expect (options/directions):**

* (A) High-level architecture explanation
* (B) Step-by-step workflow (input → processing → output)
* (C) Tools used (LangChain, LangGraph, APIs)
* (D) Real-world impact

---

### Q2.

**“Which components did you personally implement in that workflow?”**

**Expected answer dimensions:**

* (A) Model orchestration
* (B) Tool/function calling
* (C) Prompt engineering
* (D) Context management
* (E) Memory handling

---

### Q3.

**“What were the main technical trade-offs or challenges you encountered?”**

**Options interviewer is probing:**

* (A) Latency vs accuracy
* (B) Cost vs performance
* (C) Hallucination vs strict grounding
* (D) Scalability issues
* (E) Data availability

---

# 🔍 SECTION 2: RAG (Retrieval-Augmented Generation)

### Q4.

**“In your previous projects, what was your experience building Retrieval-Augmented Generation (RAG) pipelines with vector databases and embedding workflows?”**

**Expected answer areas:**

* (A) End-to-end pipeline design
* (B) Embedding generation
* (C) Retrieval logic
* (D) Integration with LLM

---

### Q5.

**“Tell me about a specific implementation you worked on.”**

**Options:**

* (A) Use case (support tickets, healthcare, etc.)
* (B) Data pipeline
* (C) Retrieval + generation flow

---

### Q6.

**“What choices did you make for embeddings and vector store?”**

**Expected options:**

* (A) OpenAI embeddings / SBERT
* (B) FAISS / OpenSearch / Pinecone
* (C) Trade-offs (latency, cost, accuracy)

---

### Q7.

**“How did you validate retrieval quality and end-user relevance?”**

**Options:**

* (A) Precision@K / Recall@K
* (B) Human evaluation
* (C) A/B testing
* (D) Feedback loops

---

# ☁️ SECTION 3: CLOUD + MLOPS + OBSERVABILITY

### Q8.

**“You’ve worked on RAG pipelines with FastAPI endpoints, AWS services like ECS, S3, and Lambda, and OpenSearch as your vector database…”**

👉 Follow-up question:

**“Tell me about a time when you implemented CI/CD and observability for AI services.”**

---

### Q9.

**“What tools did you use?”**

**Options:**

* (A) Docker
* (B) Jenkins
* (C) CloudFormation / CDK
* (D) Kubernetes

---

### Q10.

**“What did you monitor?”**

**Options:**

* (A) Latency
* (B) Hallucination rate
* (C) API usage
* (D) Model performance
* (E) System health

---

### Q11.

**“What pipelines, metrics, and alerts did you put in place?”**

**Expected areas:**

* (A) Data pipeline monitoring
* (B) Model drift alerts
* (C) SLA-based alerts
* (D) Error tracking

---

### Q12.

**“How did these practices change how the team operated or responded to incidents?”**

**Options:**

* (A) Faster incident response
* (B) Better debugging
* (C) Reduced downtime
* (D) Improved reliability

---

# 💻 SECTION 4: CODING QUESTION (RAG IMPLEMENTATION)

### Q13. (MAIN CODING PROBLEM)

**“Build a small Python module that:”**

#### Requirements:

1. Accepts a corpus of text documents
2. Creates embeddings for each document
3. Builds an in-memory vector index supporting similarity search
4. Exposes a function:

   ```
   retrieve_and_answer(query, k)
   ```

   that:

   * Retrieves top-k most similar documents
   * Generates an answer using a mock LLM function

---

### Example Given:

* Documents:

  ```
  ["A: Python is a programming language",
   "B: FAISS is a vector index"]
  ```

* Query:

  ```
  "What is Python?"
  ```

* Expected Behavior:

  * Retrieve relevant passages
  * Return:

    * Top-k documents
    * LLM-generated answer

---

### Important Constraint:

**“Do NOT rely on external managed services. Use an in-memory index.”**

---

### What they evaluate (hidden options):

* (A) Embedding generation logic
* (B) Similarity computation (cosine similarity)
* (C) Efficient retrieval (top-k)
* (D) Clean API design
* (E) Integration with LLM mock

---

### Code Structure Provided:

#### Classes expected:

* `SimpleEmbedder`
* `InMemoryVectorStore`
* `SimpleRAG`

#### Functions:

* `embed()`
* `similarity_search()`
* `retrieve_and_answer()`
* `mock_llm_generate()`


```
import typing
import numpy as np


# -----------------------------
# 1. Simple Embedder
# -----------------------------
class SimpleEmbedder:
    def __init__(self):
        pass

    def embed(self, texts: typing.List[str]) -> np.ndarray:
        """
        Deterministic embedding using simple hashing of characters.
        Output shape: (len(texts), embedding_dim)
        """
        embedding_dim = 10
        embeddings = []

        for text in texts:
            vec = np.zeros(embedding_dim)
            for i, ch in enumerate(text):
                vec[i % embedding_dim] += ord(ch)
            # normalize
            norm = np.linalg.norm(vec)
            if norm > 0:
                vec = vec / norm
            embeddings.append(vec)

        return np.array(embeddings)


# -----------------------------
# 2. In-Memory Vector Store
# -----------------------------
class InMemoryVectorStore:
    def __init__(self, embeddings: np.ndarray, documents: typing.List[str]):
        self.embeddings = embeddings
        self.documents = documents

    def similarity_search(
        self, query_embedding: np.ndarray, k: int = 3
    ) -> typing.List[typing.Tuple[str, float]]:
        """
        Cosine similarity search
        Returns top-k (document, score)
        """
        scores = []

        for i, emb in enumerate(self.embeddings):
            score = np.dot(query_embedding, emb)
            scores.append((self.documents[i], score))

        # sort by similarity descending
        scores.sort(key=lambda x: x[1], reverse=True)

        return scores[:k]


# -----------------------------
# 3. Mock LLM
# -----------------------------
def mock_llm_generate(prompt: str) -> str:
    """
    Simple mock response generator
    """
    return f"LLM Answer based on context: {prompt[:150]}"


# -----------------------------
# 4. Simple RAG Pipeline
# -----------------------------
class SimpleRAG:
    def __init__(self, docs: typing.List[str]):
        self.docs = docs
        self.embedder = SimpleEmbedder()

        # create embeddings
        self.doc_embeddings = self.embedder.embed(docs)

        # vector store
        self.vector_store = InMemoryVectorStore(self.doc_embeddings, docs)

    def retrieve_and_answer(self, query: str, k: int = 3) -> dict:
        # embed query
        query_embedding = self.embedder.embed([query])[0]

        # retrieve top-k docs
        retrieved = self.vector_store.similarity_search(query_embedding, k)

        retrieved_docs = [doc for doc, _ in retrieved]

        # build prompt
        context = "\n".join(retrieved_docs)
        prompt = f"Context:\n{context}\n\nQuestion: {query}\nAnswer:"

        # mock LLM call
        answer = mock_llm_generate(prompt)

        return {
            "query": query,
            "documents": retrieved_docs,
            "answer": answer,
        }


# -----------------------------
# Example Usage
# -----------------------------
if __name__ == "__main__":
    docs = [
        "A: Python is a programming language.",
        "B: FAISS is a vector index.",
        "C: Python supports multiple paradigms.",
    ]

    rag = SimpleRAG(docs)

    result = rag.retrieve_and_answer("What is Python?", k=2)

    print(result)

```
---

# 🎯 SECTION 5: IMPLICIT FOLLOW-UP QUESTIONS (VERY IMPORTANT)

Based on FAANG-style interviews, these are **hidden follow-ups**:

### Q14.

**How would you scale this system beyond in-memory?**

* (A) FAISS
* (B) Vector DB (Pinecone, OpenSearch)
* (C) Distributed retrieval

---

### Q15.

**How would you improve retrieval accuracy?**

* (A) Better embeddings
* (B) Reranking (cross-encoder)
* (C) Hybrid search (BM25 + vector)

---

### Q16.

**How would you reduce hallucinations?**

* (A) Grounding via RAG
* (B) Prompt constraints
* (C) Output validation

---

### Q17.

**How would you handle latency issues?**

* (A) Caching
* (B) ANN search
* (C) Batch inference

---

# 🚨 CRITICAL INTERVIEW INSIGHT

This interview is testing **3 layers simultaneously**:

### 1. LLM + Agentic Design

* LangChain / LangGraph understanding
* Tool calling
* Workflow orchestration

### 2. RAG Depth

* Embeddings
* Vector DB
* Retrieval quality

### 3. Engineering Depth

* System design
* MLOps
* Scalability
* Observability

---
