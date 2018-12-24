# MySqlDataAnalytics
Application of Mysql and Sql in Data Analytics

- Should i relate tables with PK and FK for joining queries ?  
Not nesseceraliy ,There is no need to relate tables for joining tables by quesris. As long as the run queries with join settings the result would be the same. 


#1.How to check if colummn's values in table left is matching to column's table in right table? 

"SELECT product_subsets.aisle_id
FROM product_subsets
    LEFT JOIN aisles ON product_subsets.aisle_id = aisles.aisle_id
WHERE aisles.aisle_id IS NULL"

or 

"SELECT product_subsets.aisle_id
FROM product_subsets
WHERE NOT EXISTS (
SELECT aisles.aisle_id FROM aisles WHERE product_subsets.aisle_id = aisles.aisle_id)"

#2. How to select only rows with max value in a columns? 

"SELECT order_id, MAX(order_dow)
FROM orders
GROUP BY order_id;"


#3. Compare the EDA performance between sql queries and python or R EDA syntaxes ? 

e.g in the orders table from this project if you run below query it almost takes 25 min to see the result. but in python or r it is like a seconds : 

sql query : 

"SELECT order_id, MAX(order_dow)
FROM orders
GROUP BY order_id;"

usually the aggreagate functions in sql take time to be run. in this exampl on almost 1.5 million rows takes long time. 

