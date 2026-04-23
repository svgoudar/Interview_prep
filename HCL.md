## 1. Failure Modes in RAG Applications

**Retrieval Failures**

* Low recall (relevant docs not retrieved)
* Low precision (irrelevant/noisy chunks retrieved)
* Embedding mismatch (query vs doc distribution drift)
* Poor chunking (context fragmentation or over-chunking)

**Generation Failures**

* Hallucination (LLM fabricates beyond context)
* Context ignoring (model doesn’t use retrieved data)
* Over-reliance on prompt instead of retrieval

**Data / Ingestion Failures**

* Duplicate documents → biased retrieval
* Stale data (no re-indexing strategy)
* Incorrect parsing (tables/images lost meaning)

**Ranking Failures**

* Weak reranking → wrong top-k selection
* No hybrid search (keyword + vector imbalance)

**System Failures**

* Latency spikes (fan-out retrieval, retries)
* Token overflow (context too large)
* Cost explosion (multi-query + reranking)

---

## 2. CRAG (Corrective RAG)

**Concept:** Improves RAG by validating retrieval quality before generation.

**Flow:**

1. Retrieve documents
2. **Retriever evaluator (grader)** checks relevance
3. If irrelevant → **query rewrite / re-retrieve**
4. Then generate answer

**Key Idea:**

* Adds **self-correction loop before generation**
* Prevents hallucination due to bad retrieval

**Production Add-ons:**

* Confidence scoring
* Multi-hop retrieval fallback
* Guardrails for domain constraints

---

## 3. Deduplication in Production RAG Ingestion

### Techniques (layered approach)

**1. Hash-based deduplication**

* SHA256(content) → remove exact duplicates

**2. Semantic deduplication**

* Embedding similarity:

  ```python
  cosine_sim > 0.95 → duplicate
  ```
* Use clustering (K-Means / HNSW neighborhood)

**3. Chunk-level dedup**

* Remove repeated headers/footers
* Sliding window overlap control

**4. Metadata-level dedup**

* Same `doc_id + version` → overwrite
* Maintain ingestion registry (DynamoDB-style)

**5. Near-duplicate detection**

* MinHash / Locality Sensitive Hashing (LSH)

**Production pattern:**

* Stage 1: Raw dedup (hash)
* Stage 2: Semantic dedup (embedding)
* Stage 3: Index-time filtering

---

## 4. Memory in AI Agents (Production RAG)

### Types of Memory

**1. Short-term (Working Memory)**

* Conversation buffer
* Last N interactions
* Stored in Redis / in-memory

**2. Long-term Memory**

* Vector DB (OpenSearch, Pinecone)
* Stores past interactions + knowledge

**3. Episodic Memory**

* Past agent actions (logs, traces)
* Used for reasoning improvement

**4. Semantic Memory**

* Structured knowledge (facts, embeddings)

---

### Memory Challenges

* Noise accumulation
* Irrelevant context injection
* Cost (token explosion)

---

### Production Strategy

* Relevance scoring (recency + importance)
* Memory summarization
* TTL (expiry for old memory)
* Retrieval filtering before injection

---

## 5. Deterministic Output (True/False)

LLMs are probabilistic → need constraints.

### Methods

**1. Temperature = 0**

* Reduces randomness

**2. Structured output / schema enforcement**

* JSON schema / Pydantic:

  ```json
  { "answer": true }
  ```

**3. Classification prompt**

* Force binary decision:

  > Answer ONLY "TRUE" or "FALSE"

**4. Logit bias / constrained decoding**

* Allow only tokens: TRUE / FALSE

**5. Post-validation**

* Reject invalid outputs → retry

---

## 6. RunnableExpression

From LangChain Expression Language (LCEL)

**Definition:**

* A composable unit representing a computation pipeline

**Example:**

```python
chain = prompt | llm | parser
```

Each component is a **Runnable**

**RunnableExpression =**

* Pipeline of transformations
* Supports async, batching, streaming

---

## 7. Types of Tools (in Agents)

**1. API Tools**

* External services (REST APIs)

**2. Retrieval Tools**

* Vector DB search

**3. Computation Tools**

* Python execution, math engines

**4. Action Tools**

* DB write, ticket creation

**5. Communication Tools**

* Email, Slack, notifications

**6. System Tools**

* File system, logs, observability

---

## 8. Agent Infinite Loop Handling

### Causes

* Bad tool responses
* Poor stopping criteria
* Recursive reasoning loops

### Solutions

**1. Max iteration limit**

```python
max_steps = 5
```

**2. Timeout-based cutoff**

**3. State-based termination**

* If no new info → stop

**4. Loop detection**

* Same action repeated → break

**5. Guardrails**

* Validate tool outputs before next step

---

## 9. Concurrent State Modification (2 Agents)

### Problem

Race condition on shared state

### Solutions

**1. Optimistic Locking**

* Version check before update

**2. Pessimistic Locking**

* Lock resource (Redis lock / DB lock)

**3. Event sourcing**

* Append-only logs instead of overwrite

**4. Message queue (decoupling)**

* Serialize updates via queue

**5. CRDTs (advanced)**

* Conflict-free distributed updates

---

## 10. Types of Agent Architectures

### 1. ReAct Agent

* Reason + Act loop
* Tool calling with reasoning traces

### 2. Plan-and-Execute

* Planner → Executor agents
* Better for complex workflows

### 3. Multi-Agent Systems

* Multiple specialized agents
* Coordinator / orchestrator

### 4. Hierarchical Agents

* Manager agent → worker agents

### 5. Reflection Agents

* Self-evaluation + correction

### 6. Tool-Calling Agents

* LLM decides which tool to use

### 7. Graph-based Agents

* State machine (LangGraph-style)
* Deterministic flows + branching

---

## Summary (Interview-Ready)

* **RAG failures** → retrieval, generation, ranking, ingestion
* **CRAG** → adds retrieval validation + correction loop
* **Deduplication** → hash + semantic + metadata layers
* **Memory** → short-term, long-term, episodic, semantic
* **Determinism** → temp=0 + schema + constrained decoding
* **RunnableExpression** → composable pipeline abstraction
* **Tools** → API, retrieval, compute, action
* **Loop control** → step limits + loop detection
* **Concurrency** → locking, queues, event sourcing
* **Agent architectures** → ReAct, multi-agent, hierarchical, graph


## WHat is graphRAG

