# Retail_Sales_project1
Retail Sales Data Analysis using SQL Server | Data Cleaning, Aggregation, and Business Insights.
# 🛒 Retail Sales Analysis – SQL Project  

## 📌 Problem Statement  
Retail businesses collect huge amounts of transactional data daily.  
But the raw data often has:  
- Null or missing values  
- Wrong column names (e.g., `quantiy` instead of `quantity`)  
- Time formats not ready for analysis  

This makes it hard to analyze sales, customers, and product trends.  

---

## 🔎 Approach  
1. **Data Cleaning** – fixed column names, removed nulls, formatted time.  
2. **Exploration** – counted total sales, customers, categories.  
3. **Analysis** – solved key business problems using SQL.  
4. **Insights** – generated solutions for management decisions.  

---

    -- Retail Sales SQL Analysis Project

     -- Change sales_time format
ALTER TABLE Retail_Sales
ALTER COLUMN Sale_time TIME(0);

     -- change column name quantiy to quantity
EXEC SP_RENAME 'Retail_Sales.quantiy' ,'quantity','column';

      -- How many records in my table
SELECT count(*) total_data FROM Retail_Sales;

     -- Data cleaning:
       --check and delete null values
SELECT * FROM Retail_Sales
WHERE transactions_id IS NULL 
      OR sale_date IS NULL OR sale_time IS NULL
      OR customer_id IS NULL OR gender IS NULL
      OR category IS NULL OR quantity IS NULL
      OR price_per_unit IS NULL OR cogs IS NULL OR total_sale IS NULL;

DELETE FROM Retail_Sales
WHERE transactions_id IS NULL 
      OR sale_date IS NULL OR sale_time IS NULL
      OR customer_id IS NULL OR gender IS NULL
      OR category IS NULL OR quantity IS NULL
      OR price_per_unit IS NULL OR cogs IS NULL OR total_sale IS NULL; 

    -- Data Exploration:
       --Count total records
SELECT count(*) AS Total_Sales FROM Retail_Sales;

      --Count unique customers
SELECT COUNT(distinct(customer_id)) AS total_customers FROM Retail_Sales;

      --Count distinct category
SELECT DISTINCT(category) FROM Retail_Sales;

      --Find total sales
SELECT SUM(total_sale) AS Total_Sales FROM Retail_Sales;

      --Find Category wise Sales
SELECT category, SUM(total_sale) AS Total_Sales FROM Retail_Sales GROUP BY category;

      --Find Category wise total quantity Sales
SELECT category, SUM(quantity) AS Total_Qty_Sale FROM Retail_Sales GROUP BY category;



      -- Business Problems and Solutions
      -- Q1 Write an query to retrieve all columns for sales made on '2022-11-05' .
SELECT * FROM Retail_Sales 
WHERE sale_date = '2022-11-05';

    -- Q2 Write sql query to retrieve all transactions where the category is clothing and quantity sold is more than 4 in month of nov-2022.
SELECT * FROM Retail_Sales
WHERE category = 'Clothing' AND quantity > 2 
         AND sale_date BETWEEN '2022-11-01' AND '2022-11-30'
ORDER BY sale_date ASC;

    -- Q3  How much Sales by Category are there ?
SELECT category, SUM(total_sale) AS Total_Sales, 
       SUM(quantity) AS Total_orders 
FROM Retail_Sales 
GROUP BY category;

    -- Q4 Write a query to find the avg. age of all customers who purchased from beauty category.
SELECT AVG(age) AS avg_age 
FROM Retail_Sales 
WHERE category = 'beauty';

    -- Q5 Write a sql query to find all transactions where the total sale is greater than 1000.
SELECT * 
FROM Retail_Sales 
WHERE total_sale > 1000;

     -- Q6 Write a sql query to find the total number of  transactions made by each gender in each category .
SELECT category, gender, 
       COUNT(transactions_id) AS total_no_of_transaction 
FROM Retail_Sales
GROUP BY category, gender;

    -- Q7 Write a sql query to find the average sales for each month . find out best selling month in each year .
SELECT MONTH(Sale_date) AS month, 
      AVG(total_sale) AS average_sales 
FROM Retail_Sales GROUP BY MONTH(Sale_date) 
ORDER BY MONTH(Sale_date);

    -- best sale month in each year
SELECT Year, month, total_sales FROM(
    SELECT YEAR(sale_date) AS Year, MONTH(Sale_date) AS month, SUM(total_sale) AS total_sales,
           RANK() OVER(PARTITION BY YEAR(sale_date) ORDER BY SUM(total_sale) DESC) AS RANK
    FROM Retail_Sales
    GROUP BY YEAR(sale_date), MONTH(sale_date)
) AS T1 WHERE RANK = 1;

    -- Q8 Write a sql query to find the top 5 customers based on highest total Sales .
SELECT TOP 5 customer_id, SUM(total_sale) AS Total_Sales
FROM Retail_Sales 
GROUP BY customer_id 
ORDER BY SUM(total_sale) DESC;

    -- Q9 Write a sql query to find unique customers who purchased from all category .
SELECT customer_id, COUNT(DISTINCT(category)) AS category_count,
       CASE 
          WHEN COUNT(DISTINCT(category)) = 3 THEN 'YES' 
          ELSE 'NO' 
       END AS Purchased_from_all_ctgry_status
FROM Retail_Sales 
GROUP BY customer_id 
ORDER BY customer_id ASC;

      -- Q10 Write a sql query to find total no. of order in each shift(morning shift <= 12 , afternoon shift 12 < and 17 <= , evening shift 17<)
SELECT Shift, COUNT(Shift) AS Total_orders FROM(
    SELECT CASE
        WHEN DATEPART(HOUR, sale_time) <= 12 THEN 'morning'
        WHEN DATEPART(HOUR, sale_time) > 12 AND DATEPART(HOUR, sale_time) <= 17 THEN 'afternoon'
        ELSE 'evening' END AS Shift
    FROM Retail_Sales
) AS T1 GROUP BY Shift;


## 📈 Final Insights  
- **Beauty category** generated the highest sales.  
- **Top 5 customers** contributed a large share of revenue.  
- **November** was the best-performing month.  
- **Evening shift** had the maximum orders.  

These insights can help in **marketing campaigns, staff planning, and stock management**.  

---

## ⚙️ Tech Stack  
- **SQL Server**  
- SQL Queries for cleaning, exploration, and analysis  

---

## 🚀 How to Run  
1. Import the dataset into SQL Server.  
2. Run `retail_sales_project.sql`.  
3. Explore insights from queries.  

---

✍️ **Author**: Rohit Kumar Vishwakarma  
