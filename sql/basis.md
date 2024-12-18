# SELECT

## Recyclable and Low Fat Products

> https://leetcode.com/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT p.product_id 
FROM Products AS p
WHERE 
    p.low_fats = 'Y' AND p.recyclable = 'Y'
```

## Find Customer Referee

> https://leetcode.com/problems/find-customer-referee/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT name 
FROM customer 
WHERE 
    referee_id <> 2 OR referee_id IS NULL
```

## Big Countries

> https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT name, population, area
FROM world
WHERE 
    area >= 3000000 OR population >= 25000000
```
## Article Views I

> https://leetcode.com/problems/article-views-i/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE
    author_id = viewer_id
ORDER BY id
```

## Invalid Tweets

> https://leetcode.com/problems/invalid-tweets/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT tweet_id 
FROM Tweets
WHERE CHAR_LENGTH(content) > 15
```


# JOIN
- INNER JOIN/JOIN: Returns only the rows where there is a match in both tables based on the specified condition. It excludes rows that do not meet the condition in either table. JOIN is shorthand for INNER JOIN.
- LEFT/RIGHT JOIN: Retains all rows from the left or right table as the base and selects matching rows from the other table, filling unmatched rows with NULL.
- FULL JOIN: Retains all rows from both tables, combining matching rows and filling unmatched rows with NULL.
- CROSS JOIN: Returns the Cartesian product of both tables, creating all possible combinations of rows.

## Replace Employee ID With The Unique Identifier

> https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT EmployeeUNI.unique_id, Employees.name
FROM Employees
LEFT JOIN EmployeeUNI
ON Employees.id = EmployeeUNI.id
```

## Product Sales Analysis I

> https://leetcode.com/problems/product-sales-analysis-i/?envType=study-plan-v2&envId=sql-free-50

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

## Rising Temperature

> https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT w2.id
FROM Weather w1, Weather w2
WHERE datediff(w2.recordDate, w1.recordDate) = 1
AND w2.Temperature > w1.Temperature
```
or
```sql
SELECT w1.id FROM weather w1 
JOIN weather w2 
ON datediff(w1.recorddate, w2.recorddate) = 1
WHERE w1.temperature > w2.temperature
```
or
```sql
SELECT w1.id FROM Weather w1, Weather w2
WHERE w1.recordDate - interval 1 day = w2.recordDate
AND w1.Temperature > w2.Temperature
```

## Average Time of Process per Machine

> https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT a1.machine_id, ROUND(AVG(a2.timestamp - a1.timestamp), 3) 
AS processing_time 
FROM Activity AS a1 
JOIN Activity AS a2
ON a1.machine_id = a2.machine_id
AND a1.activity_type = 'start'
AND a2.activity_type = 'end'
GROUP BY machine_id
```
or
```sql
SELECT machine_id, ROUND(
    SUM(IF (activity_type = 'start', -timestamp, timestamp)) / SUM(activity_type = 'start'), 3
) 
AS processing_time 
FROM Activity a 
GROUP BY machine_id
```

## Employee Bonus

> https://leetcode.com/problems/employee-bonus/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT name, bonus FROM Employee 
LEFT JOIN Bonus
ON Employee.EmpId = Bonus.EmpId
WHERE bonus IS NULL OR bonus < 1000
```

## Students and Examinations

> https://leetcode.com/problems/students-and-examinations/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT 
    s.student_id, s.student_name, sub.subject_name, IFNULL(grouped.attended_exams, 0) AS attended_exams
FROM 
    Students s
CROSS JOIN 
    Subjects sub
LEFT JOIN (
    SELECT student_id, subject_name, COUNT(*) AS attended_exams
    FROM Examinations
    GROUP BY student_id, subject_name
) grouped 
ON s.student_id = grouped.student_id 
AND sub.subject_name = grouped.subject_name
ORDER BY s.student_id, sub.subject_name;
```

## Managers with at Least 5 Direct Reports

> https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT Name FROM (
    SELECT Manager.Name AS Name, count(Report.Id) AS cnt
    FROM Employee AS Manager 
    INNER JOIN Employee AS Report
    ON Manager.Id = Report.ManagerId
    GROUP BY Manager.Id
) AS ReportCount
WHERE cnt >= 5
```
or
```sql
SELECT Manager.Name AS Name
FROM Employee AS Manager
JOIN Employee AS Report
ON Manager.Id = Report.ManagerId
GROUP BY Manager.Id
HAVING count(Report.Id) >= 5
```
or
```sql
SELECT Employee.Name AS Name
FROM (
    SELECT ManagerId as Id
    From Employee
    GROUP BY ManagerId
    having count(Id) >= 5
) AS Manager 
JOIN Employee
ON Manager.Id = Employee.Id
```

## Confirmation Rate

> https://leetcode.com/problems/confirmation-rate/description/?envType=study-plan-v2&envId=sql-free-50

```sql
SELECT s.user_id,ROUND(
    IFNULL(AVG(c.action = 'confirmed'), 0), 2
) AS confirmation_rate
FROM Signups AS s
LEFT JOIN Confirmations AS c
ON s.user_id = c.user_id
GROUP BY s.user_id
```