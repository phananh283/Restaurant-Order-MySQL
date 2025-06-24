**PROJECT BRIEF**
- Background: There is a restaurant named "The Taste of the World Cafe", which has diverse menu offerings and serves generous portions. The restaurant debuted a new menu at the start of the year, so they want to dig into the customer data to see which menu items are doing well/ not well and what the top customers seem to like best.
- Objectives:
  1) Explore the menu_items table to get an idea of what's on the new menu.
  2) Explore the order_details table to get an idea of the data that's been collected.
  3) Use both tables to understand how customers are reacting to the new menu.

**THE DATASET**
- File type: MySQL
- 2 tables: menu_items, order_details
- 12,266 records
- 8 fields

**OBJECTIVE 1:** To understand the items tables by finding the least and most expensive items, and the item prices within each category

**1) The least expensive and the most expensive item:**
   - Scripting:
     SELECT item_name, price
     FROM 
	    (SELECT item_name, price
      FROM menu_items
      ORDER BY price DESC
	    LIMIT 1
    ) AS most_expensive

     UNION
 
     SELECT item_name, price
     FROM 
	    (SELECT item_name, price
      FROM menu_items
      ORDER BY price ASC
	    LIMIT 1
     ) AS least_expensive;

   - Explanation: created a table of the least expensive item and another table of the most expensive item, then combined the 2 tables by using "UNION"
   - Output:
     ![image](https://github.com/user-attachments/assets/daeefbdf-3493-4ef3-8f61-f9401fede242)

**2) How many Italian dishes are on the menu? What are the least and most expensive Italian dishes on the menu?**
   - Scripting:
     SELECT item_name, count(menu_item_id) as numbers_of_dishes
     FROM menu_items
     WHERE category = 'Italian'
     GROUP BY menu_item_id;

    - Explanation: As there are many other categories like Asian, American, etc., we needed to use "WHERE" to specify the category of "Italian"
    - Output: 
    ![image](https://github.com/user-attachments/assets/366699d8-0081-42c2-8fae-470aef1cf40c)
    

  
    
**3) How many dishes are in each category? What is the average dish price within each category?**
   - Scripting:
     SELECT 
	    category,
      COUNT(menu_item_id) AS number_of_dishes,
      ROUND(AVG(price),2) AS average_price
    FROM menu_items
    WHERE category IN ('American', 'Asian', 'Mexican', 'Italian')
    GROUP BY category
    ORDER BY average_price DESC;

    - Explanation: Used "IN" to list all the categories 
    - Output: 
      
**OBJECTIVE 2:** To understand the orders tables by finding the number of items within each order, and the orders with the highest number of items

**1) How many orders were made?**
   - Scripting:
     SELECT 
      COUNT(DISTINCT order_id) AS numbers_of_orders
     FROM order_details;

    - Explanation: used "COUNT DISTINCT" to calculate the number of orders
    - Output: ![image](https://github.com/user-attachments/assets/feca615d-0508-430e-9b21-c1434ebe954b)

    
OBJECTIVE 3
