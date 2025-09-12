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

## 🧹 Data Cleaning  

### ✅ Fixing Column Name  
```sql
EXEC SP_RENAME 'Retail_Sales.quantiy', 'quantity', 'COLUMN';
```

### ✅ Fixing Time Format  
```sql
ALTER TABLE Retail_Sales
ALTER COLUMN sale_time TIME(0);
```

### ✅ Handling Null Values  
```sql
DELETE FROM Retail_Sales
WHERE transactions_id IS NULL 
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

---

## 📊 Business Questions & Solutions  

### 1️⃣ How many sales and unique customers?  
```sql
SELECT COUNT(*) AS total_sales FROM Retail_Sales;
SELECT COUNT(DISTINCT customer_id) AS total_customers FROM Retail_Sales;
```

### 2️⃣ Sales by Category  
```sql
SELECT category, SUM(total_sale) AS Total_Sales
FROM Retail_Sales
GROUP BY category;
```

### 3️⃣ Top 5 Customers by Sales  
```sql
SELECT TOP 5 customer_id, SUM(total_sale) AS Total_Sales
FROM Retail_Sales
GROUP BY customer_id
ORDER BY SUM(total_sale) DESC;
```

### 4️⃣ Best-Selling Month in Each Year  
```sql
SELECT year, month, total_sales
FROM (
   SELECT YEAR(sale_date) AS year,
          MONTH(sale_date) AS month,
          SUM(total_sale) AS total_sales,
          RANK() OVER(PARTITION BY YEAR(sale_date) ORDER BY SUM(total_sale) DESC) AS rnk
   FROM Retail_Sales
   GROUP BY YEAR(sale_date), MONTH(sale_date)
) t
WHERE rnk = 1;
```

### 5️⃣ Sales by Shift (Morning/Afternoon/Evening)  
```sql
SELECT shift, COUNT(*) AS total_orders
FROM (
   SELECT CASE
            WHEN DATEPART(HOUR, sale_time) <= 12 THEN 'Morning'
            WHEN DATEPART(HOUR, sale_time) > 12 AND DATEPART(HOUR, sale_time) <= 17 THEN 'Afternoon'
            ELSE 'Evening'
          END AS shift
   FROM Retail_Sales
) t
GROUP BY shift;
```

---

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
