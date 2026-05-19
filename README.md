# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `sr_analysis`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `sr_analysis`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sr_analysis;
USE sr_analysis;

DROP TABLE IF EXISTS retail_sales;

CREATE TABLE retail_sales (
    transactions_id  INT PRIMARY KEY,
    sale_date        DATE,
    sale_time        TIME,
    customer_id      INT,
    gender           VARCHAR(15),
    age              INT,
    category         VARCHAR(15),
    quantity         INT,
    price_per_unit   FLOAT,
    cogs             FLOAT,
    total_sale       FLOAT
);
```

> **Note**: If the table was originally created with a typo in the column name (`quantiy`), it can be corrected using:
> ```sql
> ALTER TABLE retail_sales
> RENAME COLUMN quantiy TO quantity;
> ```

---

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many customers are in the dataset.
- **Unique Customer Count**: Find the number of unique customers.
- **Null Value Check**: Check for any null values across all columns and identify incomplete records.

```sql
-- Total number of sales
SELECT COUNT(*) AS total_sale FROM retail_sales;

-- Total number of customers
SELECT COUNT(customer_id) FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Null value check
SELECT * FROM retail_sales
WHERE
    transactions_id  IS NULL
    OR sale_date     IS NULL
    OR sale_time     IS NULL
    OR customer_id   IS NULL
    OR gender        IS NULL
    OR age           IS NULL
    OR category      IS NULL
    OR quantity      IS NULL
    OR price_per_unit IS NULL
    OR cogs          IS NULL
    OR total_sale    IS NULL;
```

---

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Retrieve all columns for sales made on '2022-12-05':**
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-12-05';
```

2. **Retrieve all transactions where the category is 'Clothing', quantity sold is 4 or more, in November 2022:**
```sql
SELECT *
FROM retail_sales
WHERE
    category = 'Clothing'
    AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
    AND quantity >= 4;
```

3. **Calculate total sales and total orders for each category:**
```sql
SELECT
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*)        AS total_orders
FROM retail_sales
GROUP BY category;
```

4. **Find the average age of customers who purchased from the 'Beauty' category:**
```sql
SELECT
    FLOOR(AVG(age)) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

5. **Find all transactions where total_sale is greater than 1000:**
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

6. **Find the total number of transactions made by each gender in each category:**
```sql
SELECT
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

7. **Calculate the average sale for each month and find the best-selling month in each year:**
```sql
SELECT
    year,
    month,
    avg_sale
FROM (
    SELECT
        EXTRACT(YEAR  FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale)               AS avg_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rnk
    FROM retail_sales
    GROUP BY
        EXTRACT(YEAR  FROM sale_date),
        EXTRACT(MONTH FROM sale_date)
) t1
WHERE rnk = 1;
```

8. **Find the top 5 customers based on highest total sales:**
```sql
SELECT
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

9. **Find the number of unique customers who purchased items from each category:**
```sql
SELECT
    category,
    COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

10. **Create shift labels and count orders per shift (Morning < 12, Afternoon 12–17, Evening > 17):**
```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12                    THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17       THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT
    shift,
    COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

---

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis reveals variations in sales performance, helping identify peak seasons across years.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.
- **Shift Analysis**: Order volume broken down by time of day (Morning, Afternoon, Evening) reveals customer shopping patterns throughout the day.

---
## Findings

### Sales Overview
- The dataset contains **2,000 total transactions** across multiple product categories.
- A total of **155 unique customers** were identified in the dataset.

### Customer Demographics
- Customers range from ages **18 to 64**, covering a wide demographic.
- The average age of customers who purchased from the **Beauty category is 40 years**.
- Both male and female customers are fairly evenly distributed across all categories.

### Category Performance
| Category | Total Sales | Total Orders |
|----------|-------------|--------------|
| Electronics | $311,445 | 678 |
| Clothing | $309,995 | 698 |
| Beauty | $286,790 | 611 |

- **Electronics** generated the highest total revenue.
- **Clothing** had the most number of orders.

### High-Value Transactions
- Several transactions exceeded **$1,000** in total sale value, indicating consistent premium purchases across categories.

### Sales Trends
- **2022** — Best selling month was **July** with an average sale of **$541**
- **2023** — Best selling month was **February** with an average sale of **$535**

### Top 5 Customers
| Rank | Customer ID | Total Sales |
|------|-------------|-------------|
| 1 | CUST_ID_XXX | $38,440 |
| 2 | CUST_ID_XXX | $36,580 |
| 3 | CUST_ID_XXX | $35,100 |
| 4 | CUST_ID_XXX | $33,960 |
| 5 | CUST_ID_XXX | $32,875 |

### Orders by Shift
| Shift | Total Orders |
|-------|-------------|
| Evening (after 5 PM) | 1,062 |
| Afternoon (12 PM – 5 PM) | 549 |
| Morning (before 12 PM) | 548 |

- **Evening** is the most active shopping period, accounting for over **50% of all orders**.
---

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and time shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

---

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

---

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts to create the `sr_analysis` database and the `retail_sales` table.
3. **Import the Data**: Load your CSV or dataset into the `retail_sales` table.
4. **Run the Queries**: Execute the SQL queries provided above to perform your analysis.
5. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

---

## Author

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

Thank you for your support, and I look forward to connecting with you!
