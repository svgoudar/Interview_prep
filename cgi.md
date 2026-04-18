Below are all the **questions visible across the images in this conversation**, consolidated clearly:

---

### 1. ML Fundamentals

* **Explain the key distinctions between:**

  * Supervised learning
  * Unsupervised learning
  * Reinforcement learning
* **When would you choose one over the others?**

---

### 2. Overfitting & Generalization

* When a model performs well on training data but poorly on unseen data:

  * How do you diagnose this issue?
  * What strategies do you use to fix it?
* How do you balance **model complexity vs generalization**?

---

### 3. Ensemble Methods

* When would you choose:

  * Random Forest
  * Gradient Boosting
    over a single model?
* What are the **trade-offs**, especially:

  * Computational complexity
  * Performance
  * Interpretability

---

### 4. Hands-on Coding (Neural Network)

* Implement a **basic neural network class in Python**

  * Include at least **one hidden layer**
  * Implement **forward propagation**
  * Use an activation function like:

    * Sigmoid or ReLU

---

### 5. Real-world ML System Design

* Describe a **real-world ML project** where:

  * You had to choose between multiple algorithms
* How did you:

  * Evaluate trade-offs?
  * Decide the final model?

---

### 6. GenAI Security & Privacy

* What are the **security and data privacy risks** in Generative AI systems?
* How do you **mitigate these risks** in production?

---

### 7. (Implicit Coding Evaluation)

From the coding screen/tests:

* Ensure correct:

  * Matrix dimensions
  * Forward propagation logic
  * Activation usage
  * Output shape

---

If you want, I can next:

* Convert all of these into **FAANG-level follow-up questions**
* Provide **model answers for each in 30-sec / 3-min format**
* Create a **mock interview flow with cross-questioning**



```py
import numpy as np

class SimpleNeuralNetwork:
    def __init__(self, input_size, hidden_size, output_size):
        self.W1 = np.random.randn(input_size, hidden_size)
        self.b1 = np.zeros((1, hidden_size))
        
        self.W2 = np.random.randn(hidden_size, output_size)
        self.b2 = np.zeros((1, output_size))

    def sigmoid(self, x):
        return 1 / (1 + np.exp(-x))

    def forward(self, x):
        z1 = np.dot(x, self.W1) + self.b1
        a1 = self.sigmoid(z1)

        z2 = np.dot(a1, self.W2) + self.b2
        a2 = self.sigmoid(z2)

        return a2


# Test
if __name__ == "__main__":
    nn = SimpleNeuralNetwork(3, 4, 2)
    test_input = np.array([[1, 2, 3]])
    output = nn.forward(test_input)
    print(output)
```
