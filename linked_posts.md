1. Can you design a system end-to-end? from ingestion to serving? what are the bottlenecks?
2. How would you estimate costs? how to reduce it?
3. How would you reduce latency? what is a good tradeoff of latency vs quality? 
4. Do you really need self-hosted LLMs? When is it needed? 
5. How would you fine-tune language models on user behavior? which framework? what about model serving?
6. How would you construct the dataset, what about loss function? what about MLOps? 
7. Which database would you use and why? Vector DB? SQL? NoSQL?
8. What metrics would you track? How? 
9. What about system monitoring? How would you debug failure cases?
10. What about feedback loop? how would we track and evaluate?
11. How would you make the system more deterministic?
12. How would you replace embedding models and backfill embeddings without any downtime?
13. What are the fallback mechanisms?

My favourite:
1️⃣ How would you solve this problem without LLMs or vector DBs?
2️⃣ Can you solve the problem with classical IR, rules, heuristics?
3️⃣ How would you make the system more deterministic?
4️⃣ Can you explain tokenization and embeddings from scratch?
5️⃣ What happens during fine-tuning? optimizer, learning rate, layer freezing? - not just “let's use LoRA



I’ve attended a couple of interviews recently for GenAI / AI-ML roles at large tech organizations, and I wanted to share an honest observation.

There seems to be a growing demand for infrastructure engineers who can deploy, scale, and productionize ML/GenAI systems — and now increasingly, build Agentic AI systems and multi-agent ecosystems.

These roles expect strong skills in:

Cloud & system design
APIs & microservices
ML/GenAI integration
Multi-agent orchestration

However, the interviews are often conducted by data scientists or ML engineers, and the focus shifts heavily toward:

Deep mathematical explanations behind ML algorithms
Detailed derivations of evaluation metrics
Advanced feature engineering techniques

While these are important, it raises a question:
👉 Are we evaluating the right skills for the role?

If the expectation is to build production-grade AI and agentic systems, then:

Deployment strategies
Scalability
Latency optimization
Multi-agent coordination
System reliability
should be equally emphasized.

Today, with platforms like managed ML services, much of the heavy lifting around model training is abstracted. The real challenge lies in:
Orchestrating multiple agents
Managing context and memory
Ensuring reliable and scalable AI systems in production
At times, it feels like:

Interviews have become exams without a clear syllabus or blueprint.
Candidates are expected to be:

Strong in infrastructure
Strong in ML
Strong in GenAI
Strong in Agentic AI
And also deeply theoretical

Almost like being an LLM ourselves 🙂

1.what is runnablepassthrough, runnablesequence, runnablelambda?
2.expalin RAGA?
3. Homogeneous corpus?
4.explain Biencoder and cross encoder along with differences?
5.how to evaluate RAG when we don't have ground truths?
6. Difference btw TFIDF and BM25?
7. explain about HNSW indexing?
8. what all a standard prompt include?
9. components of RAG?
10. what are few shots?
11. python programming coding question
i/p : {a:1,b:{d :3,e:5}}  o/p:{a:1,b.d:3,b.e:5}


What is the difference between an array and a list in Python?
What is RAG (Retrieval-Augmented Generation) and where is it used?
What is RNN and how does it help in sequential data problems?
Why do we clean data before training a model? What happens if we train on uncleaned data?
What is cosine similarity and where is it commonly used?
What is a best-fit line in regression and how do we measure how well it fits the data?
What are Type I and Type II errors in machine learning/statistics?
What would be your role and responsibilities as an AI Engineer in a project?
What steps are involved in building a machine learning pipeline?
How do you evaluate a machine learning model?
What are common image preprocessing steps before training a computer vision model?
What is the difference between supervised and unsupervised learning?
How would you handle missing values in a dataset?
What are embeddings and why are they important in NLP/GenAI systems?


Here are some of the questions:
1. What are Python decorators?
2. Difference between list and tuple?
3. Explain the bias–variance tradeoff.
4. What are the steps in a typical data science project?
5. Precision, Recall, and F1-score — when to use which?
6. How do you handle outliers?
7. What is tokenization in NLP?
8. Why do we remove stop words? What happens if we don’t?
9. What is Generative AI? Give examples.
10. What are activation functions in neural networks?
11. Difference between correlation and causation? 
12. What is convolution in CNN?
13. Image preprocessing steps? 
14. Why does Random Forest reduce overfitting compared to Decision Trees?






You’re in a Netflix ML Engineer interview.
Interviewer: "Your LLM-powered recommendation explanations are accurate, but users say recommendations feel unpredictable and ‘random’ from day to day. Engagement drops. What’s happening?"

This is how you respond...

You:
"The model is reasoning too locally.
It’s overreacting to short-term signals."

Interviewer:
"But personalization should adapt, right?"

You:
"Yes — but adaptation without stability feels like randomness."

Interviewer:
"Explain that."

You:
"In production, the LLM sees:

 - Last watch
 - Recent searches
 - Session context

It weighs them heavily because they’re fresh.
Long-term preferences get diluted."

Interviewer:
"So yesterday’s action dominates today’s recommendations?"

You:
"Exactly.
A single anomaly gets treated like a preference shift."

Interviewer:
"Why didn’t offline tests catch this?"

You:
"Offline metrics reward responsiveness.
They don’t measure perceived consistency."

Interviewer:
"How does this show up for users?"

You:
"Users feel the system:

 - ‘Doesn’t know me anymore’
 - Overcorrects after one action
 - Feels unstable, even if accurate"

Interviewer:
"So what’s the fix?"

You:
"You introduce temporal balance:

 - Separate short-term intent from long-term taste
 - Cap how much one session can influence ranking
 - Add inertia so preferences don’t flip instantly"

Interviewer:
"Doesn’t that slow learning?"

You:
"Slightly.
But users trust systems that evolve smoothly, not abruptly."

Interviewer:
"So personalization is a UX problem?"

You:
"Always.
People don’t experience models.
They experience behavior over time."








🔹 What is a cost function? Why do we minimize it?
🔹 What exactly is gradient descent doing behind the scenes?
🔹 What is the difference between a loss function and a cost function?
🔹 What is bias in machine learning?
🔹 What is multicollinearity? Why is it dangerous?
🔹 Difference between correlated variables and collinear variables?
🔹 What are the assumptions of Linear Regression?
🔹 Why does the sigmoid function convert values between 0 and 1?
🔹 How do you evaluate a regression model properly?






ost people want to work in GenAI, but very few know what the roadmap actually looks like — especially when it comes to real projects and interviews.

If you're preparing for GenAI roles, here’s what actually matters:

Step 1: Core ML Foundations
Understand how models learn, overfit, and generalize.
Be able to explain train-test split, cross-validation, regularization, and evaluation metrics clearly.
This is where most interviews filter candidates.

Step 2: Deep Learning Essentials
Know how to build and train neural networks.
Experience with PyTorch or TensorFlow is non-negotiable.
Focus on: activation functions, optimizers, gradient flow, batching, early stopping.

Step 3: Transformer and LLM Fundamentals
Understand attention, self-attention, encoder-decoder architectures, embeddings, positional encoding, tokenization.
Be able to explain how LLMs predict tokens and why context windows matter.

Step 4: Prompt Engineering and RAG
Learn structured prompting, system prompts, chain-of-thought patterns, and context window planning.
RAG: Chunking, embeddings, vector search (FAISS, Pinecone, Qdrant), retrieval strategies.

Step 5: Fine-tuning Large Models
When to choose: full fine-tuning, LoRA, QLoRA, PEFT.
Be able to explain tradeoffs of compute, quality, and domain adaptation.

Step 6: LLMOps (This is where real jobs exist)
How you deploy and maintain GenAI systems matters more than model building.
Understand:
• Model serving
• KV caching
• Quantization (FP16, INT8)
• Model gating and fallback behavior
• Monitoring hallucinations and drift
• Observability dashboards
• Cost-performance optimization in production

Step 7: End-to-End Project Skills
To stand out, show:
• Problem framing
• Dataset decisions
• Evaluation strategy
• Latency & cost considerations
• Clear explanation of tradeoffs
This is what hiring managers look for — your reasoning.

Interview Tip:
If your answer sounds like theory, you lose the interviewer.
If your answer sounds like a design decision made under constraints, you win.
