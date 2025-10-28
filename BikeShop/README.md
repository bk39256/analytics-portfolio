
# SQL Project 1 | Group 1
Members: Ben Kim, Jackson Avey, Christian Hertzig, Success Onichabor, Drew Vajda

## Scenario
The task at hand is to model and build a relational database for the general operations of a network of bike stores. The central entity in the model is the Store entity, representing each individual location where bikes and related products are sold. Each store manages its stock of products, including details such as quantity, category, and brand. The system also tracks orders placed by customers, consisting of multiple order items tied to specific products. Additionally, employees are associated with each store to manage sales and inventory. The product detail entity provides descriptive information about each item, while the categories and brands entities classify and group products effectively. Our goal is to accurately model these relationships, populate the database with sample data, and perform queries that yield useful insights into sales performance, inventory management, and customer purchasing trends across all stores.





## Data Model
Explanation:
The Bike Store data model captures how products, customers, employees, and orders interact across multiple store locations. It is built around a set of well-defined relationships that mirror the real-world structure of a retail business. At the heart of the design is the Product table, which connects to several entities. Each product belongs to exactly one Brand and one Category, forming two one-to-many relationships from Product to Brand and Category. This means a single brand or category can be associated with many products, but each product can only have one brand and one category. The Product_Detail table is linked to Product through a one-to-one relationship because each product has a unique set of detailed attributes such as model year and list price that apply to that specific item only.
Sales transactions are modeled through the Orders and Order_Item tables. The Orders table connects to Customer in a one-to-many relationship since a single customer can place many orders, but each order belongs to only one customer. Each Order can also have multiple items, leading to a one-to-many relationship between Orders and Order_Item. On the other side, each Order_Item references a single Product, but a product can appear in many order items over time, forming another one-to-many relationship from Order_Item to Product. The Order_Item table serves as an associative entity, enabling a many-to-many conceptual link between Orders and Product while storing key transactional attributes like quantity, discount, and price at the item level.
Inventory and store management are supported by the Stores, Stock, and Employee tables. The Stock table connects Stores and Product in a many-to-many relationship implemented through Stock as a bridge entity. Each store carries many products, and each product can be stocked in multiple stores, with quantity levels stored in Stock. The Employee table links to Stores in a one-to-many relationship, showing that multiple employees work in one store. It also has a self-referencing relationship where an employee can manage other employees, modeling a managerial hierarchy within the same table. Finally, Orders connect back to Stores indirectly through Employees and Customers, ensuring that every transaction can be traced to both a customer and a physical location.
Overall, this model efficiently supports business operations such as tracking sales, managing inventory, and organizing staff responsibilities. The chosen relationship types ensure data consistency — for example, every product detail ties to exactly one product, while inventory and sales can scale across multiple stores and customers. This balance of one-to-one, one-to-many, and many-to-many structures provides both normalization and flexibility for future business growth.

<img width="892" height="733" alt="SQLProjectDataModelImage" src="https://github.com/user-attachments/assets/8337c60e-26e6-4089-8a54-1c85048dfe9e" />


## Data Dictionary
<img width="608" height="776" alt="dd1" src="https://github.com/user-attachments/assets/44a78b02-dc85-4cd9-a5a5-2ff7b1b14835" />
<img width="612" height="731" alt="dd2" src="https://github.com/user-attachments/assets/f79aa584-c186-4c56-b600-b04abf83066a" />
<img width="628" height="830" alt="dd3" src="https://github.com/user-attachments/assets/ed7b2800-34b3-4f24-83ff-f216a6520714" />
<img width="628" height="891" alt="dd4" src="https://github.com/user-attachments/assets/689e6454-5f9a-4133-959c-f9c21c5f653c" />
<img width="630" height="866" alt="dd5" src="https://github.com/user-attachments/assets/8307cacf-95c8-4d74-83d6-a7896a2e1e5f" />
<img width="634" height="207" alt="dd6" src="https://github.com/user-attachments/assets/2c48d451-4b3c-4898-8694-6ff3697848d3" />



## Ten Queries

1. Query 1 finds the total number of employees working under each manager.
This query uses a self-join on the employee table to determine how many employees report to each manager.
By comparing manager_id in the employee records with employee_id in the manager records, it counts the total number of direct reports per manager. The GROUP BY clause groups results by manager, and COUNT() provides the number of employees under each.
   
<img width="2400" height="995" alt="Screenshot 2025-10-26 232734" src="https://github.com/user-attachments/assets/5a94d585-b925-4390-a013-d48506fa8ea0" />

Managerial Justification:
A manager or HR leader would use this query to analyze organizational structure and span of control.
It helps identify which managers oversee larger or smaller teams, allowing leadership to evaluate workload distribution, managerial effectiveness, and opportunities to balance responsibilities across departments.

2. This query calculates the total sales revenue for each store, grouped by product category. It joins four tables — order_item, product, category, and stores — to connect each sold product to its category and store. The SUM(order_item.list_price) function adds up the total revenue generated from all sales within each store and category combination.

<img width="2128" height="995" alt="image" src="https://github.com/user-attachments/assets/cd53384f-46c7-4341-9f17-decc9a6069c3" />

Managerial Justification:
From a managerial perspective, this query helps decision-makers evaluate store performance by product type. It identifies which categories generate the most revenue in each location, allowing managers to adjust inventory levels, marketing efforts, and shelf space allocation to maximize profitability and meet local customer demand.

3.  This query uses a correlated subquery to determine which customers have purchased items from more than three distinct product categories. The inner subquery counts the number of unique category_id values per customer by joining the orders, order_item, and product tables. The outer query filters for customers where that count is greater than three.

<img width="1216" height="996" alt="Screenshot 2025-10-26 230548" src="https://github.com/user-attachments/assets/4046a3e5-104c-4867-b14c-cd20baed0073" />

Managerial Justification:
 From a managerial perspective, this query highlights diverse or high-value customers who purchase across multiple product categories. These customers represent strong cross-selling opportunities, brand loyalty, and potential candidates for premium promotions or targeted marketing campaigns. Understanding which customers buy from various categories can help refine marketing strategies and loyalty programs.

4.  This query identifies all products that have never been sold but are still in stock at one or more stores. It joins the product, stock, and stores tables to find which stores carry each product, and then uses a subquery to exclude any product that appears in the order_item table (indicating it has been sold before).

<img width="1488" height="1005" alt="Screenshot 2025-10-26 230753" src="https://github.com/user-attachments/assets/291371ad-7036-4678-9ce3-dfd05d7ca54f" />

Managerial Justification:
Managers can use this query to uncover slow-moving or non-selling inventory. Knowing which products are stocked but haven’t sold helps in making strategic decisions about product discontinuation, promotional pricing, or redistribution of unsold inventory. This insight supports better inventory management and reduces holding costs across stores.

5.  This query calculates how frequently each customer places orders by measuring the average number of days between their orders. It uses a self-join on the orders table to compare each order date with subsequent ones for the same customer. The DATEDIFF() function determines the number of days between orders, and AVG() computes each customer’s mean interval. The query then sorts by that average and displays the top 10 customers who order most often.

<img width="2370" height="997" alt="Screenshot 2025-10-26 231014" src="https://github.com/user-attachments/assets/9e586a29-4bd9-4bda-bc52-0e24a7121b10" />

Managerial Justification:
 Managers can use this query to identify high-frequency customers — those who purchase most regularly. These customers are valuable for loyalty programs and repeat-purchase promotions. Recognizing buying frequency helps businesses understand purchasing patterns, forecast demand, and tailor marketing strategies to sustain engagement and maximize customer lifetime value.

6.  This query lists employees who do not manage anyone but work at stores that have generated more than $10,000 in total sales. It first filters out employees who are listed as managers using a subquery on the employee table. The second subquery calculates store-level sales totals by joining order_item and product, grouping results by store, and using a HAVING clause to include only stores whose total sales exceed $10,000.

<img width="1822" height="1008" alt="Screenshot 2025-10-26 231300" src="https://github.com/user-attachments/assets/e9fea775-fe4b-4f50-b283-f4111aadfb7b" />

Managerial Justification:
 Managers can use this query to recognize individual contributors at top-performing locations who may be driving strong sales results without holding leadership roles. Identifying such employees can inform performance evaluations, bonus allocations, or potential promotions. It also helps leadership understand how sales contributions are distributed across management levels.

7. This query identifies which stores carry the largest number of unique products from the brands Trek and Electra. It joins the stores, stock, product, and brand tables to connect each product to its brand and store. The COUNT(DISTINCT product.product_id) function ensures that only unique products are counted. The results are grouped by store and brand, then ordered by the total number of products in descending order to highlight the top carriers.

<img width="1888" height="1012" alt="Screenshot 2025-10-26 231403" src="https://github.com/user-attachments/assets/0e746675-6157-4989-b942-6de6c42173ae" />

Managerial Justification:
 From a managerial standpoint, this query helps identify key retail locations that offer the widest selection of high-value brands like Trek and Electra. Managers can use this insight to evaluate brand distribution, assess inventory balance, and inform marketing or supply chain decisions. Knowing which stores carry the most of these premium brands can guide promotional focus, stocking strategies, and partnership discussions with brand suppliers.

8.  This query determines the top-selling product in each category by total revenue across all stores. It joins the category, product, and order_item tables to calculate revenue using SUM(quantity * list_price). The query uses a window function (ROW_NUMBER()) to rank products within each category by revenue. Only products with a rank of 1 (the highest revenue) are returned.

<img width="2839" height="992" alt="Screenshot 2025-10-26 231539" src="https://github.com/user-attachments/assets/c0b686c2-4beb-4530-a98d-fd6076f54529" />

Managerial Justification:
 From a managerial perspective, this query reveals which products are the best performers within each category, helping identify revenue leaders and key products driving category success. Managers can use these insights for strategic pricing, promotion, and inventory allocation. It also aids in understanding customer preferences and refining category-level marketing strategies to focus on high-performing items.

9.  This query identifies all products whose stock levels have dropped below 10 units in any store. It joins the stock, product, brand, and stores tables to display store information alongside the associated product and brand details. The WHERE clause filters the results to show only low-inventory items.

<img width="1672" height="983" alt="Screenshot 2025-10-26 231654" src="https://github.com/user-attachments/assets/1a2e1f42-2dea-4f7d-ad1e-324bc8222a73" />

Managerial Justification:
 From a managerial standpoint, this query supports inventory management and replenishment planning. It helps store managers and supply chain teams quickly identify which products need restocking before they run out, reducing the risk of lost sales. By including brand information, managers can also assess which suppliers may require faster reorder cycles or adjustments to their delivery schedules.

10.  This query identifies customers who have bought a greater-than-average quantity of a specific product compared to other customers. It aggregates total quantities purchased by each customer for each product, then uses a correlated subquery to calculate the average quantity purchased per product across all customers. The HAVING clause filters the results to include only those customers whose totals exceed the product’s average purchase quantity.

<img width="2775" height="1002" alt="Screenshot 2025-10-26 231818" src="https://github.com/user-attachments/assets/e7e75f50-66c4-4ef9-ae9f-6e68354c9398" />

 From a managerial perspective, this query helps identify top buyers and high-value customers for specific products. These insights can guide customer segmentation, reward programs, and personalized marketing campaigns. Additionally, recognizing products with repeat or bulk purchasers allows managers to optimize stock levels, forecast demand more accurately, and target promotions toward customers most likely to respond.






## Database Information
<img width="3290" height="1647" alt="image" src="https://github.com/user-attachments/assets/b24647dc-4a06-402c-92ca-ce863cf5a836" />

The Name of the Database is: cs_bck81809
