# MySqlDataAnalytics
Application of Mysql and Sql in Data Analytics

- Should i relate tables with PK and FK for joining queries ?  
Not nesseceraliy ,There is no need to relate tables for joining tables by quesris. As long as the run queries with join settings the result would be the same. 


#How to check if colummn's values in table left is matching to column's table in right table? 

"SELECT product_subsets.aisle_id
FROM product_subsets
    LEFT JOIN aisles ON product_subsets.aisle_id = aisles.aisle_id
WHERE aisles.aisle_id IS NULL"

or 

"SELECT product_subsets.aisle_id
FROM product_subsets
WHERE NOT EXISTS (
SELECT aisles.aisle_id FROM aisles WHERE product_subsets.aisle_id = aisles.aisle_id)"


