🍽️ Restaurant Order Analysis (SQL Project)

Project Overview

This repository presents a comprehensive SQL-based data analysis project focused on understanding customer behavior and sales performance for a restaurant's menu. The analysis leverages relational database concepts, including JOIN operations, aggregate functions, and subqueries, to explore menu pricing, item popularity, and customer spending patterns.

The primary goal is to provide actionable, data-driven insights into:

•
Which menu items and categories drive the highest revenue.

•
How customer ordering choices influence overall business performance.

•
Identifying high-value orders and the items they contain.




🛠️ Technology Stack

The entire analysis was conducted using SQL (specifically, a relational database like MySQL or PostgreSQL).

Component
Technology/Skill
Key Concepts Used
Database Language
SQL
SELECT, FROM, WHERE, GROUP BY, ORDER BY
Data Manipulation
SQL Joins
JOIN (connecting menu_items and order_details)
Statistical Analysis
Aggregate Functions
COUNT(), SUM(), AVG(), MIN(), MAX()
Advanced Querying
Subqueries, Filtering
HAVING, IN clause, Date Exploration (MIN/MAX dates)





📈 Analysis Objectives

The following key questions were addressed through the SQL queries:

Menu Structure and Pricing

1.
Menu Item Count: Determine the total number of unique items available on the menu.

2.
Price Extremes: Identify the least and most expensive dishes on the menu.

3.
Category Breakdown: Analyze the distribution of dishes across different categories (e.g., Italian, Asian, etc.) and calculate the average price within each category.

Order Volume and Popularity

1.
Order Date Range: Establish the time frame covered by the order data.

2.
Total Volume: Calculate the total number of orders and the total number of items sold.

3.
Item Popularity: Identify the most and least frequently ordered menu items and categories.

Customer Spending and High-Value Orders

1.
Order Size: Determine the orders with the highest number of items, and quantify how many orders exceeded a specific item count (e.g., 12 items).

2.
Top Spenders: Identify the Top 5 highest-spending orders based on total price.

3.
High-Value Itemization: Provide a detailed breakdown of the items and categories purchased within the highest-spending orders.




💻 SQL Queries

The complete set of SQL queries used for this analysis is provided below, structured by the analysis objective they address.

Menu Structure and Pricing

SQL


-- 1. Total number of menu items
SELECT COUNT(menu_item_id) AS Total_Number_of_items
FROM menu_items;

-- 2. Least and most expensive items on the menu
SELECT item_name, price
FROM menu_items
ORDER BY price DESC
LIMIT 1; -- Most expensive

SELECT item_name, price
FROM menu_items
ORDER BY price ASC
LIMIT 1; -- Least expensive

-- 3. Dishes per category + average price
SELECT category,
       COUNT(category) AS total_dish_categories,
       ROUND(AVG(price), 2) AS average_price
FROM menu_items
GROUP BY category
ORDER BY average_price DESC;


Order Volume and Popularity

SQL


-- 4. Date range of order_details table
SELECT MIN(order_date) AS starting_date,
       MAX(order_date) AS ending_date
FROM order_details;

-- 5. Total orders + total items
SELECT COUNT(DISTINCT order_id) AS total_orders,
       COUNT(*) AS total_items_sold
FROM order_details;

-- 6. Least and most ordered items (by category and item name)
-- Most Ordered
SELECT category, item_name, COUNT(item_id) AS order_count
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY category, item_name
ORDER BY order_count DESC;

-- Least Ordered
SELECT category, item_name, COUNT(item_id) AS order_count
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY category, item_name
ORDER BY order_count ASC;


Customer Spending and High-Value Orders

SQL


-- 7. Orders with the most number of items
SELECT order_id, COUNT(item_id) AS item_count
FROM order_details
GROUP BY order_id
ORDER BY item_count DESC;

-- Count of orders with more than 12 items
SELECT COUNT(DISTINCT(order_id)) AS total_large_orders
FROM order_details
WHERE order_id IN (
    SELECT order_id
    FROM order_details
    GROUP BY order_id
    HAVING COUNT(item_id) > 12
);

-- 8. Top 5 highest spend orders
SELECT order_id, SUM(price) AS total_spend
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY order_id
ORDER BY total_spend DESC
LIMIT 5;

-- 9. Details of the top 5 highest spend orders (Example: Order ID 440)
-- Note: The full query would use the top 5 IDs found in the previous step.
SELECT order_id, category, item_name, COUNT(item_id) AS quantity, SUM(price) AS item_revenue
FROM menu_items
JOIN order_details ON item_id = menu_item_id
WHERE order_id IN (440, 2075, 1957, 330, 2675) -- Example IDs from analysis
GROUP BY order_id, category, item_name
ORDER BY order_id, category;


