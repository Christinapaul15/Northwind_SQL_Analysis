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

---

## 🛠️ Tools & Technologies
- **SQL** (PostgreSQL)
- **Jupyter Notebook**
- **Northwind Sample Database**
- **Python** (for query execution and analysis)
- **CTEs**, **Window Functions**, **Joins**, **Aggregations**, **CASE statements**

---

## 🧩 Key Concepts Applied

1. **Common Table Expressions (CTEs)** — To simplify multi-step logic into modular queries.  
2. **Window Functions** — For calculating running totals, rankings, and growth rates.  
3. **Aggregations & Grouping** — To summarize key business metrics.  
4. **Data Cleaning & Joins** — To combine data across multiple related tables.

---

## 📈 Example Query — Monthly Sales & Running Total

The following query calculates **monthly sales** and a **running total** across months using the `SUM()` function with an `OVER()` clause:

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
