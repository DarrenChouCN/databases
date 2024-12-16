## Recyclable and Low Fat Products

> https://leetcode.com/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=sql-free-50

---
- **Step 1**: [Describe the ...]
---

```sql
SELECT p.product_id 
FROM Products AS p
WHERE 
    p.low_fats = 'Y' AND p.recyclable = 'Y'
```

## Find Customer Referee

> https://leetcode.com/problems/find-customer-referee/description/?envType=study-plan-v2&envId=sql-free-50

---
```sql
SELECT name 
FROM customer 
WHERE 
    referee_id <> 2 OR referee_id IS NULL
```


## Big Countries

> https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=sql-free-50

---
```sql
SELECT name, population, area
FROM world
WHERE 
    area >= 3000000 OR population >= 25000000
```


## Article Views I

> https://leetcode.com/problems/article-views-i/description/?envType=study-plan-v2&envId=sql-free-50

---
```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE
    author_id = viewer_id
ORDER BY id
```


## Invalid Tweets

> https://leetcode.com/problems/invalid-tweets/description/?envType=study-plan-v2&envId=sql-free-50

---
```sql
SELECT tweet_id 
FROM Tweets
WHERE CHAR_LENGTH(content) > 15
```


## Replace Employee ID With The Unique Identifier

> https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=sql-free-50

---
```sql
SELECT EmployeeUNI.unique_id, Employees.name
FROM Employees
LEFT JOIN EmployeeUNI
ON Employees.id = EmployeeUNI.id
```


## Product Sales Analysis I

> https://leetcode.com/problems/product-sales-analysis-i/?envType=study-plan-v2&envId=sql-free-50

---
```sql
SELECT product_name, year, price
FROM Product p, Sales s
WHERE p.product_id = s.product_id
```


## Customer Who Visited but Did Not Make Any Transactions

> https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT customer_id, count(customer_id) AS count_no_trans
FROM Visits
WHERE visit_id NOT IN(
    SELECT visit_id FROM Transactions
)
group by customer_id
```
or
```sql
SELECT customer_id, count(customer_id) AS count_no_trans
FROM visits
LEFT JOIN transactions USING (visit_id)
WHERE transaction_id is NULL
GROUP BY customer_id
```