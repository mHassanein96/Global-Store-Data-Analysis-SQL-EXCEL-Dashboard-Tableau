# Global-Store-Data-Analysis-SQL-EXCEL-Dashboard-Tableau
SQL-based interactive dashboard built using Excel and Tableau to Analyze Global store sales and product over the world
### Problem:
Management asked How to increase profitability?
## Hypothesis:
One way to Increase profits is to reduce losses by reducing and analyzing "Dead Stocks" products, we found through analysis that tables product has 65,000$ losses
After analysis, we found that tables have the highest discounts (85% discount with order ID "IT-2012-5588535") while the average of discount around 11% , one of reasons that tables have
low demand is having the Highest shipping cost average "25$" while chairs having the 2nd Highest shipping cost average half tables shipping cost (13$).

**Recommend solutions:** replacing tables with Easy to disassemble and install ones (same to IKEA model) for easier and smaller packaging which will reduce shipping costs and increase tables sellability
Further and deeper shipping data analysis is required.

### Built With:
+ **SQL Server**
+ **Power Query** - ETL tool created by Microsoft for data extraction, loading and transformation, and is used to retrieve data from sources, process it, and load them into one or more target systems.
+ **Excel** (Analysis/ Visualization)
+ **Tableau Public** (Visualization)

![DashboardGS](https://github.com/mHassanein96/Global-Store-Data-Analysis-SQL-EXCEL-Dashboard-Tableau/assets/133708970/19d7ebba-8216-49e9-a08e-70d64f8a3d4d)

Used ETL tool Power Query to Load data from two tables "customers" and "orders" in "GobalStore" database to:
+ Combine 
+ Cast 
+ Flter 
+ Aggregate
+ Oder Data

Using the following query:
```sql

SELECT
	customer.customer_id
  ,customer.customer_name
  ,customer.city      
	,customer.state
  ,customer.region
  ,customer.country
	,CAST(LEFT(orders.order_date,10) AS DATE) AS order_date  -- Extracting The year,month,day data and cast it Str to Data
  ,orders.order_priority
  ,orders.ship_mode      
	,orders.product_id
	,orders.category
	,orders.sub_category
	,SUM(CAST(orders.discount AS float)) AS discount_TPO	  -- casting str to float|int and Aggregate for each order (TPO: Total per Odrder)..
  ,SUM(CAST(orders.profit AS float)) AS profit_TPO	  -- ..as orders in raw data seperated into products in each order
  ,SUM(CAST(orders.quantity AS int)) AS quantity_TPO
	,SUM(CAST(orders.shipping_cost AS float)) AS shipping_cost_TPO
  FROM GlobalStore.dbo.customers AS customer
  JOIN GlobalStore.dbo.orders AS orders
  ON customer.customer_id = orders.customer_id			   -- Using extra conditions with matching keys between the two tables..
  AND customer.profit = orders.profit				   -- ..to avoid mixing between the data and duplicates in the combined  table..
  AND customer.shipping_cost = orders.shipping_cost		   -- ..as there are many similarties between the orders.
  WHERE
	orders.product_id IS NOT NULL					   -- Cleaning data from Rows without products or quantity
	OR orders.profit IS NOT NULL					   -- ..so it won't affect our next Analysis
	OR orders.quantity IS NOT NULL
	OR CAST(orders.quantity AS int) != 0 
  GROUP BY
	customer.customer_id
  ,customer.customer_name
  ,customer.city      
	,customer.state
  ,customer.region
  ,customer.country
	,orders.order_date
  ,orders.order_priority
  ,orders.ship_mode      
	,orders.product_id
	,orders.category
	,orders.sub_category
  ORDER BY orders.order_date ASC
```
Then pipe data from Excel to Tableau for more Interactive Visualization 

![TableauGS2](https://github.com/mHassanein96/Global-Store-Data-Analysis-SQL-EXCEL-Dashboard-Tableau/assets/133708970/5c39cf5b-cbc2-45f2-b30b-92531aedcf14)


### Live Demo
+ [view Project Details on Tableau Public](https://public.tableau.com/app/profile/mahmoud.hassanein/viz/GlobalSuperStoreDashboard_16841818195170/Dashboard1)
