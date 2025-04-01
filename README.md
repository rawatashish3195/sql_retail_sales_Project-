# sql_retail_sales_Project-


# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, 
clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA),
 and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis
  and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `sql_project`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

create database sql_project;
use sql_project;

create table Retail_Sales(
	transactions_id  int primary key,
	sale_date	date,
    sale_time time,
    customer_id	int,
    gender	varchar(20),
    age	int,
    category varchar(20),	
    quantity	int,
    price_per_unit	float,
    cogs	float,
    total_sale  float
);

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.
-- data cleaning 

select * 
from 
retail_sales  ;

-- deleting  rows with null data

Delete from retail_sales 
where 
	transaction_id IS NULL
    OR
    sales_date IS NULL
    OR
    sale_time IS NULL
    OR
    customer_id IS NULL
    OR
    gender IS NULL
	OR
	age IS NULL
    OR
    category IS NULL
	OR
    quantity IS NULL
    OR
    price_per_unit IS NULL
    OR
    cogs IS NULL
    OR 
    total_sale IS NULL;
    

-- Data Exploration

-- How many sales we have 
select count(*) as total_sale from retail_sales;

-- How many unique customers We Have ?
select count(distinct(customer_id)) as total_customers from retail_sales;
-- How many categories we have?
select count(distinct(category)) as categories from retail_sales;

-- Data Analysis & Business Key problem
-- Q.1 Write a SQL query to retrive all columns for sales made on 2022-11-05
select * 
from 
    retail_sales 
where 
    sale_date="2022-11-05";
        
-- Q2 2. **Write a SQL query to retrieve all transactions where the category 
-- is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:

select * from retail_sales where 
    category="clothing" 
and 
    quantity>=4
and 
date_format(sale_date, '%Y-%m') = '2022-11';

-- Q3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:

select category,sum(total_sale) as total_sales 
from retail_sales 
group by 1;

-- Q4. **Write a SQL query to find the average age of customers 
-- who purchased items from the 'Beauty' category.**:
select round(avg(age)) as avg_age 
from 
    retail_sales 
where 
        category="beauty";

-- Q5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:

select * 
from 
    retail_sales 
where 
    total_sale>1000;

-- Q6. **Write a SQL query to find the total number of transactions (transaction_id) 
-- made by each gender in each category.**:

select gender,category,count(*)  as total_transancation
from 
    retail_sales
 group by 
    category,gender;
 
 -- Q7. **Write a SQL query to calculate
 -- the average sale for each month. Find out best selling month in each year**:
 
WITH MonthlySales AS (
    SELECT YEAR(sale_date) AS year, MONTH(sale_date) AS month, ROUND(AVG(total_sale), 2) AS avg_sale
    FROM retail_sales
    GROUP BY 
        YEAR(sale_date), MONTH(sale_date)
)
SELECT year,  month, 
    avg_sale
FROM (
    SELECT year, month, avg_sale,
        RANK() OVER (PARTITION BY year ORDER BY avg_sale DESC) AS rank_sale FROM MonthlySales
) ranked_sales
WHERE rank_sale = 1
ORDER BY year, month;

-- Q8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
select customer_id,sum(total_sale) 
from 
    retail_sales 
group by 
    customer_id 
order by 
    2 desc 
limit 5;

-- q9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
 
 
 select 
    category,count(distinct(customer_id)) 
 as 
    unique_customer_count
 from 
    retail_sales 
 group by category;
 
 -- Q10. **Write a SQL query to create each shift and number of orders 
 -- (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
 select count(total_sale),
    case 
        when sale_time<"12:00" 
    then 
        "Morning Shift"
        when 
        sale_time between '12:00' and '17:00' 
    then 
        'Afternoon shift'
        when 
        sale_time>'17:00' then 'Evening shift' end as shift
from
        retail_sales 
    group by shift;
 
 
 
 
 ## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.


This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 














 
