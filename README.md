# 📊-Restaurant-Order-Analysis (SQL Project)

# This project focuses on understanding how a restaurant’s customers interact with its menu. Using SQL, I reviewed the full menu, explored pricing, and analyzed how often each item was ordered. I also examined order patterns over time such as how many items customers typically purchased and which orders resulted in the highest spending. By joining the menu and order tables, I was able to connect each purchase back to specific dishes and categories. The results highlight which items drive sales, which ones are less popular, and how customer choices shape overall business performance.

# 🛠️ Skills Used : SQL (JOINs, GROUP BY, aggregate functions, subqueries, filtering, date exploration)

# 🔍 What the Analysis Covers

Total number of menu items

Least and most expensive dishes

Breakdown of dishes by category

Average price within each category

Order date range and total order volume

Orders with the highest number of items

Most and least ordered menu items

Top 5 highest-spending orders

Detailed breakdown of items within top orders

# 🛠️ Skills Used : SQL (JOINs, GROUP BY, aggregate functions, subqueries, filtering, date exploration)


SQL (JOINs, GROUP BY, aggregate functions, subqueries, filtering, date exploration)

-- Restaurant Order Analysis

-- 1. View the menu_items table and write a query to find the
--    number of items on the menu
USE restaurant_db;

SELECT * 
FROM menu_items;

SELECT COUNT(menu_item_id) AS Total_Number_of_items
FROM menu_items;


-- 2. Least and most expensive items on the menu
SELECT item_name, price AS most_expensive_item
FROM menu_items
ORDER BY price DESC;

SELECT item_name, price AS least_expensive_item
FROM menu_items
ORDER BY price ASC;


-- 3. Italian dishes: count, least expensive, most expensive
SELECT COUNT(category) AS Total_Italian_dishes
FROM menu_items
WHERE category = 'Italian';

SELECT item_name AS least_expensive_Italian_dishes, price
FROM menu_items
WHERE category = 'Italian'
ORDER BY price ASC
LIMIT 3;

SELECT item_name AS most_expensive_Italian_dishes, price
FROM menu_items
WHERE category = 'Italian'
ORDER BY price DESC
LIMIT 3;


-- 4. Dishes per category + average price
SELECT category,
       COUNT(category) AS total_dish_categories,
       ROUND(AVG(price), 2) AS average_price
FROM menu_items
GROUP BY category;


-- 5. Date range of order_details table
SELECT MIN(order_date) AS starting_date,
       MAX(order_date) AS ending_date
FROM order_details;


-- 6. Total orders + total items
SELECT COUNT(DISTINCT order_id) AS total_orders,
       COUNT(*) AS total_items
FROM order_details;


-- 7. Orders with the most number of items
SELECT order_id,
       COUNT(item_id) AS Most_number_of_items
FROM order_details
GROUP BY order_id
ORDER BY COUNT(item_id) DESC;


-- 8. How many orders had more than 12 items?
SELECT COUNT(DISTINCT(order_id)) AS total_orders
FROM order_details
WHERE order_id IN (
    SELECT order_id
    FROM order_details
    GROUP BY order_id
    HAVING COUNT(item_id) > 12
);


-- 9. Combine menu_items and order_details tables
SELECT *
FROM menu_items
JOIN order_details ON item_id = menu_item_id;


-- 10. Least and most ordered items + categories
SELECT category,
       item_name,
       COUNT(item_id) AS least_ordered
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY category, item_name
ORDER BY COUNT(item_id) ASC;

SELECT category,
       item_name,
       COUNT(item_id) AS most_ordered
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY category, item_name
ORDER BY COUNT(item_id) DESC;


-- 11. Top 5 highest spend orders
SELECT order_id,
       SUM(price) AS total_spend
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY order_id
ORDER BY total_spend DESC
LIMIT 5;


-- 12. Details of the highest spend order (order_id = 440)
SELECT category,
       COUNT(item_id) AS total_item,
       item_name
FROM menu_items
JOIN order_details ON item_id = menu_item_id
WHERE order_id = 440
GROUP BY category, item_name;


-- 13. Details of the top 5 highest spend orders
SELECT order_id,
       SUM(price) AS total_spend,
       category,
       COUNT(item_id)
FROM menu_items
JOIN order_details ON item_id = menu_item_id
WHERE order_id IN (440, 2075, 1957, 330, 2675)
GROUP BY order_id, category;
