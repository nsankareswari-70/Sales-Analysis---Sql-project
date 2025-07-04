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
* Record Count: Determine the total number of records in the dataset.
* Customer Count: Find out how many unique customers are in the dataset.
* Category Count: Identify all unique product categories in the dataset.
* Null Value Check: Check for any null values in the dataset and delete records with missing data.
  
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
#### 1. Write a sql query to retrieve all columns for sales made on '2022-11-05'
```sql
select * from retail_sales where sale_date='2022-11-05';
```
#### 2.Write a sql query to retrieve all transactions where the category is 'Clothing' and quantity is >=4 and sales date is nov-2022
```sql
select  * from retail_sales where category='Clothing' 
and quantity>=4
and extract(month from sale_date)=11 
and  extract(year from sale_date)=2022;

select * from retail_sales where category='Clothing'
and quantity>=4
and to_char(sale_date,'yyyy-mm') = '2022-11';
```
#### 3. Write a sql query to calculate the total sales of each category.
```sql
select category,sum(total_sale) from retail_sales group by category;
```
#### 3.1 write a sql query to find the total number of orders from each category.
```sql
select category,count(*) as total_orders from retail_sales group by category;
```
#### 3.2 combined query
```sql
select category,sum(total_sale) as Total_sales,count(*) as Number_of_orders from retail_sales
group by category;
```

### 4. Write a sql query to find the average age of customers who purchased items from the 'Beauty' category;
```sql
select round(avg(age),2) from retail_sales where category='Beauty';
```
### 5. Write a sql query to find all transactions where the total_sale is greater than 1000.
```sql
select * from retail_sales where total_sale > 1000;
```
### 6. Write a sql query to find the total number of transactions (transaction_id) made by each gender in each category.
```sql
select category,gender,count(*) as total_number_of_transactions from retail_sales group by category,gender order by category;
```
### 7. Write a sql query to calculate the average sale for each month. Find out best selling month in each year.
```sql
select * from 
(
select extract(year from sale_date) as yearinfo,
extract(month from sale_date) as monthinfo,
round(avg(total_sale)::numeric,2) as avg_sales,
rank() over(partition by  extract(year from sale_date) order by round(avg(total_sale)::numeric,2)desc )
 from retail_sales 
 group by yearinfo,monthinfo) as t where rank=1;
 ```
### 8. Write a sql query to find the top 5 customers based on the highest total sales.
```sql
select customer_id,sum(total_sale)as totsales from retail_sales group by customer_id order by totsales desc limit 5;
```
### 9. Write a sql query to find the number of unique customers who purchased items from each category.
```sql
select category,count(distinct customer_id) from retail_sales group by category;
```
### 10. Write a sql query to create each shift and number of orders ( Example Morning <12, Afternoon between 12 & 17,Evening>17 )

```sql

select 
case when extract(hour from sale_time) < 12 then 'Morning'
     when extract(hour from sale_time) between 12 and 17 then 'Afternoon'
	 when extract(hour from sale_time) >17 then 'Evening'
end as shift,count(*) as number_of_orders
from retail_sales group by shift;
```
## Findings
* Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
* High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
* Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
* Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.

## Reports
* Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.
* Trend Analysis: Insights into sales trends across different months and shifts.
* Customer Insights: Reports on top customers and unique customer counts per category.
## Conclusion
This project demonstrates my ability to setup the database , data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

#### End of project
