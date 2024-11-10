# SQL Project 02: Restaurant Orders Analysis (file is attached)

**Source**: [Maven Analytics](https://mavenanalytics.io)  

## Project Overview  
This project involves analyzing restaurant order data for *Taste of the World Café*, a vibrant restaurant known for its diverse menu and generous portions. As a Data Analyst, I am tasked with assessing customer preferences and the performance of new menu items introduced at the beginning of the year.

## Project Brief  
As part of my role, I’ve been assigned to examine customer order data to determine which menu items are popular, which are underperforming, and to identify customer preferences.

## Objectives  
1. Analyze the `menu_items` table to gain insights into the new menu offerings.
2. Explore the `order_details` table to understand the available customer data.
3. Combine insights from both tables to evaluate customer responses to the new menu items.

## Let's start the project

Database: restaurant_db  
USE restaurant_db;

Objective 01: Analyze the menu_items table to gain insights into the new menu offerings

Step 1: View the full menu items table.
```sql
SELECT * 
FROM menu_items;
Step 2: Determine the total number of items on the menu
SELECT COUNT(*) AS total_items 
FROM menu_items;
-- Step 3: Identify the least and most expensive items on the menu
-- Least expensive item
SELECT *
FROM menu_items 
ORDER BY price ASC 
LIMIT 1;
-- Most expensive item
SELECT * 
FROM menu_items 
ORDER BY price DESC 
LIMIT 1;
-- Step 4: Count the number of Italian dishes on the menu
SELECT COUNT(*) AS italian_dishes
FROM menu_items
WHERE category = 'Italian';
-- Step 5: Identify the least and most expensive Italian dishes
-- Least expensive Italian dish
SELECT *
FROM menu_items 
WHERE category = 'Italian' 
ORDER BY price ASC 
LIMIT 1;
-- Most expensive Italian dish
SELECT *
FROM menu_items
WHERE category = 'Italian' 
ORDER BY price DESC
LIMIT 1;
-- Step 6: Count the number of dishes within each category
SELECT category, COUNT(menu_item_id) AS num_dishes
FROM menu_items
GROUP BY category;
-- Step 7: Calculate the average dish price within each category
SELECT category, AVG(price) AS average_price
FROM menu_items
GROUP BY category;
## Objective 02: Analysis of Orders in the order_details Table
-- 1. View the entire order_details table
SELECT *
FROM order_details;
-- 2. Determine the date range of the table.
-- Starting Date
SELECT *
FROM order_details 
ORDER BY order_date ASC 
LIMIT 1;
-- Ending Date
SELECT *
FROM order_details 
ORDER BY order_date DESC 
LIMIT 1;
-- 3. Count the number of unique orders within the date range
SELECT COUNT(DISTINCT order_id) AS total_orders
FROM order_details;
-- 4. Count the total number of items ordered within the date range
SELECT COUNT(*) AS total_items_ordered
FROM order_details;
-- 5. Identify orders with the highest number of items
SELECT order_id, COUNT(item_id) AS num_items
FROM order_details
GROUP BY order_id 
ORDER BY num_items DESC;
-- 6. Count the number of orders that had more than 12 items
SELECT COUNT(*) AS orders_with_more_than_12_items
FROM (
    SELECT order_id, COUNT(item_id) AS num_items 
    FROM order_details
    GROUP BY order_id 
    HAVING num_items > 12
) AS large_orders;
Objective 3: Analyze Customer Behavior
-- 1. Combine the menu_items and order_details tables into a single table
SELECT *
FROM order_details od 
LEFT JOIN menu_items mi 
ON od.item_id = mi.menu_item_id;
-- 2. What were the least and most ordered items? What categories were they in?
-- Least ordered item
SELECT item_name, category, COUNT(order_details_id) AS num_purchases 
FROM order_details od
LEFT JOIN menu_items mi ON od.item_id = mi.menu_item_id 
GROUP BY item_name, category
ORDER BY num_purchases ASC 
LIMIT 1;
-- Most ordered item
SELECT item_name, category, COUNT(order_details_id) AS num_purchases 
FROM order_details od
LEFT JOIN menu_items mi ON od.item_id = mi.menu_item_id 
GROUP BY item_name, category
ORDER BY num_purchases DESC 
LIMIT 1;
-- 3. What were the top 5 orders that spent the most money?
SELECT order_id, SUM(price) AS total_spend
FROM order_details od
LEFT JOIN menu_items mi ON od.item_id = mi.menu_item_id 
GROUP BY order_id
ORDER BY total_spend DESC 
LIMIT 5;
-- 4. View the details of the highest spend order. What insights can you gather from the results?
SELECT category, COUNT(item_id) AS num_items
FROM order_details od
LEFT JOIN menu_items mi ON od.item_id = mi.menu_item_id 
WHERE order_id = 440
GROUP BY category;
-- 5. View the details of the top 5 highest spend orders. What insights can you gather from the results?
SELECT order_id, category, COUNT(item_id) AS num_items
FROM order_details od
LEFT JOIN menu_items mi ON od.item_id = mi.menu_item_id
WHERE order_id IN ('440', '2075', '1957', '330', '2675') 
GROUP BY order_id, category;
