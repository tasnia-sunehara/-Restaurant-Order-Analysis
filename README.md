#                                                                                      📊 Restaurant Order Analysis (SQL Project)

## 📝 Project Description
This project analyzes customer interactions with a restaurant's menu using SQL. It explores menu items, pricing, and order patterns to identify trends in customer behavior. By combining the menu and order tables, the analysis highlights:  
- Most and least popular dishes  
- Pricing patterns across categories  
- Customer ordering behavior over time  
- Highest-spending orders and detailed item breakdowns  

This analysis helps the restaurant understand which items drive revenue and optimize their offerings.

---

## 🛠️ Skills & Tools
- **SQL**: `JOIN`, `GROUP BY`, aggregate functions, `COUNT`, `SUM`, `AVG`, subqueries, filtering, date exploration

---

## 🔍 Analysis Overview

### Menu Analysis
- Total number of menu items
- Least and most expensive items
- Category breakdown: total items and average price
- Italian dishes: count, least expensive, most expensive

### Order Analysis
- Order date range
- Total orders and total items
- Orders with the highest number of items
- Orders with more than 12 items

### Sales Analysis
- Least and most ordered items with categories
- Top 5 highest-spending orders
- Item breakdown of top orders

---

## 📂 SQL Queries

```sql
-- Use the database
USE restaurant_db;

-- 1. Total number of menu items
SELECT COUNT(menu_item_id) AS Total_Number_of_items
FROM menu_items;

-- 2. Least and most expensive items
SELECT item_name, price
FROM menu_items
ORDER BY price DESC;  -- Most expensive
SELECT item_name, price
FROM menu_items
ORDER BY price ASC;   -- Least expensive

-- 3. Italian dishes analysis
SELECT COUNT(*) AS Total_Italian_dishes
FROM menu_items
WHERE category = 'Italian';

SELECT item_name, price
FROM menu_items
WHERE category = 'Italian'
ORDER BY price ASC
LIMIT 3;  -- Least expensive Italian dishes

SELECT item_name, price
FROM menu_items
WHERE category = 'Italian'
ORDER BY price DESC
LIMIT 3;  -- Most expensive Italian dishes

-- 4. Dishes per category + average price
SELECT category,
       COUNT(*) AS total_dish_categories,
       ROUND(AVG(price), 2) AS average_price
FROM menu_items
GROUP BY category;

-- 5. Order date range
SELECT MIN(order_date) AS starting_date,
       MAX(order_date) AS ending_date
FROM order_details;

-- 6. Total orders and items
SELECT COUNT(DISTINCT order_id) AS total_orders,
       COUNT(*) AS total_items
FROM order_details;

-- 7. Orders with most items
SELECT order_id,
       COUNT(item_id) AS most_number_of_items
FROM order_details
GROUP BY order_id
ORDER BY most_number_of_items DESC;

-- 8. Orders with more than 12 items
SELECT COUNT(DISTINCT order_id) AS total_orders
FROM order_details
WHERE order_id IN (
    SELECT order_id
    FROM order_details
    GROUP BY order_id
    HAVING COUNT(item_id) > 12
);

-- 9. Combine menu_items and order_details
SELECT *
FROM menu_items
JOIN order_details ON item_id = menu_item_id;

-- 10. Least and most ordered items
SELECT category, item_name, COUNT(item_id) AS least_ordered
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY category, item_name
ORDER BY least_ordered ASC;

SELECT category, item_name, COUNT(item_id) AS most_ordered
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY category, item_name
ORDER BY most_ordered DESC;

-- 11. Top 5 highest-spending orders
SELECT order_id, SUM(price) AS total_spend
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY order_id
ORDER BY total_spend DESC
LIMIT 5;

-- 12. Highest spend order details (example: order_id = 440)
SELECT category, item_name, COUNT(item_id) AS total_item
FROM menu_items
JOIN order_details ON item_id = menu_item_id
WHERE order_id = 440
GROUP BY category, item_name;

-- 13. Top 5 highest spend orders breakdown
SELECT order_id, category, COUNT(item_id) AS total_items, SUM(price) AS total_spend
FROM menu_items
JOIN order_details ON item_id = menu_item_id
WHERE order_id IN (440, 2075, 1957, 330, 2675)
GROUP BY order_id, category;
