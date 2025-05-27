This project demonstrates how to analyze customer ordering behavior using Snowflake's virtual warehouse. The objective is to identify high-return regions, evaluate repeat purchase patterns, and uncover key metrics that help understand customer lifetime value and product-level trends.

Key Business Questions
What are the return rate trends across different product categories?
Which regions generate the highest revenue, and which face the highest returns?
Are there repeat customers? What is their ordering behavior like?

Tools & Technologies
Snowflake Data Warehouse
SQL

 Visual Insight
Regional Revenue vs Returns


Sample SQL Queries
1. Return Rate Trends by Product Category

SELECT 
    product_category,
    COUNT(order_id) AS total_orders,
    SUM(CASE WHEN is_returned = 1 THEN 1 ELSE 0 END) AS total_returns,
    ROUND(SUM(CASE WHEN is_returned = 1 THEN 1 ELSE 0 END) * 100.0 / COUNT(order_id), 2) AS return_rate_percentage
FROM orders
GROUP BY product_category
ORDER BY return_rate_percentage DESC;

2. Regional Performance (Revenue vs Returns)

SELECT 
    region,
    ROUND(SUM(order_amount), 2) AS total_revenue,
    ROUND(SUM(CASE WHEN is_returned = 1 THEN order_amount ELSE 0 END), 2) AS total_returns
FROM orders
GROUP BY region
ORDER BY total_revenue DESC;

3. Repeat Purchase Patterns

SELECT 
    customer_id,
    COUNT(DISTINCT order_id) AS total_orders,
    MIN(order_date) AS first_order_date,
    MAX(order_date) AS last_order_date,
    DATEDIFF('day', MIN(order_date), MAX(order_date)) AS days_between_orders
FROM orders
GROUP BY customer_id
HAVING COUNT(DISTINCT order_id) > 1
ORDER BY total_orders DESC;
