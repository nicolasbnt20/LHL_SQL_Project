Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT 
	ac.country,
	ac.city,
	SUM(ac.productprice * sbsc.total_ordered) AS revenue

FROM all_sessions_cleaned ac
JOIN sales_by_sku_cleaned sbsc ON ac.productsku = sbsc.productsku
GROUP BY ac.country, ac.city
ORDER BY SUM(ac.productprice * sbsc.total_ordered) DESC
LIMIT 10;



Answer: 
The United States and the United Kingdom are the only two countries that appear in the top 10 for revenue, with the United States being in most of the places. The United Kingdom is only in 8th and 10th place.

For cities, the majority are marked as "Unknown." Among the known cities, the top ones are:
Mountain View, San Francisco, Sunnyvale, Palo Alto, New York, San Jose, London, and Chicago.




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
WITH cte_total_products_ordered AS (
SELECT 
	ac.country,
	 ac.city, 
	SUM(sbs.total_ordered) AS total_products_ordered
FROM sales_by_sku_cleaned sbs
JOIN all_sessions_cleaned ac ON sbs.productsku = ac.productsku
GROUP BY ac.country, ac.city
)
SELECT 
  country,
  city,
  ROUND(AVG(total_products_ordered), 2) AS avg_products_ordered
FROM cte_total_products_ordered
GROUP BY country, city
ORDER BY country, city;



Answer:
The averages go from 0 to 56219 which seems very high. Here again the United States dominates.





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
SELECT 
	ac.country,
	ac.city, 
	ac.productcategory,
	SUM(sbsc.total_ordered) AS total_products_ordered

FROM all_sessions_cleaned ac
JOIN sales_by_sku_cleaned sbsc ON ac.productsku = sbsc.productsku
GROUP BY ac.country, ac.city, ac.productcategory
ORDER BY total_products_ordered DESC;



Answer:
Some product categories appear at the top across multiple countries and cities like Home/Shop by Brand with either Youtube or Google, Home/Office or Home/Nest which is a product also linked to Google. However, the data could be more filtered. The current grouping is heavy so it would be useful to group by either country or city but not both. With more time I could have also removed everything before the second slash—to get a better insight of category trends.





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
WITH cte_top_selling_products AS (
SELECT 
	ac.country,
	ac.city, 
	pc.product_name,
	SUM(sbsc.total_ordered) AS total_product_ordered,
	RANK() OVER (PARTITION BY ac.country, ac.city ORDER BY SUM(sbsc.total_ordered) DESC) AS selling_rank
FROM all_sessions_cleaned ac 
JOIN products_cleaned pc ON ac.productsku = pc.productsku
JOIN sales_by_sku_cleaned sbsc ON ac.productsku = sbsc.productsku
GROUP BY ac.country, ac.city, pc.product_name
)
SELECT 
	country,
	city, 
	product_name,
	total_product_ordered,
	selling_rank

FROM cte_top_selling_products
WHERE selling_rank = 1
ORDER BY country, city;


Answer:
Many of the top-selling products are either bottles or bottle-related items. In terms of clothing, it seems male-oriented products sales a bit more compared to female-oriented ones. 





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
WITH cte_revenue_per_country_city AS (
SELECT 
    ac.country,
    ac.city,
    SUM(ac.productprice * sbsc.total_ordered) AS revenue_per_country_city
FROM all_sessions_cleaned ac
JOIN sales_by_sku_cleaned sbsc ON ac.productsku = sbsc.productsku
GROUP BY ac.country, ac.city
)
SELECT 
  rpcc.country,
  rpcc.city,
  ROUND((rpcc.revenue_per_country_city / (
SELECT 
	SUM(ac.productprice * sbsc.total_ordered)
	FROM all_sessions_cleaned ac
	JOIN sales_by_sku_cleaned sbsc ON ac.productsku = sbsc.productsku)) * 100, 2) AS revenue_percentage
FROM cte_revenue_per_country_city rpcc
ORDER BY revenue_percentage DESC; 




Answer:
Most of the percentage comes again from the United States, with over 50%, and a large portion is from unknown cities. Here too a grouping by either country or city instead of both could be better and clearer.






