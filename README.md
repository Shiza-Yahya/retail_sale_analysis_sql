# Retail Sales Analysis (SQL Project)

## Overview
This is  SQL project built to practice and demonstrate core data analysis skills. A retail sales database (`sr_analysis`) is created and populated with transaction-level sales data, which is then cleaned and analyzed using SQL to answer real business questions.

**Database:** sr_analysis

## What This Project Does
1. **Database Setup** – Creates a database and a `retail_sales` table to store transaction details such as sale date/time, customer info (ID, gender, age), product category, quantity, price, cost of goods sold, and total sale amount.
2. **Data Cleaning** – Checks the dataset for missing/null values and removes incomplete records so the analysis is based on clean data.
3. **Exploratory Data Analysis (EDA)** – Looks at basic dataset stats like total number of sales and total/unique customers.
4. **Business Analysis** – Runs SQL queries to answer specific business questions, including:
   - Sales on a specific date
   - High-quantity clothing orders in a given month
   - Total sales and orders per category
   - Average age of customers buying from a specific category
   - High-value transactions (sales above a set threshold)
   - Transaction counts by gender and category
   - Best-selling month for each year
   - Top 5 customers by total spend
   - Unique customers per category
   - Order distribution across Morning, Afternoon, and Evening shifts

## Key Findings
- **Dataset size:** 2,000 total transactions from 155 unique customers.
- **Demographics:** Customers range from age 18 to 64; average age of Beauty category buyers is 40. Male and female customers are fairly evenly spread across categories.
- **Category performance:**

  | Category    | Total Sales | Total Orders |
  |-------------|-------------|---------------|
  | Electronics | $311,445    | 678           |
  | Clothing    | $309,995    | 698           |
  | Beauty      | $286,790    | 611           |

  Electronics brought in the highest revenue, while Clothing had the most orders.
- **Sales trends:** July 2022 (avg. sale $541) and February 2023 (avg. sale $535) were the best-selling months of their respective years.
- **Top customers:** The top 5 customers contributed between $32,875 and $38,440 each in total sales.
- **Shift analysis:** Evening (after 5 PM) is by far the busiest shopping period with 1,062 orders — over half of all orders — compared to 549 in the afternoon and 548 in the morning.

## Skills Demonstrated
- Database and table design
- Data cleaning (null handling)
- Aggregations and grouping
- Window functions (RANK) for trend analysis
- CASE statements for custom labeling (shift classification)
- Business-driven query writing

## How to Use
1. Clone this repository.
2. Run the SQL script to create the `sr_analysis` database and `retail_sales` table.
3. Import your sales dataset (CSV) into the table.
4. Run the analysis queries to explore the data or adapt them for your own questions.

## Author
This project is part of a data analyst portfolio, built to demonstrate practical SQL skills. Feel free to reach out with questions, feedback, or collaboration ideas.
