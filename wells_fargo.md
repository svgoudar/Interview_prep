# Problems

- ## 1

### Extracted Content from Image

---

## **Frequent Request Detection**

Arrays `requestIDs` and `timestamps` are given, along with an integer `timeWindow`.

* `requestIDs[i]` represents the ID of a request
* `timestamps[i]` represents the time (in seconds) when that request occurred

---

### **A request ID is considered repeated within the window if:**

* It appears at least twice, **and**
* There exist two occurrences whose time difference is **≤ timeWindow**

---

### **Task**

Count how many **distinct request IDs** satisfy this condition.
Return the count.

---

## **Example**

```
n = 4
requestIDs = ["swiftreq001", "swiftreq002", "swiftreq001", "swiftreq002"]
timestamps = [10, 25, 20, 15]
timeWindow = 10
```

---

### **Explanation**

* `"swiftreq001"` appears at times **10 and 20**
  → 20 - 10 = 10 ≤ 10 → ✅ valid

* `"swiftreq002"` appears at times **15 and 25**
  → 25 - 15 = 10 ≤ 10 → ✅ valid

---

### **Output**

```
2
```

---

## **Constraints (visible part)**

* Total length does not exceed **2 × 10⁵**
* `1 ≤ timestamps[i], timeWindow ≤ 10⁹`
* `requestIDs[i]` contains only alphanumeric characters (`a-z`, `A-Z`, `0-9`)

---

## **Sample Input 0**

```
requestIDs = ["req1", "req2", "req1"]
timestamps = [45, 35, 40]
timeWindow = 5
```

---

### **Sample Output 0**

```
1
```

---

### **Explanation**

* `"req1"` → appears at 45 and 40
  → 45 - 40 = 5 ≤ 5 → ✅ valid

* `"req2"` → appears once → ❌ invalid


```

def countFrequentRequests(requestIDs, timestamps, timeWindow):
    from collections import defaultdict
    
    # Step 1: Group timestamps by request ID
    mp = defaultdict(list)
    for rid, ts in zip(requestIDs, timestamps):
        mp[rid].append(ts)
    
    count = 0
    
    # Step 2: Check each request ID
    for rid in mp:
        times = sorted(mp[rid])
        
        left = 0
        for right in range(len(times)):
            # Shrink window if condition breaks
            while times[right] - times[left] > timeWindow:
                left += 1
            
            # If at least 2 occurrences in window
            if right - left + 1 >= 2:
                count += 1
                break  # count this ID once
    
    return count

- ## 3

### **SQL: Tax Software's Quarterly Income Report**

For a tax software application, there is a requirement to produce a **quarterly income report** for all customer accounts for the fiscal year **2023**.

This report will:

* Estimate the examination of income distribution across different quarters
* Offer a cumulative perspective on total income for each customer over the year
* Detail income for each quarter (**Q1, Q2, Q3, Q4**) and total yearly income

---

### **Required Output Columns**

The result should include:

* **iban** → IBAN of the customer account
* **q1, q2, q3, q4** → total income for each quarter of 2023

  * Rounded to **2 decimal places** (include trailing zeros if needed)
* **year2023** → total income for entire year 2023

  * Rounded to **2 decimal places**

---

**Sorting**

* Sort results in **ascending order by iban**

---

**Note**

* Only **2023 declarations** should be included

---

### **Schema**

#### **accounts**

| Column | Type         | Constraint  | Description         |
| ------ | ------------ | ----------- | ------------------- |
| id     | INT          | PRIMARY KEY | Customer account ID |
| iban   | VARCHAR(255) |             | IBAN of customer    |

---

### **declarations**

| Column     | Type         | Constraint                | Description        |
| ---------- | ------------ | ------------------------- | ------------------ |
| account_id | INT          | FOREIGN KEY → accounts.id | Customer reference |
| dt         | VARCHAR(19)  |                           | Date of income     |
| amount     | DECIMAL(6,2) |                           | Income amount      |

---

### **Sample Data**

#### **accounts**

| id | iban                              |
| -- | --------------------------------- |
| 1  | CR23 9441 4417 7652 1967 9        |
| 2  | FR37 0461 7975 854Y RASL 3C9P K25 |
| 3  | AT68 0390 5615 5306 9949          |

---

### **declarations**

| account_id | dt                  | amount  |
| ---------- | ------------------- | ------- |
| 3          | 2022-12-31 04:25:45 | 3213.34 |
| 1          | 2023-01-13 06:06:11 | 3292.12 |
| 3          | 2023-02-12 01:19:35 | 1100.94 |
| 3          | 2023-03-29 09:57:50 | 3290.40 |
| 1          | 2023-04-03 11:19:02 | 2126.57 |
| 1          | 2023-04-11 12:14:01 | 2302.32 |
| 1          | 2023-04-18 18:35:40 | 2000.36 |
| 2          | 2023-04-22 01:47:20 | 1816.30 |
| 2          | 2023-04-29 17:53:24 | 1334.95 |
| 3          | 2023-05-15 17:28:24 | 4492.89 |
| 3          | 2023-05-25 22:46:18 | 2345.08 |
| 1          | 2023-05-28 06:01:06 | 2247.97 |
| 2          | 2023-08-25 19:56:44 | 4718.02 |
| 2          | 2023-08-26 15:42:56 | 3083.94 |
| 2          | 2023-09-29 00:38:26 | 4824.83 |
| 2          | 2023-10-18 14:25:41 | 2967.21 |
| 1          | 2023-11-14 03:06:27 | 2582.61 |
| 3          | 2023-12-12 08:46:18 | 1957.15 |
| 3          | 2023-12-28 13:20:34 | 3468.78 |
| 1          | 2024-01-31 23:35:08 | 2156.52 |

---

**Sample Output (partial)**

| iban    | q1      | q2      | q3       | q4      | year2023 |
| ------- | ------- | ------- | -------- | ------- | -------- |
| AT68... | 4391.34 | 6837.97 | 0.00     | 5425.93 | 16655.24 |
| CR23... | 3292.12 | 8677.22 | 0.00     | 2582.61 | 14551.95 |
| FR37... | 0.00    | 3151.25 | 12626.79 | 2967.21 | 18745.25 |



```sql
SELECT 
    a.iban,
    
    ROUND(SUM(CASE 
        WHEN QUARTER(STR_TO_DATE(d.dt, '%Y-%m-%d %H:%i:%s')) = 1 THEN d.amount 
        ELSE 0 END), 2) AS q1,
        
    ROUND(SUM(CASE 
        WHEN QUARTER(STR_TO_DATE(d.dt, '%Y-%m-%d %H:%i:%s')) = 2 THEN d.amount 
        ELSE 0 END), 2) AS q2,
        
    ROUND(SUM(CASE 
        WHEN QUARTER(STR_TO_DATE(d.dt, '%Y-%m-%d %H:%i:%s')) = 3 THEN d.amount 
        ELSE 0 END), 2) AS q3,
        
    ROUND(SUM(CASE 
        WHEN QUARTER(STR_TO_DATE(d.dt, '%Y-%m-%d %H:%i:%s')) = 4 THEN d.amount 
        ELSE 0 END), 2) AS q4,
    
    ROUND(SUM(d.amount), 2) AS year2023

FROM accounts a
JOIN declarations d 
    ON a.id = d.account_id

WHERE YEAR(STR_TO_DATE(d.dt, '%Y-%m-%d %H:%i:%s')) = 2023

GROUP BY a.iban
ORDER BY a.iban;
```

- ## 3


### **SQL: MMORPG Game Character Outfit Advisor**

An MMORPG (Massively Multiplayer Online Role-Playing Game) is under development.
For the profile and inventory mechanics, a query is needed to **advise the best quality items available in the inventory**.

---

### **Item Quality Ranking**

(from lowest → highest):

* `common`
* `rare`
* `epic`

---

### **Expected Output Columns**

* **username** → account username
* **type** → item type
* **advised_quality** → best available quality (`common`, `rare`, `epic`)
* **advised_name** → list of advised item names

### **advised_name rules**

* Each record is the **item name**
* Multiple items are:

  * **comma-separated**
  * **sorted in ascending order by name**
* If duplicates exist → **include only once**

---

### **Sorting Requirement**

* Sort by:

  1. **username ASC**
  2. **type ASC**

---

**Note**

* An account **might not have all item types**

---

### **Schema**

### **accounts**

| column   | type         | constraint  | description      |
| -------- | ------------ | ----------- | ---------------- |
| id       | INT          | PRIMARY KEY | Account ID       |
| username | VARCHAR(255) |             | Account username |

---

### **items**

| column | type                           | constraint  | description |
| ------ | ------------------------------ | ----------- | ----------- |
| id     | INT                            | PRIMARY KEY | Item ID     |
| type   | ENUM('sword','shield','armor') |             | Item type   |
| name   | VARCHAR(255)                   |             | Item name   |

---

### **accounts_items**

| column     | type                         | constraint                | description  |
| ---------- | ---------------------------- | ------------------------- | ------------ |
| account_id | INT                          | FOREIGN KEY → accounts.id | Account ID   |
| item_id    | INT                          | FOREIGN KEY → items.id    | Item ID      |
| quality    | ENUM('common','rare','epic') |                           | Item quality |

---

## **Sample Data**

### **accounts**

| id | username    |
| -- | ----------- |
| 1  | sdavidovic0 |
| 2  | rcarleton1  |

---

### **items**

| id | type   | name                     |
| -- | ------ | ------------------------ |
| 1  | shield | Shield of Fagaceae       |
| 2  | sword  | Sword of Loasaceae       |
| 3  | sword  | Sword of Arthoniaceae    |
| 4  | sword  | Sword of Hydrophyllaceae |
| 5  | sword  | Sword of Solanaceae      |
| 6  | sword  | Sword of Cactaceae       |
| 7  | armor  | Armor of Ericaceae       |
| 8  | shield | Shield of Rosaceae       |
| 9  | sword  | Sword of Fagaceae        |
| 10 | armor  | Armor of Asteraceae      |

---

### **accounts_items**

| account_id | item_id | quality |
| ---------- | ------- | ------- |
| 1          | 10      | epic    |
| 1          | 2       | rare    |
| 1          | 2       | rare    |
| 1          | 7       | rare    |
| 1          | 1       | common  |
| 1          | 1       | common  |
| 1          | 2       | common  |
| 1          | 3       | common  |
| 1          | 3       | common  |
| 1          | 5       | common  |
| 1          | 8       | common  |
| 1          | 8       | common  |
| 1          | 9       | common  |
| 2          | 8       | epic    |
| 2          | 5       | rare    |
| 2          | 3       | common  |
| 2          | 3       | common  |

---

If needed next:

* MySQL query solution (optimized with GROUP_CONCAT + ranking logic)
* Window function approach
* Performance-optimized version for large datasets


```
WITH ranked_items AS (
    SELECT
        a.id AS account_id,
        a.username,
        i.type,
        i.name,
        ai.quality,
        CASE ai.quality
            WHEN 'common' THEN 1
            WHEN 'rare' THEN 2
            WHEN 'epic' THEN 3
        END AS quality_rank
    FROM accounts a
    JOIN accounts_items ai
        ON a.id = ai.account_id
    JOIN items i
        ON i.id = ai.item_id
),
best_quality AS (
    SELECT
        account_id,
        username,
        type,
        MAX(quality_rank) AS best_rank
    FROM ranked_items
    GROUP BY account_id, username, type
)
SELECT
    b.username,
    b.type,
    CASE b.best_rank
        WHEN 1 THEN 'common'
        WHEN 2 THEN 'rare'
        WHEN 3 THEN 'epic'
    END AS advised_quality,
    GROUP_CONCAT(DISTINCT r.name ORDER BY r.name SEPARATOR ',') AS advised_name
FROM best_quality b
JOIN ranked_items r
    ON r.account_id = b.account_id
   AND r.type = b.type
   AND r.quality_rank = b.best_rank
GROUP BY
    b.account_id,
    b.username,
    b.type,
    b.best_rank
ORDER BY
    b.username,
    b.type;
```


### **1. Wrangling Clickstream Data with RDDs**

### **Extracted Question**

A Spark application analyzes clickstream data:

* Input: text file → `user_id, timestamp, product_id, category`
* Goal: **Top 5 product categories by revenue (clicks × avg price)**
* Challenges:

  1. Large + skewed data
  2. Streaming updates
  3. Dynamic date filtering

### **Options**

1. Single RDD + static aggregation
2. Partition by category
3. Use DStreams + join with cached historical data + dynamic aggregation
4. Combine RDD + DataFrames + SQL

### **Correct Answer**

**Option 3**

> Utilize DStreams to continuously ingest new click events, join with cached historical data, and perform dynamic aggregations.

---

### **2. Spark Interactive Shell Commands**

### **Extracted Question**

Dataset: `userActivityDF`

```
userId: string
activityDate: date
pageVisited: string
durationSeconds: integer
```

Goal:

* Filter users who visited `"homepage"` on `"2023-01-01"`
* Compute **total duration per user**
* No imports allowed

### **Correct Answer**

```
val filteredDF = userActivityDF.filter($"pageVisited" === "homepage" && $"activityDate" === "2023-01-01")
val resultDF = filteredDF.groupBy($"userId").agg(sum("durationSeconds"))
```

**Option 4**

---

### **3. Optimizing Data Aggregation**

### **Extracted Question**

Goal:

* Rolling count of distinct users
* Fixed-size time window
* Minimize shuffle + overhead

### **Options Summary**

1. Window + custom distinct + partition by userId
2. GroupBy timestamp + countDistinct + repartition
3. Sliding window + approx_count_distinct + partition by category
4. GroupBy timestamp + collect_set + repartition

### **Correct Answer**

**Option 1**

> Window function + distinct counting + partition by userId (best for minimizing shuffle)

---

### **4. Configuring Apache Arrow**


Goal:

* Optimize Spark with Arrow
* Improve in-memory columnar processing

### **Options Summary**

1. Increase driver memory + batch size + shared memory
2. Executor tuning + GPU + fallback
3. Metadata caching + compression + direct memory
4. Plasma store + buffer tuning + disable Arrow

### **Correct Answer**

**Option 3**

> Enable Arrow direct memory + optimize caching + compression

---




