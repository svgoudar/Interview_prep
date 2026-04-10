# 🧾 Extracted Questions

## 1. DSA – Graph (Hard)

**Shortest Path in Graph (Dijkstra’s Algorithm)**

**Problem Statement:**

* Given a directed graph represented as an adjacency list with weighted edges
* Find the shortest path from a starting node to all other nodes
* Graph constraints:

  * At least 2 nodes
  * Edge weights are positive integers (≤ 10⁶)
* If a node is unreachable → return `-1` for that node

---

## 2. SQL – Database Query

**Sales Performance By Region And Product**

**Problem Statement:**

* Given tables:

  * `sales`
  * `products`
  * `categories`
  * `regions`

**Task:**

* Calculate **total quantity sold for each product category in each region**

**Output:**

* `region_name`
* `category_name`
* `total_quantity_sold`

**Constraints:**

* Must handle cases where:

  * No sales exist for a region/category
  * Those rows should still appear in output

**Sorting:**

* Order by:

  * `region_name`
  * then `category_name`

---

## 3. DSA – Graph (Medium)

**Course Schedule / Topological Sort**

**Problem Statement:**

* Given:

  * `n` courses (0 to n-1)
  * List of prerequisite pairs `[a, b]`
* `b` must be completed before `a`

**Task:**

* Determine a valid order to complete all courses

**Output:**

* Valid order of courses
* If cycle exists → return **empty list/string**

**Constraints:**

* `1 ≤ courses ≤ 10,000`

---

## 4. DSA – Tree (Medium)

**Level Order Traversal of Binary Tree (Graph Representation)**

**Problem Statement:**

* Binary tree represented as:

  * Graph / adjacency list
  * Each node has at most 2 children

**Task:**

* Perform **level order traversal**
* Return values **level by level (left to right)**

**Input Details:**

* `n` nodes
* Each line contains:

  * `node, left_child, right_child`
* `-1` indicates no child

**Output:**

* List of lists:

  * Each inner list = nodes at that level

**Edge Case:**

* If tree is empty → return empty list

---

### 📊 Summary (All Questions)

| # | Category | Difficulty | Topic                    |
| - | -------- | ---------- | ------------------------ |
| 1 | DSA      | Hard       | Dijkstra (Shortest Path) |
| 2 | SQL      | Medium     | Aggregation + Joins      |
| 3 | DSA      | Medium     | Topological Sort         |
| 4 | DSA      | Medium     | Level Order Traversal    |


## 1) Shortest Path in Graph — Dijkstra’s Algorithm

```python id="h5k1pq"
import heapq
import sys

def solve():
    input = sys.stdin.readline
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))

    s = int(input().strip())

    INF = float('inf')
    dist = [INF] * n
    dist[s] = 0

    pq = [(0, s)]  # (distance, node)

    while pq:
        curr_dist, node = heapq.heappop(pq)

        if curr_dist > dist[node]:
            continue

        for nei, w in graph[node]:
            new_dist = curr_dist + w
            if new_dist < dist[nei]:
                dist[nei] = new_dist
                heapq.heappush(pq, (new_dist, nei))

    for i in range(n):
        print(-1 if dist[i] == INF else dist[i])

if __name__ == "__main__":
    solve()
```

**Time complexity:** `O((V + E) log V)`

---

## 2) SQL — Total quantity sold for each category in each region

```sql id="u9t4mw"
SELECT
    r.region_name,
    c.category_name,
    COALESCE(SUM(s.quantity_sold), 0) AS total_quantity_sold
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

**Why this works**

* `CROSS JOIN` creates every `region × category` combination
* `LEFT JOIN` keeps rows even where no sales exist
* `COALESCE(..., 0)` returns `0` instead of `NULL`

---

## 3) Course Schedule / Topological Sort

```python id="m2x8zc"
from collections import deque
import sys

def solve():
    input = sys.stdin.readline
    n, p = map(int, input().split())

    graph = [[] for _ in range(n)]
    indegree = [0] * n

    for _ in range(p):
        a, b = map(int, input().split())
        graph[b].append(a)   # b -> a
        indegree[a] += 1

    q = deque()
    for i in range(n):
        if indegree[i] == 0:
            q.append(i)

    order = []

    while q:
        node = q.popleft()
        order.append(node)

        for nei in graph[node]:
            indegree[nei] -= 1
            if indegree[nei] == 0:
                q.append(nei)

    if len(order) != n:
        print("")
    else:
        print(*order)

if __name__ == "__main__":
    solve()
```

**Time complexity:** `O(V + E)`

---

## 4) Level Order Traversal of Binary Tree

The image shows input like:

* `n`
* then `n` lines containing `node left_child right_child`
* `-1` means no child

A robust way is:

* store children
* find root as the node that never appears as a child
* do BFS

```python id="r7n3vk"
from collections import deque
import sys

def solve():
    input = sys.stdin.readline
    n = int(input().strip())

    if n == 0:
        print([])
        return

    children = {}
    all_nodes = set()
    child_nodes = set()

    for _ in range(n):
        node, left, right = map(int, input().split())
        children[node] = (left, right)
        all_nodes.add(node)

        if left != -1:
            child_nodes.add(left)
            all_nodes.add(left)
        if right != -1:
            child_nodes.add(right)
            all_nodes.add(right)

    roots = list(all_nodes - child_nodes)
    if not roots:
        print([])
        return

    root = roots[0]

    result = []
    q = deque([root])

    while q:
        size = len(q)
        level = []

        for _ in range(size):
            node = q.popleft()
            level.append(node)

            left, right = children.get(node, (-1, -1))
            if left != -1:
                q.append(left)
            if right != -1:
                q.append(right)

        result.append(level)

    print(result)

if __name__ == "__main__":
    solve()
```

**Time complexity:** `O(n)`

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

