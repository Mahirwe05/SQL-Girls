#  PL/SQL Window Functions Assignment

## Introduction:


**1. Names: Mahirwe Yvette        ID: 26510**

**2. Names: Irakoze Grace Vanny   ID: 26425**

---

## Objective
This project demonstrates the use of **SQL Window Functions** on a generic sales dataset using Oracle PL/SQL. The functions used include:

- `LAG()`, `LEAD()` – for comparing current rows with previous/next.
- `RANK()`, `DENSE_RANK()`, `ROW_NUMBER()` – for ordering and ranking within groups.
- Aggregate functions like `MAX()` – with and without `PARTITION BY`.

---

## 🗂️ Dataset Used

### `sales_data`

```sql
CREATE TABLE sales_data (
    sale_id       NUMBER PRIMARY KEY,
    employee_id   NUMBER,
    employee_name VARCHAR2(100),
    department    VARCHAR2(50),
    sale_amount   NUMBER,
    sale_date     DATE
);
```

![Dataset](https://github.com/user-attachments/assets/8023c322-3d58-409e-ae69-9a411050fd0c)



```sql
INSERT INTO sales_data VALUES (1, 101, 'Alice', 'Electronics', 1000, TO_DATE('2023-01-10', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (2, 102, 'Bob', 'Electronics', 800, TO_DATE('2023-01-12', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (3, 103, 'Charlie', 'Furniture', 950, TO_DATE('2023-01-11', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (4, 104, 'David', 'Electronics', 1200, TO_DATE('2023-01-15', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (5, 105, 'Eva', 'Furniture', 900, TO_DATE('2023-01-17', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (6, 106, 'Frank', 'Furniture', 950, TO_DATE('2023-01-20', 'YYYY-MM-DD'));

```
1.🗂️LAG FUNCTION

```sql
 SELECT 
    employee_name,
    department,
    sale_amount,
    LAG(sale_amount) OVER (ORDER BY sale_date) AS prev_sale,
    CASE 
        WHEN sale_amount > LAG(sale_amount) OVER (ORDER BY sale_date) THEN 'HIGHER'
        WHEN sale_amount < LAG(sale_amount) OVER (ORDER BY sale_date) THEN 'LOWER'
        ELSE 'EQUAL'
    END AS comparison_with_prev
FROM sales_data;
```


Explanation: 
LAG() pulls the previous record sale_amount.

![Lag](https://github.com/user-attachments/assets/e323400d-1fa3-46e1-94f2-8d6acea13a96)


2.🗂️LEAD FUNCTION

```sql
SELECT 
    employee_name,
    department,
    sale_amount,
    LEAD(sale_amount) OVER (ORDER BY sale_date) AS next_sale,
    CASE 
        WHEN sale_amount > LEAD(sale_amount) OVER (ORDER BY sale_date) THEN 'HIGHER'
        WHEN sale_amount < LEAD(sale_amount) OVER (ORDER BY sale_date) THEN 'LOWER'
        ELSE 'EQUAL'
 END AS comparison_with_next
FROM sales_data;
```
Explanation: LEAD() checks the sale after the current one.

![lead()](https://github.com/user-attachments/assets/254a9037-15fd-4e97-b008-b57141d005a3)



3.🗂️RANK AND DENSE_RANK

```sql
 SELECT 
    department,
    employee_name,
    sale_amount,
    RANK() OVER (PARTITION BY department ORDER BY sale_amount DESC) AS rank,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY sale_amount DESC) AS dense_rank
FROM sales_data;
```
![rank denserank](https://github.com/user-attachments/assets/31714b2c-bc0e-4100-a213-6fefbe344ff1)


Explanation:
RANK() skips numbers when there is a tie.

DENSE_RANK() does not skip and gives continuous ranks.

Real life application

Used for leaderboards, employee performance comparisons, or prize distributions.

4.RANK FUNCTIONS
   ```SQL
    SELECT * FROM (
    SELECT 
        department,
        employee_name,
        sale_amount,
        RANK() OVER (PARTITION BY department ORDER BY sale_amount DESC) AS sale_rank
    FROM sales_data
)
WHERE sale_rank <= 3;
```
Explanation:
Fetches the top 3 sales per department.
RANK() allows ties to share the same rank.

Real-life Application
Useful for recognizing top performers in each team or region.

![RankFunction](https://github.com/user-attachments/assets/0bfa832a-5866-4e5d-8096-d20b0123ac96)


5. ROW NUMBER FUNCTIONS
```SQL
SELECT * FROM (
    SELECT 
        department,
        employee_name,
        sale_date,
        ROW_NUMBER() OVER (PARTITION BY department ORDER BY sale_date) AS row_num
    FROM sales_data
)
WHERE row_num <= 2;
```
Explanation:
Explanation
ROW_NUMBER() gives a unique number per department sorted by date.
Used to get the earliest employees, sales, etc.

Real-life Application
Great for onboarding analysis or tracking early adopters.


![RowNumberFunction](https://github.com/user-attachments/assets/7b305d09-0da5-44c8-8937-392ed48bd47e)



6. MAX AND MAX_OVERALL FUNCTIONS

    ```sql
SELECT 
    department,
    employee_name,
    sale_amount,
    MAX(sale_amount) OVER (PARTITION BY department) AS max_in_dept,
    MAX(sale_amount) OVER () AS max_overall
FROM sales_data;
```

Explanation
Shows both departmental and overall max sale values.
No GROUP BY needed—data remains row-wise.

Real-life Application
Used in dashboards for KPIs, benchmarks, and performance goals.


![AggregateFunction](https://github.com/user-attachments/assets/926debcc-f631-446f-a99d-bdffb042403d)






## Findings

Alice had the highest sale in Electronics.
Furniture had two employees with equal sales.
The earliest sale was on Jan 10, 2023.

## Conclusion
This database system efficiently manages sales data by tracking employee records and sales, their ids, employee names, departments, date, and sales amount. The queries help saler's to analyze their working and employee engagement.
________________________________________
For further enhancements, we could implement:
•	A feature to track overdue payments due date.
•	Automated fine calculations for late returns.
•	A notification system to remind salerperson of due dates.

**Repository:** [https://github.com/Mahirwe05/SQL-Girls/edit/main/README.md]

