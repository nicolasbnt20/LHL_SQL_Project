What issues will you address by cleaning the data?

To clean the data, I focused on three tables: all_sessions, products, and sales_by_sku. I chose these because sales_by_sku was already quite clean, less dense and similar to sales_report. In all_sessions, I removed many columns that were irrelevant to the analysis or filled with NULLs or missing values. This helped simplify the dataset and made it easier to see the actual data. I also could have normalized better the tables,for example, by moving product-related data from all_sessions to the products table but due to time constraints, I prioritized the analysis.

I also checked for duplicates, like productSKU in products or sales_by_sku. I saw duplicate fullVisitorID values, but this made sense since a visitor can go on the site multiple times.

In the products and sales_by_sku tables, I made minor cleaning adjustments like renaming columns for consistency, standardizing capitalization, and removing small inconsistencies. In products, I decided to keep a single NULL value found in the sentimentScore and sentimentMagnitude columns, because it had no real impact on the data and probably came from a visitor who didn’t leave any feedback. For location data, I replaced NULL values in city and country columns with "Unknown" so I could use that data without skewing insights. I also replaced NULLs in time_on_site with 0, reformatted the date column for clarity, and divided and rounded product prices by 1,000,000 to make them more accurate.




Queries:
Below, provide the SQL queries you used to clean your data.

CREATE TABLE all_sessions_cleaned AS 
SELECT 
	"fullVisitorId" as fullvisitorid,
	"channelGrouping" as channelgrouping,
	time,
	CASE 
	WHEN TRIM(country) is NULL or TRIM(country) = '(not set)' THEN 'Unknown'
	ELSE country 
	END as country,
	CASE 
	WHEN TRIM(city) IS NULL or TRIM(city) = '(not set)' or TRIM(city) = 'not available in demo dataset' THEN 'Unknown'
	ELSE city
	END as city,
	COALESCE("timeOnSite", 0) as timeonsite,
	pageviews,
	TO_DATE(date, 'YYYYMMDD') as date,
	"visitId" as visitid,
	"type" as type_of_session,
	ROUND("productPrice"/1000000.0, 2) as productprice, ( j’aurai pu le séparer et le mettre dans products pour plus de logique ) 
	"productSKU" as productsku,
	"v2ProductName" as productname,
	"v2ProductCategory" as productcategory,
	"pageTitle" as pagetitle

FROM all_sessions;




CREATE TABLE products_cleaned AS 
SELECT 
	"SKU" as productsku,
	TRIM(name) AS product_name,
	"orderedQuantity" AS ordered_quantity,
	"stockLevel" AS stock_level,
	"restockingLeadTime" AS restocking_lead_time,
	"sentimentScore" AS sentiment_score, 
	"sentimentMagnitude" AS sentiment_magnitude 

FROM products;




CLEANED SALES_BY_SKU 

CREATE TABLE sales_by_sku_cleaned AS 
SELECT 
	"productSKU" AS productsku,
	total_ordered

FROM sales_by_sku;
