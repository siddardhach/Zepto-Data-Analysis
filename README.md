# Zepto-Data-Analysis
The main aim of this project is to analyze zepto data based on various factorsThe primary objective of this project is to perform a comprehensive analysis of Zepto's data by examining various influencing factors that can drive informed business decisions and operational improvements

**Create Table :**
create table zepto (
sku_id SERIAL PRIMARY KEY,
category VARCHAR(120),
name VARCHAR(150) NOT NULL,
mrp NUMERIC(8,2),
discountPercent NUMERIC(5,2),
availableQuantity INTEGER,
discountSellingPrice NUMERIC(8,2),
weightInGms INTEGER,
outOfStock BOOLEAN,	
quantity INTEGER
);

**Different product categories**
SELECT DISTINCT category
FROM zepto
ORDER BY category;

**products in stock vs out of stock**

SELECT outOfStock, COUNT(sku_id)
FROM zepto
GROUP BY outOfStock;

**To check Distinct Product**
SELECT name, count(*) FROM zepto GROUP by
name having count(*)>1 order by count(*) DESC

**Data Transformation** 
UPDATE zepto
SET mrp = mrp / 100.0,
discountSellingPrice = discountSellingPrice / 100.0;
Data Analysis
** Find the top 10 best-value products based on the discount percentage.**
SELECT DISTINCT name, mrp, discountPercent
FROM zepto
ORDER BY discountPercent DESC
LIMIT 10;
**What are the Products with High MRP but Out of Stock**
SELECT DISTINCT name,mrp
FROM zepto
WHERE outOfStock = TRUE and mrp > 300
ORDER BY mrp DESC;
**Calculate Estimated Revenue for each category**
SELECT category,
SUM(discountSellingPrice * availableQuantity) AS total_revenue
FROM zepto
GROUP BY category
ORDER BY total_revenue;
** Find all products where MRP is greater than ₹500 and discount is less than 10%.**
SELECT DISTINCT name, mrp, discountPercent
FROM zepto
WHERE mrp > 500 AND discountPercent < 10
ORDER BY mrp DESC, discountPercent DESC;
**Identify the top 5 categories offering the highest average discount percentage.**
SELECT category,
ROUND(AVG(discountPercent),2) AS avg_discount
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;
**Find the price per gram for products above 100g and sort by best value.**
SELECT DISTINCT name, weightInGms, discountSellingPrice,
ROUND(discountSellingPrice/weightInGms,2) AS price_per_gram
FROM zepto
WHERE weightInGms >= 100
ORDER BY price_per_gram;
**Group the products into categories like Low, Medium, Bulk.**
SELECT DISTINCT name, weightInGms,
CASE WHEN weightInGms < 1000 THEN 'Low'
	WHEN weightInGms < 5000 THEN 'Medium'
	ELSE 'Bulk'
	END AS weight_category
FROM zepto;
**What is the Total Inventory Weight Per Category **
SELECT category,
SUM(weightInGms * availableQuantity) AS total_weight
FROM zepto
GROUP BY category
ORDER BY total_weight;

