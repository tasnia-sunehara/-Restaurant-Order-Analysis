# 📊 Restaurant Order Analysis (SQL Project)

## 📝 Project Overview
This project explores customer interactions with a restaurant's menu using SQL. By analyzing menu items, pricing, and order patterns, it uncovers insights into:  

- 🍽️ Most and least popular dishes  
- 💰 Pricing trends across categories  
- 📅 Customer ordering behavior over time  
- 🏆 Highest-spending orders and detailed item breakdowns  

The analysis provides actionable insights for optimizing the menu, improving sales, and understanding customer preferences.

---

## 🛠️ Skills & Tools
- **SQL**: `JOIN`, `GROUP BY`, aggregate functions (`SUM`, `AVG`, `COUNT`), subqueries, filtering, date exploration  

---

## 🔍 Key Analyses

### 🍴 Menu Analysis
- Total number of menu items  
- Least and most expensive dishes  
- Category breakdown: total items and average price  
- Italian dishes: count, least expensive, most expensive  

### 🧾 Order Analysis
- Order date range  
- Total orders and total items  
- Orders with the highest number of items  
- Orders with more than 12 items  

### 💵 Sales Analysis
- Least and most ordered items with categories  
- Top 5 highest-spending orders  
- Detailed item breakdown of top orders  

---

## 📂 SQL Queries

```sql
-- Use the database
USE restaurant_db;

-- 1️⃣ Total number of menu items

SELECT COUNT(menu_item_id) AS Total_Number_of_items
FROM menu_items;

-- 2️⃣ Least and most expensive items

 -- Most expensive

SELECT item_name, price
FROM menu_items
ORDER BY price DESC;

-- Least expensive

SELECT item_name, price
FROM menu_items
ORDER BY price ASC;

 

-- 3️⃣ Italian dishes analysis

SELECT COUNT(*) AS Total_Italian_dishes
FROM menu_items
WHERE category = 'Italian';

-- Least expensive Italian dishes

SELECT item_name, price
FROM menu_items
WHERE category = 'Italian'
ORDER BY price ASC
LIMIT 3;  

 -- Most expensive Italian dishes

SELECT item_name, price
FROM menu_items
WHERE category = 'Italian'
ORDER BY price DESC
LIMIT 3; 

-- 4️⃣ Dishes per category + average price

SELECT category,
       COUNT(*) AS total_dish_categories,
       ROUND(AVG(price), 2) AS average_price
FROM menu_items
GROUP BY category;

-- 5️⃣ Order date range

SELECT MIN(order_date) AS starting_date,
       MAX(order_date) AS ending_date
FROM order_details;

-- 6️⃣ Total orders and items

SELECT COUNT(DISTINCT order_id) AS total_orders,
       COUNT(*) AS total_items
FROM order_details;

-- 7️⃣ Orders with most items

SELECT order_id,
       COUNT(item_id) AS most_number_of_items
FROM order_details
GROUP BY order_id
ORDER BY most_number_of_items DESC;

-- 8️⃣ Orders with more than 12 items

SELECT COUNT(DISTINCT order_id) AS total_orders
FROM order_details
WHERE order_id IN (
    SELECT order_id
    FROM order_details
    GROUP BY order_id
    HAVING COUNT(item_id) > 12
);

-- 9️⃣ Combine menu_items and order_details

SELECT *
FROM menu_items
JOIN order_details ON item_id = menu_item_id;

-- 🔟 Least and most ordered items

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

-- 1️⃣1️⃣ Top 5 highest-spending orders

SELECT order_id, SUM(price) AS total_spend
FROM menu_items
JOIN order_details ON item_id = menu_item_id
GROUP BY order_id
ORDER BY total_spend DESC
LIMIT 5;

-- 1️⃣2️⃣ Highest spend order details

SELECT category, item_name, COUNT(item_id) AS total_item
FROM menu_items
JOIN order_details ON item_id = menu_item_id
WHERE order_id = 440
GROUP BY category, item_name;

-- 1️⃣3️⃣ Top 5 highest spend orders breakdown

SELECT order_id, category, COUNT(item_id) AS total_items, SUM(price) AS total_spend
FROM menu_items
JOIN order_details ON item_id = menu_item_id
WHERE order_id IN (440, 2075, 1957, 330, 2675)
GROUP BY order_id, category;
