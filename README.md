# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
The goal of this project was to load, explore and analyze an e-commerce dataset. The objective was to get insights about revenue, visitors, and product performance across different cities and countries. With a data cleaning processes, and queries with a QA process  I tried to see patterns in product categories, site engagement, and purchasing behavior.

## Process
### Load and set up the database
### Data exploration and cleaning
### Quality assurance 
### Analyze with queries
### Interpret

## Results
I discovered things like top perforrming products and location, the average visitor behavior, categories that were popular accross the world, patterns on purchases. We could use this result to focus certain efforts in particular fields, like where to develop, what kind of product to keep and push or not, what channel is the best to promote the commerce. 

## Challenges 
One of the main challenges I faced during this project was the limited time available. With multiple tables and a lot of columns to explore, I had to choose the most relevant aspects and make assumptions in cases where the data was not clear. The all_sessions table had an overwhelming number of columns with many either null or redundant, which made it difficult to navigate. I also found it ambiguous to interpret the exact purpose of some fields. It was sometimes hard to determine what was expected or strictly necessary for the analysis, especially when fields were very similar.

## Future Goals
If I had more time to spend on this project, I would start by better normalizing and structuring the database, by breaking down and cleaning more in depth certain fields. I could also trim product category paths to have much clearer insights. I’d also like separate analyses across countries and cities, because it felts a bit too messy sometimes. I’m curious about the potential relationship between columns like sentiment scores and actual sales. I also realized at the very end that even though fullvisitorID may be duplicated because a visitor could go on the site multiple times, visitID should be unique which I did not realize at the cleaning data step but after finishing the whole project, so some of my results could be innacurate and my ERD is wrong. I misunderstood what this field was standing for as I thought it was a redundant field of the first one.  
