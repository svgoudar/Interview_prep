All questions extracted from the images:

---

### **Coding Questions**

1. **Item Shop**

   * You enter a shop where there are *n* items on sale. You are given an array *A* of length *n* where *A[i]* represents the cost of the *i-th* item.
   * You have *k* amount of currency.
   * The shop has limited stock and you can only buy:

     * First item *n* times
     * Second item *n-1* times
     * Third item *n-2* times, and so on
   * **Question:** Find the maximum number of items you can buy.

---

2. **Factorial-Sum Special Numbers**

   * Given an array of positive integers.
   * For each integer:

     * Compute sum of digits until it becomes a single digit (final sum).
     * Compute factorial of the final sum.
   * A number is “special” if all its digits are present in its factorial-sum result.
   * **Question:** Find the total number of special numbers in the array.

---

3. **Array Frequency Analysis**

   * Given an array of integers.
   * **Question:** Find the most frequently occurring value.
   * If multiple values have the same frequency, return the smallest one.

---

### **Python MCQ Questions**

4. **Nested Conditions Output**

   ```python
   x = 5
   y = 3
   z = 7

   if x > y:
       if y > z:
           print("x is greater than y and y is greater than z")
       else:
           print("y is not greater than z")
   else:
       print("x is not greater than y")
   ```

   * **Question:** What will be the output?

---

5. **Python Output Prediction**

   ```python
   for i in range(5):
       if i == 2:
           continue
       print(i, end=" ")
   ```

   * **Question:** What will be the output?

---

6. **String Reversal Analysis**

   ```python
   def reverse_words(string):
       words = string.split()
       reversed_words = [word[::-1] for word in words]
       return ' '.join(reversed_words)
   ```

   * **Question:** Which statement about this function is correct?

---

### **SQL Questions**

7. **MySQL Query and Grouping**

   * Display:

     * `STUD_ID`, `STUD_NAME`, and number of courses enrolled
   * Tables:

     * STUDENT(STUD_ID, STUD_NAME)
     * COURSES(COURSE_NO, ...)
   * **Question:** Write a query grouped by `STUD_ID` and `STUD_NAME`.

---

8. **MySQL Filtering Query**

   * Display:

     * `ITEM_NAME`, `ITEM_PRICE`, `MAN_COM_CODE`
   * Conditions:

     * `ITEM_PRICE` not equal to 1000 and 5000
     * `MAN_COM_CODE` between 30 and 100
   * **Question:** Write the SQL query.

---

### **Aptitude Questions**

9. **Man Goes to Row**

   * A man rows:

     * 40 km upstream and 60 km downstream in 13 hours
     * Same distance downstream in 16 hours (partial info visible)
   * **Question:** What is the rate of the current?

---

10. **Mixing Lentils**

* Lentil A: Rs. 20.40/kg
* Lentil B: Rs. 18.60/kg
* Mixture: Rs. 19.40/kg
* **Question:** In what ratio should A and B be mixed?

---

11. **Incomplete Number Series**

* Series: `3, 9, 45, X, 2835, ...`
* **Question:** Find X.

---

12. **Percentages and Comparisons**

* Katy’s height is 20% less than Carol’s.
* **Question:** Katy’s height is what percent of Carol’s?

---

If needed, solutions or explanations can be provided for each.
