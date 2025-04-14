Question 1: What is the average time spent on the site  ?

SQL Queries:
SELECT 
	ROUND(AVG(timeonsite),2) AS avg_time_on_site
FROM all_sessions_cleaned;

Answer: 
The average time spent on the site is 175.71 seconds, or 2.93 minutes, which shows there's not much engagement. There might be an issue with the site itself.



Question 2: What product have the highest sentiment_score? Does one product has a perfect sentiment_score ?

SQL Queries:
SELECT 
	sku,
	product_name,
	sentiment_score
FROM products_cleaned 
WHERE sentiment_score IS NOT NULL
ORDER BY sentiment_score DESC;

Answer:
The USB wired soundbar is the only product with the highest and perfect sentiment score but it’s available in-store only.



Question 3: Which channel bring the highest revenue overall?

SQL Queries:
SELECT 
	ac.channelgrouping,
	SUM(productprice * total_ordered) AS total_revenue

FROM all_sessions_cleaned ac
JOIN sales_by_sku_cleaned sbsc ON ac.productsku = sbsc.productsku
GROUP BY ac.channelgrouping
ORDER BY total_revenue DESC;

Answer:
The channel that brings the highest revenue is Referral. Knowing that Referral is the best performing channel can allow to focus a strategy around that to increase sales. 



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
