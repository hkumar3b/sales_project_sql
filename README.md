# Retail Sales Data Analysis with SQL

## Project Overview

This project focuses on performing a comprehensive data analysis of retail sales data using SQL. The primary objective was to extract meaningful insights from a transactional dataset, clean the data for accuracy, and answer key business questions related to sales performance, customer behavior, and product categories.

The project demonstrates fundamental SQL skills in database creation, table management, data cleaning, data exploration, and advanced querying for business intelligence.

## Database Setup

The project begins by setting up a dedicated database and defining the schema for the `retail_sales` table.

### Database Creation
CODE BLOCK START
CREATE DATABASE sales_project;
CODE BLOCK END

### Table Definition
The `retail_sales` table captures essential transaction details, including customer information, product categories, quantities, and sales figures.

CODE BLOCK START
DROP TABLE IF EXISTS retail_sales;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(50),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale INT
);
CODE BLOCK END

## Data Cleaning and Preparation

Before analysis, data integrity was ensured by identifying and removing records with missing values.

### Initial Data Inspection
To understand the raw data structure and identify potential issues:
CODE BLOCK START
SELECT * FROM retail_sales LIMIT 10;
SELECT COUNT(*) FROM retail_sales;
CODE BLOCK END

### Identifying Null Values
A check was performed to identify any rows containing `NULL` values across critical columns:
CODE BLOCK START
SELECT * FROM retail_sales
WHERE
    transactions_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR customer_id IS NULL
    OR gender IS NULL
    OR age IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR price_per_unit IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;
CODE BLOCK END

### Deleting Null Value Data
Rows with any `NULL` values in the specified columns were removed to maintain data quality for analysis:
CODE BLOCK START
DELETE FROM retail_sales
WHERE
    transactions_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR customer_id IS NULL
    OR gender IS NULL
    OR age IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR price_per_unit IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;

SELECT COUNT(*) From retail_sales; -- Verify count after deletion
CODE BLOCK END

## Data Exploration

Basic exploration queries were executed to get an overview of the dataset's contents and key metrics.

* Total Sales Transactions:
CODE BLOCK START
SELECT COUNT(*) as total_sale FROM retail_sales;
CODE BLOCK END
* Unique Customers:
CODE BLOCK START
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
CODE BLOCK END
* Distinct Categories:
CODE BLOCK START
SELECT DISTINCT category FROM retail_sales;
CODE BLOCK END

## Data Analysis & Business Key Problems & Answers

A series of SQL queries were developed to address specific business questions, providing actionable insights from the sales data.

### Q.1 Retrieve all sales made on a specific date ('2022-11-05').
CODE BLOCK START
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
CODE BLOCK END

### Q.2 Find all transactions for 'Clothing' category with quantity more than 4 in November 2022.
CODE BLOCK START
SELECT * FROM retail_sales
WHERE Category ='Clothing'
AND quantity >=4
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11';
CODE BLOCK END

### Q.3 Calculate the total sales and total orders for each product category.
CODE BLOCK START
SELECT
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1;
CODE BLOCK END

### Q.4 Determine the average age of customers who purchased items from the 'Beauty' category.
CODE BLOCK START
SELECT ROUND(AVG(age), 2) FROM retail_sales
WHERE Category = 'Beauty';
CODE BLOCK END

### Q.5 Identify all transactions where the total sale amount is greater than 1000.
CODE BLOCK START
SELECT * FROM retail_sales
WHERE total_sale >= 1000;
CODE BLOCK END

### Q.6 Find the total number of transactions made by each gender within each product category.
CODE BLOCK START
SELECT
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP BY
    category,
    gender
ORDER BY 1;
CODE BLOCK END

### Q.7 Calculate the average sale for each month and identify the best-selling month in each year.
This involves using a Common Table Expression (CTE) and window functions (`RANK()`) to find the top month per year based on average sales.
CODE BLOCK START
-- Average sale per month per year
SELECT
  EXTRACT(YEAR FROM sale_date) as year,
  EXTRACT(MONTH FROM sale_date) as month,
  AVG(total_sale) as avg_sale
FROM retail_sales
GROUP BY 1, 2
ORDER BY 1 DESC, 3 DESC;

-- Best selling month in each year
WITH monthly_avg_sales AS
(
    SELECT
        EXTRACT(YEAR FROM sale_date) as year,
        EXTRACT(MONTH FROM sale_date) as month,
        AVG(total_sale) as avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank_in_year
    FROM retail_sales
    GROUP BY 1, 2
)
SELECT
    year,
    month,
    avg_sale
FROM monthly_avg_sales
WHERE rank_in_year = 1;
CODE BLOCK END

### Q.8 Identify the top 5 customers based on the highest total sales.
CODE BLOCK START
SELECT customer_id, SUM(total_sale) as sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
CODE BLOCK END

### Q.9 Find the number of unique customers who purchased items from each category.
CODE BLOCK START
SELECT COUNT(DISTINCT customer_id) as unique_customers, category
FROM retail_sales
GROUP BY 2;
CODE BLOCK END

### Q.10 Create sales shifts (Morning, Afternoon, Evening) and count the number of orders in each shift.
This query defines shifts based on `sale_time` and aggregates orders by these shifts.
CODE BLOCK START
WITH hourly_sale AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT
    shift,
    COUNT(*) as total_orders
FROM hourly_sale
GROUP BY shift
ORDER BY total_orders;
CODE BLOCK END

## Technologies Used

* SQL (PostgreSQL/MySQL): For database management and querying.
* Database Client: Any SQL client (e.g., DBeaver, pgAdmin, MySQL Workbench) to execute the queries.
* (Implicit) CSV Data: Assumes the retail sales data was loaded from a CSV file into the database.

## How to Run This Project

1. Set up your Database: Ensure you have a SQL database (e.g., PostgreSQL, MySQL) installed and running.
2. Create the Database: Execute `CREATE DATABASE sales_project;` in your SQL client.
3. Connect to the Database: Connect your SQL client to the newly created `sales_project` database.
4. Create the Table: Run the `CREATE TABLE retail_sales (...)` statement to define the table structure.
5. Load Data: Import your retail sales data (presumably from a CSV file) into the `retail_sales` table. You would typically use a `COPY` command (for PostgreSQL) or `LOAD DATA INFILE` (for MySQL) for this.
    * Example (PostgreSQL): `COPY retail_sales FROM 'path/to/your/retail_sales_data.csv' DELIMITER ',' CSV HEADER;`
6. Execute Queries: Run the SQL queries provided in the `Data Cleaning`, `Data Exploration`, and `Data Analysis` sections sequentially.
