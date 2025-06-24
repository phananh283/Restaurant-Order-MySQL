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
     ) AS least_expensive

   - Explanation: created a table of the least expensive item and another table of the most expensive item, then combined the 2 tables by using "UNION"
   - Output:
     - The least expensive item is "Edamame", priced at $5.00
     - The most expensive item is "Shrimp Scampi", priced at $19.95

**2) How many Italian dishes are on the menu? What are the least and most expensive Italian dishes on the menu?**
   - Scripting:
     SELECT item_name, count(menu_item_id) as numbers_of_dishes
     FROM menu_items
     WHERE category = 'Italian'
     GROUP BY menu_item_id
     
   - Explanation: As there are many other categories like Asian, American, etc., we needed to use "WHERE" to specify the category of "Italian"
   - Output: There are 9 Italian dish items, including Spaghetti, Spaghetti & Meatballs, Fettuccine Alfredo, Meat Lasagna, Cheese Lasagna, Mushroom Ravioli, Shrimp Scampi, Chicken Parmesan, and Eggplant Parmesan. 
   
**3) How many dishes are in each category? What is the average dish price within each category?**
   - Scripting:
     SELECT 
	    category,
      COUNT(menu_item_id) AS number_of_dishes,
      ROUND(AVG(price),2) AS average_price
    FROM menu_items
    WHERE category IN ('American', 'Asian', 'Mexican', 'Italian')
    GROUP BY category
    ORDER BY average_price DESC

   - Explanation: Used "IN" to list all the categories
   - Output:
     - The number of Italian dishes is 9, with an average price of $16.75
     - The number of Asian dishes is 8, with an average price of $13.48
     - The number of Mexican dishes is 9, with an average price of $11.80
     - The number of American dishes is 6, with an average price of $10.07

      
**OBJECTIVE 2:** To understand the orders tables by finding the number of items within each order, and the orders with the highest number of items

**1) How many orders were made?**
   - Scripting:
     SELECT 
      COUNT(DISTINCT order_id) AS numbers_of_orders
     FROM order_details
     
   - Explanation: Used "COUNT DISTINCT" to calculate the number of orders
   - Output: Total number of orders is 5,370

**2) How many orders had more than 12 items?**
   - Scripting:
     SELECT COUNT(*) FROM
	(SELECT
		order_id,
		COUNT(item_id) AS numbers_of_items 
	FROM order_details
	GROUP BY order_id
	HAVING numbers_of_items > 12) as numbers_of_orders;

   - Explanation: Used "HAVING" to identify the orders with more than 12 items
   - Output: There are 12 orders with more than 12 items
         
**OBJECTIVE 3:** To find the least and most ordered categories, and dive into the details of the highest spend orders

**1) What were the least and most ordered items? What categories were they in?**
   - Scripting:
     SELECT category, item_name, total_orders
     FROM 
	    (SELECT category, item_name, COUNT(*) AS total_orders
      FROM combined_table
      GROUP BY category, item_name
      ORDER BY total_orders ASC
      LIMIT 1
    ) AS least_item

     UNION
 
     SELECT category, item_name, total_orders
     FROM 
	    (SELECT category, item_name, COUNT(*) AS total_orders
      FROM combined_table
      GROUP BY category, item_name
      ORDER BY total_orders DESC
      LIMIT 1
    ) AS most_item

   - Explanation: Created a table of the least ordered item and another table of the most ordered item, then combined the 2 tables by using "UNION"
   - Output:
     	- The least ordered item is Chicken Tacos of Mexican category, with total orders of 123
     	- The most ordered item is Hamburger of American category, with total orders of 622
    
**2) What were the top 5 orders that spent the most money?**
   - Scripting:
     SELECT order_id, SUM(price) AS total_spending
     FROM combined_table
     GROUP BY order_id
     ORDER BY total_spending DESC
     LIMIT 5

   - Explanation: used LIMIT 5 to get the top 5 orders
   - Output:
     	- The top 5 order_id are 440, 2075, 1957, 330, 2675
     	- The total spending is ranged from $185.10 to $192.15
