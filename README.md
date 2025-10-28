# Northwind SQL Analysis

This project explores business insights using the **Northwind** sample database, focusing on sales trends, customer performance, and product category analysis.  
It was built as part of my Data Analysis learning journey to strengthen my SQL and analytical problem-solving skills.

---

## 📊 Project Overview

The goal of this project was to apply **intermediate to advanced SQL techniques** to extract meaningful insights from a relational database.  
The queries answer real-world business questions such as:

- What are the total sales and growth trends month-over-month?
- Which employees and customers contribute the most to revenue?
- Which product categories and products generate the highest sales?
- What percentage of total sales does each category represent?
- How do individual orders compare to the overall average?

---

## 🛠️ Tools & Technologies
- **SQL** (PostgreSQL syntax)
- **Jupyter Notebook**
- **Northwind Sample Database**
- **Python** (via Jupyter for SQL execution)
- **CTEs**, **Window Functions**, **Joins**, **Aggregations**, **CASE statements**

---

## 🧩 Key Concepts Applied

1. **Common Table Expressions (CTEs)**  
   Simplified complex queries and modularized logic for better readability.

2. **Window Functions**  
   Used `ROW_NUMBER()`, `RANK()`, `LAG()`, and `SUM() OVER()` to calculate rankings, growth trends, and running totals.

3. **Data Aggregation & Categorization**  
   Calculated KPIs like total revenue, percentage contribution, and average order values.

4. **Performance Insights**  
   Identified top-performing employees, customers, and products based on sales.

---

## 🧾 Example Highlights

- **Monthly Sales Trends**
  ```sql
  SELECT 
    DATE_TRUNC('month', o.order_date) AS month,
    SUM(od.unit_price * od.quantity) AS monthly_sales,
    SUM(SUM(od.unit_price * od.quantity)) OVER (ORDER BY DATE_TRUNC('month', o.order_date)) AS running_total
FROM 
    orders AS o
JOIN 
    order_details AS od
    ON o.order_id = od.order_id
GROUP BY 
    DATE_TRUNC('month', o.order_date)
ORDER BY 
    month; 
Top 3 Products by Category

sql
Copy code
WITH product_sales AS (
  SELECT category_id, product_id, SUM(quantity * unit_price) AS total_sales
  FROM products
  JOIN order_details USING(product_id)
  GROUP BY category_id, product_id
)
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY category_id ORDER BY total_sales DESC) AS rank
  FROM product_sales
) ranked
WHERE rank <= 3;
