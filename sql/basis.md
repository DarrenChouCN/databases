#### [Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=sql-free-50)

```sql
SELECT p.product_id 
FROM Products AS p
WHERE 
    p.low_fats = 'Y' AND p.recyclable = 'Y'
```

#### [Find Customer Referee](https://leetcode.com/problems/find-customer-referee/description/?envType=study-plan-v2&envId=sql-free-50)

```sql
SELECT name 
FROM customer 
WHERE 
    referee_id <> 2 OR referee_id IS NULL
```

#### [Big Countries](https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=sql-free-50)

```sql
SELECT name, population, area
FROM world
WHERE 
    area >= 3000000 OR population >= 25000000
```
#### [Article Views I](https://leetcode.com/problems/article-views-i/description/?envType=study-plan-v2&envId=sql-free-50)

```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE
    author_id = viewer_id
ORDER BY id
```

#### [Invalid Tweets](https://leetcode.com/problems/invalid-tweets/description/?envType=study-plan-v2&envId=sql-free-50)

```sql
SELECT tweet_id 
FROM Tweets
WHERE CHAR_LENGTH(content) > 15
```


### JOIN
- INNER JOIN/JOIN: Returns only the rows where there is a match in both tables based on the specified condition. It excludes rows that do not meet the condition in either table. JOIN is shorthand for INNER JOIN.
- LEFT/RIGHT JOIN: Retains all rows from the left or right table as the base and selects matching rows from the other table, filling unmatched rows with NULL.
- FULL JOIN: Retains all rows from both tables, combining matching rows and filling unmatched rows with NULL.
- CROSS JOIN: Returns the Cartesian product of both tables, creating all possible combinations of rows.

#### [Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.

```sql
SELECT EmployeeUNI.unique_id, Employees.name
FROM Employees
LEFT JOIN EmployeeUNI
ON Employees.id = EmployeeUNI.id
```

#### [Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to report the product_name, year, and price for each sale_id in the Sales table.

```sql
SELECT product_name, year, price
FROM Product p, Sales s
WHERE p.product_id = s.product_id
```

#### [Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.

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

#### [Rising Temperature](https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).

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

#### [Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=sql-free-50)

The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.

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

#### [Employee Bonus](https://leetcode.com/problems/employee-bonus/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to report the name and bonus amount of each employee with a bonus less than 1000.

```sql
SELECT name, bonus FROM Employee 
LEFT JOIN Bonus
ON Employee.EmpId = Bonus.EmpId
WHERE bonus IS NULL OR bonus < 1000
```

#### [Students and Examinations](https://leetcode.com/problems/students-and-examinations/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to find the number of times each student attended each exam.

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

#### [Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to find managers with at least five direct reports.

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

#### [Confirmation Rate](https://leetcode.com/problems/confirmation-rate/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to find the confirmation rate of each user.

```sql
SELECT s.user_id,ROUND(
    IFNULL(AVG(c.action = 'confirmed'), 0), 2
) AS confirmation_rate
FROM Signups AS s
LEFT JOIN Confirmations AS c
ON s.user_id = c.user_id
GROUP BY s.user_id
```

#### [Not Boring Movies](https://leetcode.com/problems/not-boring-movies/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to report the movies with an odd-numbered ID and a description that is not "boring". Return the result table ordered by rating in descending order.

```sql
SELECT * FROM cinema
WHERE mod(id, 2) = 1 
AND description != 'boring'
ORDER BY rating DESC
```

#### [Average Selling Price](https://leetcode.com/problems/average-selling-price/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to find the average selling price for each product. average_price should be rounded to 2 decimal places. If a product does not have any sold units, its average selling price is assumed to be 0.

```sql
SELECT product_id, IFNULL(ROUND(SUM(sales)/SUM(units) ,2), 0) AS average_price
FROM (
    SELECT Prices.product_id AS product_id, 
        Prices.price * UnitsSold.units AS sales,
        UnitsSold.units AS units
    FROM Prices
    LEFT JOIN UnitsSold 
    ON Prices.product_id = UnitsSold.product_id
    AND (UnitsSold.purchase_date BETWEEN Prices.start_date AND Prices.end_date)
) T
GROUP BY product_id
```
or
```sql
SELECT p.product_id, 
    ROUND(IFNULL(SUM(p.price*u.units)/SUM(u.units) , 0) ,2) AS average_price
FROM Prices AS p
LEFT OUTER JOIN UnitsSold AS u
ON p.product_id = u.product_id 
AND (u.purchase_date BETWEEN p.start_date AND p.end_date)
GROUP BY p.product_id
```

#### [Project Employees I](https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=sql-free-50)

Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.

```sql
SELECT project_id, ROUND(AVG(e.experience_years), 2) AS average_years
FROM Project as p
LEFT JOIN Employee as e
ON p.employee_id = e.employee_id
GROUP BY p.project_id
```

#### [Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to find the percentage of the users registered in each contest rounded to two decimals. Return the result table ordered by percentage in descending order. In case of a tie, order it by contest_id in ascending order.

```sql
SELECT contest_id, 
    ROUND(COUNT(user_id)*100 / (SELECT COUNT(*) FROM Users), 2) percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC, contest_id
```

#### [Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to find each query_name, the quality and poor_query_percentage. Both quality and poor_query_percentage should be rounded to 2 decimal places.

```sql
SELECT 
    query_name, 
    ROUND(AVG(rating/position), 2) quality,
    ROUND(SUM(IF(rating<3, 1, 0))*100 / COUNT(*), 2) poor_query_percentage
FROM Queries 
WHERE query_name IS NOT NULL
GROUP BY query_name 
```

#### [Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/description/?envType=study-plan-v2&envId=sql-free-50)

Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

```sql
SELECT DATE_FORMAT(trans_date, '%Y-%m') AS month, country,
    COUNT(*) AS trans_count,
    COUNT(IF(state = 'approved', 1, NULL)) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(IF(state = 'approved', amount, 0)) AS approved_total_amount
FROM Transactions
GROUP BY month, country
```

#### [Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

```sql
SELECT ROUND(
    SUM(order_date = customer_pref_delivery_date) * 100 / COUNT(*), 2
) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date)
IN (
    SELECT customer_id, min(order_date)
    FROM Delivery
    GROUP BY customer_id
)
```

#### [Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/description/?envType=study-plan-v2&envId=sql-free-50)

Write a solution to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.

```sql
SELECT IFNULL(ROUND(
    COUNT(DISTINCT(Result.player_id))/COUNT(DISTINCT(Activity.player_id)),
     2), 0) AS fraction
FROM (
    SELECT Activity.player_id AS player_id
    FROM(
        SELECT player_id, DATE_ADD(MIN(event_date), INTERVAL 1 DAY) 
        AS second_date
        FROM Activity
        GROUP BY player_id
    ) AS Expected, Activity
    WHERE Activity.event_date = Expected.second_date 
    AND Activity.player_id = Expected.player_id
)
AS Result, Activity
```
or
```sql
SELECT 
    ROUND(COUNT(*)/(SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
FROM Activity 
WHERE (player_id, event_date) IN (
    SELECT player_id, DATE_ADD(MIN(event_date), INTERVAL 1 DAY)
    FROM Activity
    GROUP BY player_id
)
```