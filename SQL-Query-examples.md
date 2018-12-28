# MySqlDataAnalytics
Application of Mysql and Sql in Data Analytics
Recources  : http://a4academics.com/interview-questions/53-database-and-sql/397-top-100-database-sql-interview-questions-and-answers-examples-queries?showall=&start=4


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

e.g in the orders table from this project if you run below query it almost takes 1 min to see the result. but in python or r it is like 25 seconds : 

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


#6. How To clean data if there white spaces after and before data ? or any tab or any break line? 

 for white space : "UPDATE `table` SET `col_name` = REPLACE(`col_name`, ' ', '')"
 
 for tab : "UPDATE `table` SET `col_name` = REPLACE(`col_name`, '\t', '' )"
 
 remove space at first and last characters : "UPDATE `table` SET `col_name` = TRIM(`col_name`)"
 
 
 
#.7 Query for Outputting Sorted Data Using ‘Order By’ ?

"select * from orders where order_dow = 2 order by order_hour_of_day DESC"
"select order_hour_of_day,count(order_hour_of_day) as cnt from orders group by order_hour_of_day order by cnt DESC"


#8.How long this costumers has been loyal to this retail store ? 

"select user_id,sum(days_since_prior_order) as LoyaltyDays from orders group by user_id order by loyaltyDays DESC"


#.8 Subqueries : 

"select max(LoyaltyDays),Min(LoyaltyDays),Avg(LoyaltyDays)
from
(select user_id,sum(days_since_prior_order) as LoyaltyDays 
from retail.orders 
group by user_id 
order by LoyaltyDays ASC)"

i performed this query but recivied this error : Every derived table must have its own alias. 
it means every drived table(AKA subquery table) must have and alias Therefor i changed it to :

"select max(LoyaltyDays),Min(LoyaltyDays),Avg(LoyaltyDays)
from
(select user_id,sum(days_since_prior_order) as LoyaltyDays 
from retail.orders 
group by user_id 
order by LoyaltyDays ASC) As sub1" 

and it works!

#.9 How long is the loyalty of each user in the retail dataset ? 

Solution : how do you define loyalty ? from this data loyalty is the sum of the all values of days_since_prior_order for each user_id would be total days the user id has been involved to purchase from this retail store.The query and subquery for fetching this data is : 

"select count(*)
from
(select user_id,sum(days_since_prior_order) as LoyaltyDays 
from retail.orders 
group by user_id 
order by LoyaltyDays ASC) as LoyltyDays
where LoyaltyDays  between 90 and 270"




#10. find Min and Max in each group of days of wee ( order_dow) ?

"


select order_dow,min(days_since_prior_order),max(days_since_prior_order)
from retail.orders 
group by order_dow 
order by order_dow DESC

"

#11. Data Manupulation using Sum,Count : 

"
select user_id, Sum(days_since_prior_order) as sum 
from retail.orders group by user_id order by sum DESC

"
 
 or to see how many times each user gor for shopping each 10 days : 



"
select user_id,count(user_id) as cnt
from retail.orders where days_since_prior_order between 0 and 10 group by user_id


"

#12. Creating View for above question  : 

"
create view purchasing_Each_10days as 
select user_id,count(user_id) as cnt
from retail.orders 
where 
days_since_prior_order between 0 and 10 
group by user_id

"

and retreive the information from this view : 

"
select * from purchasing_each_10days 

"




#13. query to display user tables,primary key,foriegn ke,unique key, : 


"
SELECT * FROM Sys.objects WHERE Type='u'
"

"
SELECT * from Sys.Objects WHERE Type='PK'
"


"
SELECT * FROM Sys.Objects WHERE Type='uq'
"
    
"    
SELECT * FROM Sys.Objects WHERE Type='f'
"
"
SELECT * FROM Sys.Objects WHERE Type='tr'
"

"
SELECT * FROM Sys.Objects WHERE Type='p'
"


#14. Swapping the Values of Two Columns in a table ?

"
UPDATE Customers SET Zip=Phone, Phone=Zip   
"

#.15 Returning a Column of Unique Values ?

"
select distinct(order_id) from retail.orders 

"



#16. Making a Top 25 with the SELECT TOP Clause : 

" 
select TOP 25 from retail.orders where order_id <> NULL

" 



#17. Fiding intersection of two tables : 

"
SELECT ID FROM Customers INNER
JOIN Orders ON Customers.ID = Orders.ID

"


#18. How to add columns to the current table :

"alter table rental add month date"

if you want to change the columns type : 

"alter table rental modify month int(10)"

#19. How to update new added column by only insert the month data extracted from another columns holding full date information :

"update rental set month = month(rental_date)"


#19. How to calculate diffrence between dates : 

"select DATEDIFF(return_date,rental_date) from rental"


#20. Get names of employees from employee table who has '%' in Last_Name. Tip : Escape character for special characters in a query : 


"Select FIRST_NAME from employee where Last_Name like '%\%%'"


#21. Get department,no of employees in a department,total salary with respect to a department from employee table order by total salary descending :

"

Select DEPARTMENT,count(FIRST_NAME),sum(SALARY) Total_Salary from employee group by DEPARTMENT order by Total_Salary descending

"

#21. Select no of rental Id with respect to year and month from rental table :

"
select year(rental_date),month(rental_date),count(rental_id) as cnt 
from rental 
group by year(rental_date),month(rental_date) order by cnt DESC
"

#22. have a condition on group by : Select department,total salary with respect to a department from employee table where total salary greater than 800000 order by Total_Salary descending :

"
Select DEPARTMENT,sum(SALARY) Total_Salary from employee group by DEPARTMENT having sum(SALARY) >800000 order by Total_Salary desc
"

#23. Select employee details from employee table if data exists in incentive table ?

"
select * from EMPLOYEE where exists (select * from INCENTIVES)
"

#24. How to fetch data that are common in two query results ? 


"
select * from EMPLOYEE where EMPLOYEE_ID INTERSECT select * from EMPLOYEE where EMPLOYEE_ID < 4
"

#25. Get Employee ID's of those employees who didn't receive incentives without using sub query ?
"
select EMPLOYEE_ID from EMPLOYEE
MINUS
select EMPLOYEE_REF_ID from INCENTIVES
"

#26. Select Banking as 'Bank Dept', Insurance as 'Insurance Dept' and Services as 'Services Dept' from employee table :

"
SELECT case DEPARTMENT when 'Banking' then 'Bank Dept' when 'Insurance' then 'Insurance Dept' when 'Services' then 'Services Dept' end FROM EMPLOYEE
"

#27. Delete employee data from employee table who got incentives in incentive table :

"
delete from EMPLOYEE where EMPLOYEE_ID in (select EMPLOYEE_REF_ID from INCENTIVES)

"


#28. Select Last Name from employee table which contain only numbers :

"
Select * from EMPLOYEE where lower(LAST_NAME)=upper(LAST_NAME)
"

Explanation : In order to achieve the desired result, we use "ASCII" property of the database. If we get results for a column using Lower and Upper commands, ASCII of both results will be same for numbers. If there is any alphabets in the column, results will differ.



