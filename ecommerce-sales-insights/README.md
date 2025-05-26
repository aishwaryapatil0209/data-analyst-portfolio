
** E-Commerce Sales Insights **  
Tech Stack: Databricks | Spark SQL | Excel   

Project Overview  
Analyzed over 1 million rows of e-commerce order data using Spark SQL in Databricks to uncover revenue trends, top-performing categories, return issues, and customer behavior. Delivered findings through SQL scripts and Excel dashboards.

---

 Dataset  
- Simulated e-commerce order dataset  
- Columns: `Order ID`, `Customer ID`, `Product`, `Category`, `Sales`, `Quantity`, `Order Date`, `Return Status`, etc.

---

 Key Tools & Techniques  
- Spark SQL on Databricks (CTEs, Window Functions, Aggregations)
- Excel Dashboards: PivotTables, Slicers, Conditional Formatting
- Performance Optimization: Filter pushdown, partitioning

---

 Insights Generated  
- Top 5 Categories by Sales 
- Return Rate Trends across product categories  
- Regional Performance (highest revenue vs. highest returns)  
- Repeat Purchase Patterns using customer order history  

---

Top Categories by Total Sales
```sql
SELECT 
  category, 
  ROUND(SUM(sales), 2) AS total_sales
FROM ecommerce_orders
GROUP BY category
ORDER BY total_sales DESC
LIMIT 5;
