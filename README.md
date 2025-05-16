# 🛒 Retail Sales Analysis (SQL Project)

**Level**: Beginner  
**Tools Used**: SQL  

This project demonstrates my SQL skills applied to a retail sales dataset. It includes setting up the database, cleaning the data, performing exploratory analysis, and answering key business questions using SQL queries.

---

## 🔍 Project Objectives

- 🗄️ Set up and populate a retail sales database  
- 🧹 Clean and prepare the data for analysis  
- 📊 Perform exploratory data analysis (EDA)  
- 💡 Answer business-focused queries to derive meaningful insights  

---

## 📊 Key Insights

- 🏷️ Identified top-performing product categories and customer segments  
- 📆 Analyzed sales trends across dates, months, and time shifts (morning, afternoon, evening)  
- 💰 Discovered top-spending customers and high-value transactions  
- ✅ Ensured clean and consistent data by removing null or incomplete records  

---
## 📊 SQL Retail Sales Analysis

This analysis focuses on exploring and extracting insights from retail sales data using SQL.

---

### 🛠️ Table Creation

```sql
DROP TABLE IF EXISTS retail_sales;

CREATE TABLE retail_sales (
    transactions_id     INT,
    sale_date           DATE,
    sale_time           TIME,
    customer_id         INT,     
    gender              VARCHAR(20),
    age                 INT,
    category            VARCHAR(15),
    quantiy             INT,
    price_per_unit      FLOAT,    
    cogs                FLOAT,
    total_sale          FLOAT
);
```

---

### 🔍 Initial Inspection

```sql
SELECT * FROM retail_sales
LIMIT 10;

SELECT COUNT(*) FROM retail_sales;
```

---

### 🧹 Data Cleaning

```sql
-- Check for NULLs
SELECT * FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;

-- Delete NULL records
DELETE FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

---

### 📈 Data Exploration

```sql
-- Total Sales
SELECT COUNT(*) AS total_sales FROM retail_sales;

-- Unique Customers
SELECT COUNT(DISTINCT customer_id) AS total_customers FROM retail_sales;

-- Unique Categories
SELECT COUNT(DISTINCT category) AS total_categories FROM retail_sales;

-- List of Categories
SELECT DISTINCT category FROM retail_sales;
```

---

### 🧠 Data Analysis & Insights

#### ✅ Q1: All sales made on '2022-11-05'
```sql
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
```

#### ✅ Q2: Clothing category with quantity > 4 in Nov-2022
```sql
SELECT * 
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantiy >= 4;
```

#### ✅ Q3: Total sales for each category
```sql
SELECT category,
       SUM(total_sale) AS net_sale,
       COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

#### ✅ Q4: Average age of Beauty category customers
```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

#### ✅ Q5: Transactions with total_sale > 1000
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000;
```

#### ✅ Q6: Transactions by gender in each category
```sql
SELECT category,
       gender,
       COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

#### ✅ Q7: Best-selling month in each year
```sql
SELECT year,
       month,
       avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        ROUND(AVG(total_sale)) AS avg_sale,
        RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) AS t1
WHERE rank = 1;
```

#### ✅ Q8: Top 5 customers by total sales
```sql
SELECT customer_id,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

#### ✅ Q9: Unique customers per category
```sql
SELECT category,
       COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

#### ✅ Q10: Orders by shift (Morning, Afternoon, Evening)
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

Let me know if you'd like a visual dashboard, ERD diagram, or setup instructions added!


## 📁 How to Use

1. **Clone** this repository  
   ```bash
   git clone https://github.com/guptaghanshyam/Retail-Sales-Analysis-SQL-Project
