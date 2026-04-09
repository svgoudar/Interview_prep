
### Question 2:  
**What is the primary benefit of using database connection pooling?**

**Options:**
- It converts SQL to NoSQL queries
- It automatically backs up the database
- It reduces overhead by reusing existing connections
- It encrypts database connections

### Question 3:  
**In a microservices architecture, what is a circuit breaker pattern used for?**

**Options:**
- Encrypting data between services
- Routing requests to the correct service
- Preventing cascade failures by stopping calls to failing services
- Compressing data to reduce bandwidth

### Question 6:  
**Which is the correct way to handle environment-specific configuration in production?**

**Options:**
- Create separate Git branches for each environment
- Store configuration in comments within the code
- Use environment variables or external configuration services
- Hardcode values for each environment in the source code

### Question 7:  
**An LLM-based assistant is giving different answers to the same question asked minutes apart. What is the MOST likely cause?**

**Options:**
- The API is rate-limiting requests
- The user's IP address changed
- The temperature parameter is set above 0
- The model's weights are being updated in real-time

### Question 8:  
**When would you choose a traditional ML classification model over an LLM?**

**Options:**
- When you have no training data available
- When you need to classify text into predefined categories with high accuracy and low cost at scale
- When the task requires understanding context across long documents
- When you need to generate creative content


Here’s the complete list of **all questions** from the images you’ve shared so far (including the new ones), with their options clearly listed:

### Question 1 (HTTP Method):
**Which HTTP method should be used to update a specific field of an existing resource without replacing the entire resource?**

**Options:**
- PATCH
- UPDATE
- PUT
- POST

### Question 1 (LLM - Hallucination):
**What is "hallucination" in the context of LLMs?**

**Options:**
- When the model refuses to answer a question
- When the model takes too long to respond
- When the model generates plausible-sounding but factually incorrect information
- When the model repeats the same response multiple times

### Question 4:
**What is the main advantage of using async/await in Python or JavaScript?**

**Options:**
- It allows non-blocking I/O operations without callbacks or threads
- It automatically handles all error conditions
- It makes code run faster by using multiple CPU cores
- It reduces memory usage by half

### Question 5 (Python):
**In Python, what is the output of:**

```python
def process(items):
    items.append(1)
    return items

process([])
process([])
```

**Options:**
- [] and []
- [1] and [1]
- [1] and [1, 1]
- Error: list index out of range

### Question 7 (Idempotency):
**What does idempotency mean in the context of API design?**

**Options:**
- The API responds in under 100ms
- Multiple identical requests have the same effect as a single request
- The API can handle requests from multiple clients
- The API returns data in JSON format

### Question 8 (REST API Status Code):
**What is the primary purpose of a REST API's HTTP status code 201?**

**Options:**
- The requested resource was not found
- The request was successful and no content is returned
- The request requires authentication
- The request was successful and a new resource was created



### Question 1 (Design Flaw in Production):
**You discover a significant design flaw in a system you built that's already in production. Fixing it would require substantial rework. What do you do?**

**Options:**
- Raise it transparently with stakeholders, explain the impact, and propose a remediation plan
- Document it but deprioritize it indefinitely
- Wait until someone else notices the problem
- Fix it quietly without telling anyone

### Question 2 (Custom Solution vs Third-Party):
**Your team is deciding between building a custom solution vs. using a third-party service. What is the MOST important factor to consider first?**

**Options:**
- Whether the capability is core to your differentiation and needs to be owned
- Which option is more technically interesting
- Which option your team already has experience with
- Which option has more online tutorials available

### Question 2 (Chatbot for Company Policies):
**You're building a chatbot that needs to answer questions about company policies. The policies update weekly. What is the BEST approach?**

**Options:**
- Answer Choice: Use a larger context window
- Fine-tune the LLM weekly with new policy documents
- Use RAG with an updated document index
- Increase the model's temperature setting

### Question 3 (Prompt Injection):
**What is "prompt injection" in the context of LLM applications?**

**Options:**
- Adding more examples to improve model performance
- The process of converting prompts to embeddings
- A technique to speed up model inference
- A security vulnerability where malicious input manipulates the model's behavior

### Question 3 (Junior Engineer Code Quality):
**A junior engineer has implemented a feature that works but has significant code quality issues. The team is under deadline pressure. What is the BEST approach?**

**Options:**
- Rewrite the code yourself without involving the junior engineer
- Accept the code as-is to meet the deadline
- Reject the code and have them rewrite it completely
- Merge with a plan to refactor, and use this as a coaching opportunity afterward

### Question 4 (Context Window):
**What is the "context window" of a Large Language Model?**

**Options:**
- The maximum amount of text (tokens) the model can process in one request
- The browser window used to access the model
- The time window during which the model remembers previous conversations
- The user interface where prompts are entered

### Question 4 (Stakeholder "Quick Fix"):
**A stakeholder requests a "quick fix" that would create technical debt. How do you handle it?**

**Options:**
- Refuse to implement it
- Implement it exactly as requested without comment
- Explain the trade-offs clearly and let the stakeholder make an informed decision
- Implement a perfect solution even if it takes longer than requested

### Question 5 (Embeddings):
**What is the primary purpose of embeddings in AI applications?**

**Options:**
- To compress images for storage
- To represent text/data as numerical vectors for similarity comparison
- To encrypt sensitive user data
- To convert speech to text

### Question 6 (RAG):
**What is RAG (Retrieval Augmented Generation)?**

**Options:**
- A way to reduce the cost of API calls
- Retrieving relevant documents to provide context before generating a response
- A technique to make LLMs respond faster
- A method to fine-tune LLMs on custom data

### Question 7 (LLM Different Answers):
**An LLM-based assistant is giving different answers to the same question asked minutes apart. What is the MOST likely cause?**

**Options:**
- The API is rate-limiting requests
- The user's IP address changed
- The temperature parameter is set above 0
- The model's weights are being updated in real-time



Here’s a clear list of the questions visible in the images you shared from the **Python Application Developer (LLM)** interview on FabricQA:

### Question 7 of 9: Batching Events for LLM
**Problem Statement:**

Batch events into lists so each fits `max_tokens` and `max_bytes`. Oversized events go in their own batch and separate list. Preserve order; use greedy batching. Use notepad for your solution.

**Scenario (KPI Partners context):**  
KPI Partners builds GenAI copilots that summarize enterprise analytics events (BI usage, pipeline runs, data-quality alerts). Before sending events to an LLM, the application must batch events into request payloads that fit strict limits.

**Task:**  
Write a Python function that takes a list of event dictionaries and returns a list of batches (each batch is a list of events) such that:

**Constraints & Rules:**
1. Each batch respects two limits:
   - `max_tokens`: maximum total token estimate per batch
   - `max_bytes`: maximum total UTF-8 payload size per batch
2. Token estimate per event is provided as `event["tokens"]` (integer)
3. Byte size per event should be computed as `len(json.dumps(event, separators=(",", ":"), sort_keys=True).encode('utf-8'))`
4. Preserve input order within and across batches (greedy strategy)
5. Add events to the current batch until adding the next would exceed either limit; then start a new batch
6. If a single event exceeds either limit by itself, it must be placed in its own batch and also returned in a separate list of `oversized_events`

**Function Signature:**
```python
from typing import List, Dict, Tuple

def batch_events_for_llm(
    events: List[Dict],
    max_tokens: int,
    max_bytes: int
) -> Tuple[List[List[Dict]], List[Dict]]:
    ...
    # Return (batches, oversized_events)
```
```
import json
from typing import List, Dict, Tuple

def batch_events_for_llm(
    events: List[Dict],
    max_tokens: int,
    max_bytes: int
) -> Tuple[List[List[Dict]], List[Dict]]:
    
    batches = []
    current_batch = []
    current_tokens = 0
    current_bytes = 0
    oversized_events = []

    for event in events:
        tokens = event.get("tokens", 0)
        byte_size = len(json.dumps(event, separators=(",", ":"), sort_keys=True).encode("utf-8"))

        # Check if event itself is oversized
        if tokens > max_tokens or byte_size > max_bytes:
            oversized_events.append(event)
            batches.append([event])  # its own batch
            continue

        # Check if adding to current batch exceeds limits
        if (current_tokens + tokens > max_tokens) or (current_bytes + byte_size > max_bytes):
            if current_batch:
                batches.append(current_batch)
            current_batch = [event]
            current_tokens = tokens
            current_bytes = byte_size
        else:
            current_batch.append(event)
            current_tokens += tokens
            current_bytes += byte_size

    # Add last batch if not empty
    if current_batch:
        batches.append(current_batch)

    return batches, oversized_events
```

**Example & Notes:**
- Greedy batching should preserve order
- Aim for linear time
- Do not reorder events
- You may precompute sizes/tokens per event

---

### Question 8:
**You're developing a GenAI app using a large language model to answer customer queries based on company docs. Responses are often slow and inconsistent. How would you optimize this, and what key factors would you consider?**

(This is an open-ended / discussion question — no multiple-choice options.)

---
