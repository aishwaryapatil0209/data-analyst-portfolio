
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

1.Top Categories by Total Sales

SELECT 
  category, 
  ROUND(SUM(sales), 2) AS total_sales
FROM ecommerce_orders
GROUP BY category
ORDER BY total_sales DESC
LIMIT 5;

2.Return Rate Trends Across Product Categories

SELECT 
    category,
    COUNT(CASE WHEN return_flag = 'Y' THEN 1 END) AS total_returns,
    COUNT(*) AS total_orders,
    ROUND(
        COUNT(CASE WHEN return_flag = 'Y' THEN 1 END) * 100.0 / COUNT(*), 
        2
    ) AS return_rate_percent
FROM 
    orders
GROUP BY 
    category
ORDER BY 
    return_rate_percent DESC;
    
Purpose: To find which product categories have the highest return rates.

Insights: Helps identify categories with quality or satisfaction issues and supports product improvement decisions.

3. Regional Revenue vs. Return Comparison

SELECT 
    region,
    ROUND(SUM(CASE WHEN return_flag = 'N' THEN quantity * unit_price ELSE 0 END), 2) AS net_revenue,
    COUNT(CASE WHEN return_flag = 'Y' THEN 1 END) AS return_count,
    ROUND(
        COUNT(CASE WHEN return_flag = 'Y' THEN 1 END) * 100.0 / COUNT(*), 
        2
    ) AS return_rate_percent
FROM 
    orders
GROUP BY 
    region
ORDER BY 
    net_revenue DESC;
    
Purpose: Compare regions with high sales vs. regions with high return rates.

Insights: Useful for identifying operational or fulfillment issues in specific regions.

4. Repeat Purchase Patterns by Customer

WITH customer_activity AS (
    SELECT 
        customer_id,
        COUNT(DISTINCT order_id) AS total_orders,
        MIN(order_date) AS first_purchase,
        MAX(order_date) AS last_purchase
    FROM 
        orders
    GROUP BY 
        customer_id
)

SELECT 
    customer_id,
    total_orders,
    DATEDIFF(day, first_purchase, last_purchase) AS active_days,
    CASE 
        WHEN total_orders >= 3 THEN 'Repeat Buyer'
        ELSE 'One-Time/Occasional Buyer'
    END AS purchase_behavior
FROM 
    customer_activity
ORDER BY 
    total_orders DESC;
    
Purpose: Segment users by repeat purchase behavior and analyze customer lifecycle.

Insights: Helps target campaigns for customer retention and loyalty.


