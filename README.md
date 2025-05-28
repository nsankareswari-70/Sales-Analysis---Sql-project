# Sales-Analysis-Sql-project
Retail Sales Analysis
## Project Overview:
Project Title: Retail Sales Analysis

Database: RetailSales

This project helped me to demonstrate my SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.
## Objectives:
1. Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
2. Data Cleaning: Identify and remove any records with missing or null values.
3. Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
4. Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.
## Project Structure
### 1. Database Setup
Database Creation:
The project starts by creating a database named RetailSales

Table Creation: A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
```sql
create database RetailSales;

-- Create Table
DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales
            (
                transaction_id INT PRIMARY KEY,	
                sale_date DATE,	 
                sale_time TIME,	
                customer_id	INT,
                gender	VARCHAR(15),
                age	INT,
                category VARCHAR(15),	
                quantity	INT,
                price_per_unit FLOAT,	
                cogs	FLOAT,
                total_sale FLOAT
            );
			
select * from retail_sales;
```
## 2. Data Exploration & Cleaning
Record Count: Determine the total number of records in the dataset.
Customer Count: Find out how many unique customers are in the dataset.
Category Count: Identify all unique product categories in the dataset.
Null Value Check: Check for any null values in the dataset and delete records with missing data.
```sql
--To view the 10 records
SELECT * FROM retail_sales
LIMIT 10

--Get the count of the records in the retail_sales
SELECT 
    COUNT(*) 
FROM retail_sales

--Data cleaning - selecting and deleting the null records

SELECT * FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantity IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;


DELETE FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantity IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;


-- Data Exploration

-- How many sales do we have?
select count(*) as Totalsale from retail_sales;

-- How many unique customers do we have?
select count(distinct customer_id) from retail_sales;

-- How many categories do we have?
select distinct category from retail_sales;

select count(distinct category) from retail_sales;
select distinct category from retail_sales;

```


### Data Analysis - Business key problems and answers
#### My analysis and findings
```sql
--1. Write a sql query to retrieve all columns for sales made on '2022-11-05'
--2.Write a sql query to retrieve all transactions where the category is 'Clothing' and quantity is >=4 and sales date is nov-2022
--3. Write a sql query to calculate the total sales of each category.
--3.1 write a sql query to find the total number of orders from each category.
--3.2 combined query
--4. Write a sql query to find the average age of customers who purchased items from the 'Beauty' category;
--5. Write a sql query to find all transactions where the total_sale is greater than 1000.
--6. Write a sql query to find the total number of transactions (transaction_id) made by each gender in each category.
--7. Write a sql query to calculate the average sale for each month. Find out best selling month in each year.
--8. Write a sql query to find the top 5 customers based on the highest total sales.
--9. Write a sql query to find the number of unique customers who purchased items from each category.
--10. Write a sql query to create each shift and number of orders ( Example Morning <12, Afternoon between 12 & 17,
--Evening>17 )
```
