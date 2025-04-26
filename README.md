# SQL1_retail_transactions
This is my first project on Github and I'll be using SQL to exlore and clean retail sales for a shop.
I first turned the Excel file from CSV to SQL using a website converter. 

# CLeaning
I counted the data to make sure nothing is missing and compared it by Excel tables count which is 2000.
Checking if any of the columns are null and found three rows with ID: 679, 746 and 1225 are null, so I'm going to delete them and final count is 2000. Wanted to count number of custormers and sales using SELECT COUNT() function.

Questions:
-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
```sql
SELECT * FROM mytable WHERE sale_date = '2022-11-05';

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 3 in the month of Nov-2022

SELECT * FROM mytable 
WHERE category = 'Clothing' 
AND quantiy>3
AND sale_date BETWEEN '2022-11-01' AND '2022-11-30';


-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.

SELECT category, SUM(total_sale)
FROM mytable
GROUP BY 1;

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

SELECT avg(age) AS average_age 
FROM mytable
WHERE category = 'Beauty';


-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.

SELECT * From mytable
WHERE total_sale > 1000;

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.

SELECT COUNT(transactions_id) AS number_of_transactions, gender, category FROM mytable
GROUP by gender, category

-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year

SELECT avg(total_sale) AS average_sale, extract(YEAR FROM sale_date) as year, extract(MONTH FROM sale_date) as month
FROM mytable
GROUP by year, month
ORDER by year, month;

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 

SELECT customer_id, SUM(total_sale) AS total
FROM mytable
GROUP BY customer_id
ORDER BY total DESC
LIMIT 5;

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.

SELECT COUNT(DISTINCT customer_id), category FROM mytable
GROUP by category

 Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)

WITH hourly_sale as (
SELECT *,
CASE
	WHEN EXTRACT(HOUR FROM sale_time) <=12 THEN 'morning'
	when EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'afternoon'
	when EXTRACT(HOUR FROM sale_time) >17 then 'evening'
    END as shift
FROM mytable
)
SELECT shift, COUNT(*) as total
FROM hourly_sale
GROUP by shift

