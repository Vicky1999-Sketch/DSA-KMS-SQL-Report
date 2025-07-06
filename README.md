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
ORDER BY category_sales DESC```

* **Category:** Technology
* **Total Sales:** ₦5,984,248.18

Insight: *Technology is the most profitable category.*



