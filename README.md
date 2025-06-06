# Retail Sales Data Analysis with SQL

## Project Overview

This project focuses on performing a comprehensive data analysis of retail sales data using SQL. The primary objective was to extract meaningful insights from a transactional dataset, clean the data for accuracy, and answer key business questions related to sales performance, customer behavior, and product categories.

The project demonstrates fundamental SQL skills in database creation, table management, data cleaning, data exploration, and advanced querying for business intelligence.

## Database Setup

The project began by setting up a dedicated database and defining the schema for the `retail_sales` table.

### Database Creation
A new database named `sales_project` was created.

### Table Definition
The `retail_sales` table was created with specific columns to capture essential transaction details, including:
* `transactions_id` (Primary Key)
* `sale_date`
* `sale_time`
* `customer_id`
* `gender`
* `age`
* `category`
* `quantity`
* `price_per_unit`
* `cogs` (Cost of Goods Sold)
* `total_sale`

## Data Cleaning and Preparation

Before analysis, data integrity was ensured by identifying and removing records with missing values.

### Initial Data Inspection
Initial checks were performed to view a sample of the raw data and determine the total number of records, providing a preliminary understanding of the dataset's size and structure.

### Identifying and Deleting Null Values
A systematic check was conducted across all critical columns (`transactions_id`, `sale_date`, `sale_time`, `customer_id`, `gender`, `age`, `category`, `quantity`, `price_per_unit`, `cogs`, `total_sale`) to identify any rows containing `NULL` values. All identified rows with missing data were subsequently removed to maintain data quality for accurate analysis. The total record count was then re-verified to confirm the cleaning process.

## Data Exploration

Basic exploration queries were executed to get an overview of the dataset's contents and key summary metrics.

* **Total Sales Transactions:** The total number of individual sales transactions in the dataset was counted.
* **Unique Customers:** The total number of distinct customers who made purchases was determined.
* **Distinct Categories:** A list of all unique product categories available in the sales data was retrieved.

## Data Analysis & Business Key Problems & Answers

A series of SQL queries were developed to address specific business questions, providing actionable insights from the sales data.

* **Q.1 Sales on a Specific Date:** Retrieved all sales records that occurred on a particular date (`2022-11-05`).
* **Q.2 Clothing Sales in November 2022:** Identified all transactions within the 'Clothing' category where the quantity sold was greater than 4, specifically for the month of November 2022.
* **Q.3 Total Sales and Orders by Category:** Calculated the aggregated total sales revenue and the total number of orders for each distinct product category.
* **Q.4 Average Age for Beauty Category Purchases:** Determined the average age of customers who purchased items specifically from the 'Beauty' category.
* **Q.5 High-Value Transactions:** Identified all individual transactions where the `total_sale` amount exceeded 1000.
* **Q.6 Transactions by Gender and Category:** Counted the total number of transactions made by each gender within every product category, providing insights into purchasing patterns.
* **Q.7 Monthly Average Sales and Best-Selling Month:** Calculated the average sale for each month across all years. Furthermore, identified the single best-selling month (based on average sales) for each year using ranking functions.
* **Q.8 Top 5 Customers by Sales:** Identified the top 5 customers with the highest cumulative total sales amount.
* **Q.9 Unique Customers per Category:** Determined the number of unique customers who purchased items from each individual product category.
* **Q.10 Sales Shifts and Order Counts:** Defined sales shifts (Morning: before 12 PM, Afternoon: 12 PM to 5 PM, Evening: after 5 PM based on `sale_time`) and then counted the total number of orders that occurred within each of these defined shifts.

## Technologies Used

* **SQL (PostgreSQL):** Utilized for all database management, data manipulation, and querying operations.
* **Database Client:** A SQL client (pgAdmin) is used to execute the queries.
* **Data Source:** The project implicitly relies on retail sales data, typically loaded from a CSV file into the database.

## How to Run This Project

To replicate this analysis:
1.  **Set up your Database:** Ensure a SQL database ( PostgreSQL) is installed and running.
2.  **Create the Database:** Execute the SQL command to create the `sales_project` database.
3.  **Connect to the Database:** Connect your SQL client to the newly created database.
4.  **Create the Table:** Run the SQL DDL statement to define the `retail_sales` table structure.
5.  **Load Data:** Import your retail sales data (from a CSV file) into the `retail_sales` table using your database's specific import utility.
6.  **Execute Queries:** Run the provided SQL queries for data cleaning, exploration, and analysis sequentially to reproduce the insights.
