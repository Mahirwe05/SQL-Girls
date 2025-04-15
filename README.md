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


1. SELECT 
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

Explanation: 
LAG() pulls the previous record sale_amount.
```
![Lag](https://github.com/user-attachments/assets/e323400d-1fa3-46e1-94f2-8d6acea13a96)

```sql
2.SELECT 
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



```sql
3. SELECT 
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




