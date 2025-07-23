

#   Sales Analysis – SQL Project

## 📌 Project Overview

This project focuses on analyzing a fictional retail dataset using **SQL**. The goal is to explore, clean, and query the data to extract meaningful insights about **customer behavior**, **sales trends**, and **business performance**.

---

## 🛠 Tools & Technologies

* **SQL**
* **MySQL / PostgreSQL** (syntax compatible)
* **DBMS Client** (e.g., MySQL Workbench, DBeaver)

---

## 🗂 Database & Table Structure

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

## 🔍 Data Exploration & Cleaning

### ✅ Record Count

```sql
SELECT COUNT(*) FROM retail_sales;
```

### ✅ Unique Customers

```sql
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
```

### ✅ Categories

```sql
SELECT DISTINCT category FROM retail_sales;
```

### ✅ Null Value Check & Deletion

```sql
SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
      gender IS NULL OR age IS NULL OR category IS NULL OR 
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
      gender IS NULL OR age IS NULL OR category IS NULL OR 
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

## 📊 Data Analysis & Insights

### 🔹 All Sales on a Specific Date

```sql
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
```

### 🔹 Clothing Sales (Quantity > 4) in Nov 2022

```sql
SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
  AND quantity >= 4;
```

### 🔹 Total Sales & Orders by Category

```sql
SELECT category,
       SUM(total_sale) AS net_sale,
       COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

### 🔹 Average Age of Beauty Product Customers

```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

### 🔹 Transactions with Sales > 1000

```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000;
```

### 🔹 Transactions Count by Gender per Category

```sql
SELECT category,
       gender,
       COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

### 🔹 Best Selling Month Each Year

```sql
SELECT year, month, avg_sale
FROM (
    SELECT EXTRACT(YEAR FROM sale_date) AS year,
           EXTRACT(MONTH FROM sale_date) AS month,
           AVG(total_sale) AS avg_sale,
           RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY year, month
) AS ranked
WHERE rank = 1;
```

### 🔹 Top 5 Customers by Total Sales

```sql
SELECT customer_id,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

### 🔹 Unique Customers by Category

```sql
SELECT category,
       COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

### 🔹 Sales by Shift (Morning, Afternoon, Evening)

```sql
WITH hourly_sale AS (
    SELECT *,
           CASE
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
               ELSE 'Evening'
           END AS shift
    FROM retail_sales
)
SELECT shift,
       COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

---

## ✅ Key Learnings

✔ Data profiling and null handling using SQL
✔ Aggregated metrics for customer behavior by **gender**, **category**, and **time**
✔ Identified **high-value customers** and **peak sales periods**
✔ Classified sales into **business shifts** for deeper time-based analysis

---

## 🏁 Conclusion

This project illustrates how **SQL** can be leveraged to analyze structured retail data, uncover hidden patterns, and enable **data-driven business decisions**.

