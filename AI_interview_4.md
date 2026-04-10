Here’s a clear summary of **all the visible questions and tasks** from the images you shared (from your interviews and assessments on 09/04/2026):

### 1. Python, LLM Integration Coding Task (IntelliSwift)
**Question:**  
Build a small Python module for text document embeddings and retrieval:

- Accept a corpus of text documents
- Create in-memory embeddings for each document
- Build a vector index supporting similarity search
- Expose a `retrieve_and_answer` function that returns top-k most similar documents + a mock LLM-generated answer

**Example:**
- Documents: `["A: Python is a programming language.", "B: FAISS is a vector index."]`
- Query: `"What is Python?"`
- Expected: Retrieve relevant passages and return them with an LLM-generated answer string.

**Note:** Use in-memory index only (no external services). Evaluation checks embedding generation, similarity search, retrieval, and mock LLM integration.

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

### 2. SQL Assessment (Happiest Minds – Aiml Engineer)
**Question 1: Sales Performance By Region And Product**

A retail company wants to analyze sales performance across regions and product categories.

**Tables:**
- `sales` (sale_id, product_id, region_id, quantity_sold, sale_date)
- `products` (product_id, category_id, product_name)
- `categories` (category_id, category_name)
- `regions` (region_id, region_name)

**Task:**  
Write a PostgreSQL query to calculate the **total quantity sold for each product category in each region**.  
Display: `region_name`, `category_name`, `total_quantity_sold`.  
Order by `region_name` first, then `category_name`.

**Constraint:** Handle cases where there are **no sales** in a region or category (do not exclude them).

```sql

SELECT 
    r.region_name,
    c.category_name,
    COALESCE(SUM(s.quantity), 0) AS total_quantity_sold
FROM regions r
CROSS JOIN categories c
LEFT JOIN products p 
    ON p.category_id = c.category_id
LEFT JOIN sales s 
    ON s.product_id = p.product_id 
    AND s.region_id = r.region_id
GROUP BY r.region_name, c.category_name
ORDER BY r.region_name, c.category_name;

```

---

### 3. DSA Assessment (OpenIntervue)
**Problem:** Level Order Traversal of a Binary Tree (represented as graph/adjacency list)  
- Return values of nodes level by level, from left to right.
- If tree is empty, return empty list.

**Another Problem:** Shortest Path in Graph (Dijkstra’s algorithm) with weighted edges.

**Another Problem:** Topological Sort / Course Schedule with prerequisites (detect cycles, return valid order or empty if impossible).


```
from collections import deque, defaultdict
import heapq


# ---------------------------------------------------
# 1) Level Order Traversal of a Binary Tree
# Tree represented as adjacency:
# {
#   node: [left_child, right_child]
# }
# values:
# {
#   node: value
# }
# root: root node id
# ---------------------------------------------------
def level_order_traversal(adj, values, root):
    if root is None or root not in values:
        return []

    result = []
    q = deque([root])

    while q:
        level_size = len(q)
        level = []

        for _ in range(level_size):
            node = q.popleft()
            level.append(values[node])

            children = adj.get(node, [])
            for child in children:
                if child is not None:
                    q.append(child)

        result.append(level)

    return result


# Example:
# adj_tree = {
#     1: [2, 3],
#     2: [4, 5],
#     3: [6, 7]
# }
# values_tree = {
#     1: 10, 2: 20, 3: 30, 4: 40, 5: 50, 6: 60, 7: 70
# }
# print(level_order_traversal(adj_tree, values_tree, 1))
# Output: [[10], [20, 30], [40, 50, 60, 70]]


# ---------------------------------------------------
# 2) Dijkstra’s Algorithm for Shortest Path
# Graph represented as:
# {
#   node: [(neighbor, weight), ...]
# }
# Returns shortest distance from source to all nodes.
# ---------------------------------------------------
def dijkstra(graph, source):
    distances = {node: float("inf") for node in graph}
    distances[source] = 0

    min_heap = [(0, source)]

    while min_heap:
        curr_dist, node = heapq.heappop(min_heap)

        if curr_dist > distances[node]:
            continue

        for neighbor, weight in graph.get(node, []):
            new_dist = curr_dist + weight
            if new_dist < distances.get(neighbor, float("inf")):
                distances[neighbor] = new_dist
                heapq.heappush(min_heap, (new_dist, neighbor))

    return distances


# Example:
# graph_weighted = {
#     'A': [('B', 1), ('C', 4)],
#     'B': [('C', 2), ('D', 5)],
#     'C': [('D', 1)],
#     'D': []
# }
# print(dijkstra(graph_weighted, 'A'))
# Output: {'A': 0, 'B': 1, 'C': 3, 'D': 4}


# ---------------------------------------------------
# 3) Topological Sort / Course Schedule
# num_courses = total number of courses labeled 0..num_courses-1
# prerequisites = [(course, prereq), ...]
# Return valid order or [] if cycle exists.
# ---------------------------------------------------
def topological_sort(num_courses, prerequisites):
    graph = defaultdict(list)
    indegree = [0] * num_courses

    for course, prereq in prerequisites:
        graph[prereq].append(course)
        indegree[course] += 1

    q = deque([i for i in range(num_courses) if indegree[i] == 0])
    order = []

    while q:
        node = q.popleft()
        order.append(node)

        for neighbor in graph[node]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                q.append(neighbor)

    return order if len(order) == num_courses else []


# Example:
# num_courses = 4
# prerequisites = [(1, 0), (2, 0), (3, 1), (3, 2)]
# print(topological_sort(num_courses, prerequisites))
# Possible output: [0, 1, 2, 3]

# Cycle example:
# num_courses = 2
# prerequisites = [(0, 1), (1, 0)]
# print(topological_sort(num_courses, prerequisites))
# Output: []
```
---

### 4. Behavioral / AI Interview Questions (Pihu / Charlie AI)
These were asked during the live AI/video interview:

- What are the key stages in deploying a machine learning model?
- How do a lakehouse, feature store, and data warehouse fit together in your ML architecture?
- How would you create a point-in-time correct training dataset in SQL for a churn model?
- How do you determine the right time to retrain your model? What does the orchestration process look like?
- How do you ensure training runs are reproducible across environments?
- What key metrics do you monitor after deployment, and how do you automate detection of issues?
- For an LLM use case, how do you decide between fine-tuning vs prompt engineering + tools?
- How would you design chunking, retrieval, grounding, and evaluation for a RAG assistant?
- Tell me about a time you built an agentic AI workflow (e.g., using LangGraph, supervisor + sub-agents).
- Describe a time you disagreed with a teammate’s technical choice and what the outcome was.
- How would you navigate team conflicts under tight deadlines while keeping the team motivated?

---

