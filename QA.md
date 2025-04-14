What are your risk areas? Identify and describe them.

Some key risk areas I identified during the QA process included missing or NULL values in columns like country and city, as well as issues with the product_price with extremely large values. I also checked all integer and numeric columns for negative or again extreme values that could indicate outliers. Other risk areas were improperly formatted dates in the column date and key columns like productSKU in the products and sales_by_sku tables that could potentially hide duplicates.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

To ensure data quality, I followed guidelines to check for common issues: NULL values, searching for duplicates, analyzing value distributions to spot outliers, and checking the format and consistency of date and time fields. 

The generic queries were (I included one example of the same type of query, like one for every numeric and integer field I checked if It wasn't below 0):

SELECT 
  COUNT(*) 
FROM all_sessions
WHERE "productSKU" IS NULL;

 
SELECT 
  COUNT(*)
FROM all_sessions
WHERE city IN ('(not set)', 'not available in demo dataset');


SELECT 
  "productSKU",
  COUNT(*)
FROM products
GROUP BY "productSKU"
HAVING COUNT(*) > 1; 

(The * in some () won't display on screen for unknown reasons)

SELECT *
FROM all_sessions
WHERE "timeOnsite" < 0;

SELECT *
FROM all_sessions
WHERE "timeOnsite" > 9999; 
  
  
