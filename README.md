# Walmart-Sales-Data-Analysis
This project is an end-to-end data analysis solution designed to extract critical business insights from Walmart sales data. We utilize Python for data processing and analysis, SQL for advanced querying, and structured problem-solving techniques to solve key business questions.

## Project Steps

**Set Up the Environment**

Tools Used: Visual Studio Code (VS Code), Python, SQL (MySQL and PostgreSQL)

Goal: Create a structured workspace within VS Code and organize project folders for smooth development and data handling.

**Set Up Kaggle API**

API Setup: Obtain your Kaggle API token from Kaggle by navigating to your profile settings and downloading the JSON file.
Configure Kaggle:
Place the downloaded kaggle.json file in your local .kaggle folder.
Use the command kaggle datasets download -d <dataset-path> to pull datasets directly into your project.

**Download Walmart Sales Data**

Data Source: Use the Kaggle API to download the Walmart sales datasets from Kaggle.
Dataset Link: Walmart Sales Dataset
Storage: Save the data in the data/ folder for easy reference and access.

**Install Required Libraries and Load Data**

Libraries: Install necessary Python libraries using:
pip install pandas numpy sqlalchemy mysql-connector-python psycopg2
Loading Data: Read the data into a Pandas DataFrame for initial analysis and transformations.

**Explore the Data**

Goal: Conduct an initial data exploration to understand data distribution, check column names, and types, and identify potential issues.
Analysis: Use functions like .info(), .describe(), and .head() to get a quick overview of the data structure and statistics.

**Data Cleaning**

*Remove Duplicates*: Identify and remove duplicate entries to avoid skewed results.
*Handle Missing Values*: Drop rows or columns with missing values if they are insignificant; fill values where essential.
*Fix Data Types*: Ensure all columns have consistent data types (e.g., dates as datetime, prices as float).
*Currency Formatting*: Use .replace() to handle and format currency values for analysis.
*Validation*: Check for any remaining inconsistencies and verify the cleaned data.

**Feature Engineering**
*Create New Columns*: Calculate the Total Amount for each transaction by multiplying unit_price by quantity and adding this as a new column.
*Enhance Dataset*: Adding this calculated field will streamline further SQL analysis and aggregation tasks.

**Load Data into MySQL and PostgreSQL**

*Set Up Connections*: Connect to MySQL and PostgreSQL using sqlalchemy and load the cleaned data into each database.
*Table Creation*: Set up tables in both MySQL and PostgreSQL using Python SQLAlchemy to automate table creation and data insertion.
Verification: Run initial SQL queries to confirm that the data has been loaded accurately.

**SQL Analysis: Complex Queries and Business Problem Solving**
Business Problem-Solving: Write and execute complex SQL queries to answer critical business questions, such as:

**Q.1 Find different payment method and number of transactions, number of qty sold.**
```
SELECT 
	 payment_method,
	 COUNT(*) as no_payments,
	 SUM(quantity) as no_qty_sold
FROM walmart
GROUP BY payment_method

```
**Q.2 Identify the highest-rated category in each branch, displaying the branch, category
-- AVG RATING .**
```
SELECT * 
FROM
(	SELECT 
		branch,
		category,
		AVG(rating) as avg_rating,
		RANK() OVER(PARTITION BY branch ORDER BY AVG(rating) DESC) as rank
	FROM walmart
	GROUP BY 1, 2
)
WHERE rank = 1

```
**Q.3 Identify the busiest day for each branch based on the number of transactions**
```
SELECT * 
FROM
	(SELECT 
		branch,
		TO_CHAR(TO_DATE(date, 'DD/MM/YY'), 'Day') as day_name,
		COUNT(*) as no_transactions,
		RANK() OVER(PARTITION BY branch ORDER BY COUNT(*) DESC) as rank
	FROM walmart
	GROUP BY 1, 2
	)
WHERE rank = 1

```
**Q.4 Calculate the total quantity of items sold per payment method. List payment_method and total_quantity.
.**
```
SELECT 
	 payment_method,
	 -- COUNT(*) as no_payments,
	 SUM(quantity) as no_qty_sold
FROM walmart
GROUP BY payment_method
```
**Q.5
-- Determine the average, minimum, and maximum rating of category for each city. 
-- List the city, average_rating, min_rating, and max_rating.**
```
SELECT 
	city,
	category,
	MIN(rating) as min_rating,
	MAX(rating) as max_rating,
	AVG(rating) as avg_rating
FROM walmart
GROUP BY 1, 2
```
**Q.6
-- Calculate the total profit for each category by considering total_profit as
-- (unit_price * quantity * profit_margin). 
-- List category and total_profit, ordered from highest to lowest profit.**
```
SELECT 
	category,
	SUM(total) as total_revenue,
	SUM(total * profit_margin) as profit
FROM walmart
GROUP BY 1
```
 **Q.7
-- Determine the most common payment method for each Branch. 
-- Display Branch and the preferred_payment_method.**
```
WITH cte 
AS
(SELECT 
	branch,
	payment_method,
	COUNT(*) as total_trans,
	RANK() OVER(PARTITION BY branch ORDER BY COUNT(*) DESC) as rank
FROM walmart
GROUP BY 1, 2
)
SELECT *
FROM cte
WHERE rank = 1

```
**Q.8
-- Categorize sales into 3 group MORNING, AFTERNOON, EVENING 
-- Find out each of the shift and number of invoices**
```
SELECT
	branch,
CASE 
		WHEN EXTRACT(HOUR FROM(time::time)) < 12 THEN 'Morning'
		WHEN EXTRACT(HOUR FROM(time::time)) BETWEEN 12 AND 17 THEN 'Afternoon'
		ELSE 'Evening'
	END day_time,
	COUNT(*)
FROM walmart
GROUP BY 1, 2
ORDER BY 1, 3 DESC
```



**Sales Insights**: 

Key categories, branches with the highest sales, and preferred payment methods.
Profitability: Insights into the most profitable product categories and locations.
Customer Behavior: Trends in ratings, payment preferences, and peak shopping hours.

**Future Enhancements**

Possible extensions to this project:
Integration with a dashboard tool (e.g., Power BI or Tableau) for interactive visualization.
Additional data sources to enhance analysis depth.
Automation of the data pipeline for real-time data ingestion and analysis.
**License**

This project is licensed under the MIT License.

**Acknowledgments**

Data Source: Kaggle’s Walmart Sales Dataset
Inspiration: Walmart’s business case studies on sales and supply chain optimization.

