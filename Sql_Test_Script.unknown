CREATE TABLE sales_data (
    sale_id       NUMBER PRIMARY KEY,
    employee_id   NUMBER,
    employee_name VARCHAR2(100),
    department    VARCHAR2(50),
    sale_amount   NUMBER,
    sale_date     DATE
);

INSERT INTO sales_data VALUES (1, 101, 'Alice', 'Electronics', 1000, TO_DATE('2023-01-10', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (2, 102, 'Bob', 'Electronics', 800, TO_DATE('2023-01-12', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (3, 103, 'Charlie', 'Furniture', 950, TO_DATE('2023-01-11', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (4, 104, 'David', 'Electronics', 1200, TO_DATE('2023-01-15', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (5, 105, 'Eva', 'Furniture', 900, TO_DATE('2023-01-17', 'YYYY-MM-DD'));
INSERT INTO sales_data VALUES (6, 106, 'Frank', 'Furniture', 950, TO_DATE('2023-01-20', 'YYYY-MM-DD'));

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

SELECT 
    department,
    employee_name,
    sale_amount,
    RANK() OVER (PARTITION BY department ORDER BY sale_amount DESC) AS rank,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY sale_amount DESC) AS dense_rank
FROM sales_data

SELECT * FROM (
    SELECT 
        department,
        employee_name,
        sale_amount,
        RANK() OVER (PARTITION BY department ORDER BY sale_amount DESC) AS sale_rank
    FROM sales_data
)
WHERE sale_rank <= 3;

SELECT * FROM (
    SELECT 
        department,
        employee_name,
        sale_date,
        ROW_NUMBER() OVER (PARTITION BY department ORDER BY sale_date) AS row_num
    FROM sales_data
)
WHERE row_num <= 2;

SELECT 
    department,
    employee_name,
    sale_amount,
    MAX(sale_amount) OVER (PARTITION BY department) AS max_in_dept,
    MAX(sale_amount) OVER () AS max_overall
FROM sales_data;


