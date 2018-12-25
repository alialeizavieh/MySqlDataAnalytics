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


#4. Which products has been reordered most ? 

"
SELECT product_subsets.product_id,product_subsets.product_name,count(order_product_prior.reordered) as cnt from product_subsets 
INNER JOIN 
order_product_prior 
ON 
product_subsets.product_id=order_product_prior.product_id WHERE order_product_prior.reordered = 1 group by product_subsets.product_id 
order by cnt DESC LIMIT 10
"


#5. Query for Retrieving Tables ?

" SHOW tables "

#6. Query for Outputting Data Using a Constraint ?

I used "select * from departments where department = 'frozen'" as the record exists but the query has no result. i tried the query with 'like' operator and the query returned the result and i found out there should be some spaces in the data. 

" select * from departments where department like "%frozen%" "


