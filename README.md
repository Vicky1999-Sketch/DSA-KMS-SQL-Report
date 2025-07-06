# **Kultra Mega Stores Inventory Report**
*Disclaimer: The data is for educational and analytical purposes only.*

## **I. Introduction**
Kultra Mega Stores (KMS), headquartered in Lagos, specializes in office supplies and furniture. Its customer base includes individual consumers, small businesses (retail), and large corporate clients (wholesale) across Lagos, Nigeria. This report contains key insights and findings from data analysis conducted using SQL queries.

## **II. Data Description**
The dataset comprises a single table containing 21 columns:
[Row ID, Order ID, Order Date, Order Priority, Order Quantity, Sales, Discount, Ship Mode, Profit, Unit Price, Shipping Cost, Customer Name, Province, Region, Customer Segment, Product Category, Product Subcategory, Product Name, Product Container, Product Base Margin, Ship Date]
Total records: 8,399 rows.

## **III. Problem Statement**
The analysis aims to answer the following questions:
1. Which product category had the highest sales?
2. What are the Top 3 and Bottom 3 regions in terms of sales?
3. What were the total sales of appliances in Ontario?
4. Advise the management of KMS on what to do to increase the revenue from the bottom
10 customers
5. KMS incurred the most shipping cost using which shipping method?
6. Who are the most valuable customers, and what products or services do they typically
purchase?
7. Which small business customer had the highest sales?
8. Which Corporate Customer placed the most number of orders in 2009 – 2012?
9. Which consumer customer was the most profitable one?
10. Which customer returned items, and what segment do they belong to?
11. If the delivery truck is the most economical but the slowest shipping method and
Express Air is the fastest but the most expensive one, do you think the company
appropriately spent shipping costs based on the Order Priority? Explain your answer

## **IV. Methodology**
Microsoft SQL Server was used for transforming and analyzing the dataset using a series of structured queries.

## **V. Data Transformation & Cleaning**
Key transformation steps included:
1. Verified and corrected data types (e.g., converted `Order ID` to `INT`, `Unit Price` and `Profit` to `DECIMAL(18,2)`).
2. Checked and confirmed no duplicate records.
3. Assigned `Row ID` as the primary key, allowing nulls in other columns to support data import.
4. Allowed null values in the `Product Base Margin` column.

## **VI. Findings & Insights**
### 1. **Product Category with the Highest Sales**
```sql
SELECT TOP 1 product_category, SUM(sales) AS category_sales
FROM kms_table
GROUP BY Product_Category
ORDER BY category_sales DESC
```

**Category:** Technology
**Total Sales:** ₦5,984,248.18

Insight: *Technology is the most profitable category.*

### 2. **Top & Bottom 3 Regions by Sales**

**Top 3 Regions:**
```sql
--Top 3
SELECT TOP 3 region, SUM(sales) AS region_sales
FROM kms_table
GROUP BY region
ORDER BY region_sales DESC
```
Findings
* West – ₦3,597,549.27
* Ontario – ₦3,063,212.48
* Prairie – ₦2,837,304.61

**Bottom 3 Regions:**
```sql
SELECT TOP 3 region, SUM(sales) AS region_sales
FROM kms_table
GROUP BY region
ORDER BY region_sales ASC
```
Findings
* Nunavut – ₦116,376.48
* Northwest Territories – ₦800,847.33
* Yukon – ₦975,867.38

Insight: *Significant sales disparity exists across regions, highlighting geographic concentration.*

### 3. **Total sales of appliances in Ontario**
```sql
SELECT region, product_sub_category, SUM(sales) AS total_sales_appliances
FROM kms_table
GROUP BY region, Product_Sub_Category
HAVING region = 'Ontario' AND Product_Sub_Category = 'appliances'
```
Findings
**Total Sales:** ₦202,346.84
Insight: *A relatively small portion of total Ontario sales.*

### 4. **Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers**
**Bottom 10 Customers by Profit**
```sql
SELECT TOP 10 Customer_Name, SUM(profit) AS revenue, COUNT(customer_name) AS no_of_returns
FROM kms_table
GROUP BY Customer_Name
ORDER BY revenue ASC
```
Findings:
Customer_name                       |revenue                   |no_of_returns
:-----------------------:|:-------------------------------:|:----------------------------------------:
Julia West                          |-14154.29                |10
Dave Kipp 	                        |-12930.59 	              |14
Laurel Workman   	                  |-12587.05 	              |7
Adrian Barton                       |-11853.03 	              |5
Lauren Leatherbury                  |-11152.83 	              |24
Roger Demir                         |-10187.92 	              |9
Roy Phan  	                        |-9613.99  	              |18
Tamara Willingham	                  |-9238.29                	|16
Cyra Reiten                         |-8529.67  	              |6
Corey Lock	                        |-8392.04  	              |10
 
Advise: All 10 customers have negative profit, implying high return rates, deep discounts, or costly service. These customers may be draining profitability despite frequent orders. Investigate their purchase patterns, returns, and pricing—possibly migrate them to prepaid plans or remove excessive discounts.

### 5. **The shipping method that incurred the most shipping cost**
```sql 
SELECT TOP 1 ship_mode, SUM(shipping_cost) AS most_shipping_cost
FROM kms_table
GROUP BY ship_mode
ORDER BY most_shipping_cost DESC
```
Finding
* **Mode:** Delivery Truck
* **Cost:** ₦51,971.94

### 6. **Most valuable customers and the products they purchase**
 **Customer value determined by profits from their purchase**
```sql 
WITH valuable_customers AS(
SELECT TOP 5 customer_name, COUNT(customer_name) AS customer_appearing_times, SUM (profit) AS customer_profit
FROM kms_table
GROUP BY customer_name
ORDER BY customer_profit DESC
)
SELECT v.customer_name, k.product_name, SUM(v.customer_profit) AS customer_total_profit
FROM kms_table k 
RIGHT JOIN valuable_customers v
ON k.customer_name = v. customer_name
GROUP BY v.customer_Name, k.product_Name, v.customer_profit
ORDER BY customer_profit DESC
```
Findings
Customer_name                       |product_name                   |customer_total_profit
:-----------------------:|:-------------------------------:|:----------------------------------------:
Emily Phan                          |Bell Sonecor JB700 Caller ID                |34005.44
                                    |Bell Sonecor JB700 Caller ID                |      	
**Top 3 Customers:**

* Emily Phan (₦34,005.44)
* Deborah Brumfield (₦31,121.22)
* Grant Carroll (₦27,977.29)

✅ *They consistently purchase a wide range of premium office equipment and electronics.*











