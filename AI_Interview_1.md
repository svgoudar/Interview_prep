All questions and options extracted **completely** from the images:

---

## **1. Homographs**

**Question:**
Which of the following methods could be used to disambiguate the two distinct meanings of the word “bank” in the sentences below?

* They made nightly deposits at the bank.
* They went fishing at the river bank.

**Options:**

* Use BERT-based embeddings to find the nearest neighbor in a sense-tagged corpus.
* Apply coreference resolution and linking.
* Run a syntactic parser, then retrieve the correct sense from WordNet via lemma and POS.
* Leverage word2vec or glove to triangulate the closest semantic representation given all other words in the sentence.

---

## **2. Regular Expressions**

**Question:**
Which regular expression will match any consecutive sequence of English letters?

**Options:**

* `[a-zA-Z]+`
* `[^a-zA-Z]`
* `[a-zA-Z\s]`
* `[a-z?A-Z]`
* `[a-z|A-Z]`

---

## **3. Select a Model**

**Question:**
For a binary classification problem, the cost of misclassifying a positive sample is three times more than the cost of misclassifying a negative example. Which of these models has the lowest cost, while achieving at least 60% recall?

**Table:**

| Model   | TP | FN | TN | FP |
| ------- | -- | -- | -- | -- |
| Model 1 | 9  | 6  | 25 | 10 |
| Model 2 | 5  | 10 | 20 | 15 |
| Model 3 | 1  | 14 | 30 | 5  |
| Model 4 | 10 | 5  | 20 | 15 |

**Options:**

* Model 1
* Model 2
* Model 3
* Model 4

---

## **4. Performance Plot**

**Question:**
Looking at this chart of training vs. validation loss for a classifier, can you say whether the classifier is overfitting?

**Options:**

* Classifier starts overfitting at point A
* Classifier starts overfitting at point B
* Classifier starts overfitting at point C
* Classifier doesn’t appear to be overfitting
* This graph contains insufficient information to determine if the classifier is overfitting

<img width="1365" height="624" alt="image" src="https://github.com/user-attachments/assets/fe1a5f54-3f9a-49f5-adf9-f657b3d26f4f" />

---

## **5. Medical Diagnostics**

**Question:**
A medical company is building a model to predict cancer.

* 900 negative samples
* 100 positive samples
* Model has 90% accuracy but poor recall

What steps can improve performance?

**Options:**

* Collect more data for the positive case
* Over-sample instances from the negative class
* Under-sample instances from the positive class
* Generate synthetic samples using something like SMOTE

---

## **6. High Dimensionality**

**Question:**
After modeling a dataset with 2,000 labels and 100K features, results are poor due to sparsity. How to mitigate before training again?

**Options:**

* Apply a factorization machine to the training data
* Apply PCA to the training data
* Drop all the correlated feature columns
* Concatenate the correlated features together

---

## **7. Regression (L1 Regularization)**

**Question:**
A regression model with L1 regularization underfits. What improves accuracy?

**Options:**

* Increase the coefficient of the L1 term
* Decrease the coefficient of the L1 term
* Try L2 rather than L1 regularization
* Try L0 rather than L1 regularization

---

## **8. Evaluation Plot (ROC)**

**Question:**
The plot below was generated during evaluation of a binary classifier. What can we say?

**Options:**

* The AUC is 1.0
* The model has no discrimination capability
* The model has a perfect measure of separability
* The model has perfect accuracy
* None of the above are correct

<img width="694" height="393" alt="image" src="https://github.com/user-attachments/assets/59b6eff7-b43a-4cd8-a4e4-3bd57cfcb7fe" />

---

## **9. Term Informativeness**

**Question:**
Which approximates the specificity of a query term in a search collection?

**Options:**

* Term frequency
* Inverse document frequency
* Term frequency-inverse document frequency
* Collection frequency

---

## **10. TF-IDF / Language Model**

**Question:**
Two texts same length and vocabulary:

* Text 1: real novel
* Text 2: random sampling

Which is TRUE?

**Options:**

* Perplexity of the language model will be higher on Text 1
* Perplexity of the language model will be higher on Text 2
* The number of unique ngrams is the same in Text 1 and Text 2 (for n > 2)

---

## **11. K-Means Plot**

**Question:**
Based on elbow method graph, optimal number of clusters?

**Options:**

* 2
* 8
* 4 or 5
* 6, 7 or 8
<img width="1147" height="627" alt="image" src="https://github.com/user-attachments/assets/d82bfc91-28fb-4511-b167-87c19f606f77" />


---

## **12. Bagging**

**Question:**
When is Bagging useful?

**Options:**

* When the base learner has high variance
* When the base learner is linear regression or k-NN for large k
* When the dataset contains a lot of outliers
* None of the above are correct

---

## **13. Store Location (K-Means)**

**Question:**
When will k-means produce suboptimal results?

**Options:**

* The target demographic is spread across both high- and low-density regions
* Shopping districts include waterfront and oddly shaped zones
* Planning for different store types (high and low traffic)
* Company has a precise formula for number of stores

---

## **14. Topic Models**

**Question:**
Which statements are generally TRUE?

**Options:**

* Perplexity is easy to compute and correlates well with human judgment
* Boilerplate repeated text can be ignored
* Preprocessing includes tokenization, stop word removal, normalization, collocations
* Topic modeling is a form of dimensionality reduction

---

## **15. Linear Regression (SGD Prep)**

**Question:**
Before applying linear regression with SGD, what steps are appropriate?

**Options:**

* Normalize features to mean 0 and std 1
* Add small random noise to training matrix
* Shuffle training instances
* Normalize dependent variable
* Normalize features to fixed range (e.g., 0–1)

---

## **16. QA Precision**

**Question:**
Precision@5 = 85%. What does it mean?

**Options:**

* Using 5-fold CV, answers correct 85% of time
* On average, 85% of top 5 answers are correct
* 85% chance at least one correct answer above rank 5
* None of the above

---

## **17. NDCG Metric**

**Question:**
NDCG discounts gain based on:

**Options:**

* Document length
* Query complexity
* Document popularity
* Position in the ranked result

---

## **18. MRR (Mean Reciprocal Rank)**

**Question:**
Which systems have MRR ≥ 0.5?

**Table (visible):**

* System 1 → ranks: 1, 2
* System 2 → ranks: 3, 2
* System 3 → ranks: 2, 2
* System 4 → ranks: 4, 3

**Options:**

* System 1
* System 2
* System 3
* System 4

---

## **19. Paper Claims**

**Question:**
Which claim is acceptable?

**Options:**

* Training error lower than all previous methods
* Test error lower, λ chosen on test set
* Test error lower, λ chosen via cross-validation
* Cross-validation error lower, λ chosen via cross-validation

---

## **20. Vector Search**

**Question:**
Choose all that apply:

**Options:**

* Normalizing embeddings will improve precision
* Adding more content will improve precision
* Adding more content may improve recall
* Applying random projection reduces dimensions but not precision/recall
* Increasing k will improve recall

---

If you want, I can next:

* Provide **all correct answers**
* Or give **quick reasoning shortcuts** for each (useful for interviews)
