# **Kultra Mega Stores Inventory Report**
*Disclaimer: The data is for educational and analytical purposes only.*

![472b2671-5659-4fae-adf2-61d0eb9e1b93](https://github.com/user-attachments/assets/82960bea-3887-4da5-925b-e7ecd1fe1c2f)

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
1. Verified and corrected data types (e.g., converted Order ID to INT, Unit Price and Profit to DECIMAL(18,2)).
2. Checked and confirmed no duplicate records.
3. Assigned Row ID as the primary key, allowing nulls in other columns to support data import.
4. Allowed null values in the Product Base Margin column.

## **VI. Findings & Insights**
### 1. **Product Category with the Highest Sales**
```sql
SELECT TOP 1 product_category, SUM(sales) AS category_sales
FROM kms_table
GROUP BY Product_Category
ORDER BY category_sales DESC
```

* **Category:** Technology
* **Total Sales:** ₦5,984,248.18

Insight: *Technology is the most profitable category.*

### 2. **Top & Bottom 3 Regions by Sales**

**Top 3 Regions:**
```sql
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
SELECT TOP 3 customer_name, COUNT(customer_name) AS customer_appearing_times, SUM (profit) AS customer_profit
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
Emily Phan                          |Bell Sonecor JB700 Caller ID                                           |34005.44                  
  "                                 |Boston 1730 StandUp Electric Pencil Sharpener                          |
  "                                 |Fellowes Bankers Box™ Stor/Drawer® Steel Plus™                         |
  "                                 |Harmony HEPA Quiet Air Purifiers                                       |
  "                                 |Heavy-Duty E-Z-D® Binders                                              |
  "                                 |Hewlett-Packard Deskjet 1220Cse Color Inkjet Printer                   |
  "                                 |O'Sullivan Elevations Bookcase, Cherry Finish                          |
  "                                 |Polycom ViewStation™ ISDN Videoconferencing Unit                       |
  "                                 |Surelock™ Post Binders 34005.44                                        |
  "                                 |TimeportP7382 34005.44                                                 |
Deborah Brumfield     	             |Acme® 8" Straight Scissors       	                                     |31121.22
"     	                             |Avery 51          	                                                    
"                  	                |DAX Contemporary Wood Frame with Silver Metal Mat, Desktop, 11 x 14 Size 	
"                   |Global High-Back Leather Tilter, Burgundy        	
"     	             |Gyration Ultra Professional Cordless Optical Suite         	
"     	             |Hewlett Packard LaserJet 3310 Copier	
"     	             |Hon GuestStacker Chair            	
"     	             |Hoover® Commercial Lightweight Upright Vacuum       	
"     	             |Hunt BOSTON® Vista® Battery-Operated Pencil Sharpener, Black          	
"     	             |Iceberg Mobile Mega Data/Printer Cart ®         	
"     	             |KF 788   
"     	             |Letter Size Cart 
"             	     |M3682  
"     	             |Memorex 4.7GB DVD-RAM, 3/Pack     	
"     	             |Panasonic KX-P3626 Dot Matrix Printer  
"                   |SAFCO Folding Chair Trolley     	
"     	             |Talkabout T8367           	
"     	             |Tennsco Regal Shelving Units 	
"     	             |Tuff Stuff™ Recycled Round Ring Binders          	
"    	              |V70    	
Grant Carroll  	                  |Accessory23   	                                                         |27977.29
"                  	  |Avery 506        	
"  	                  |Bell Sonecor JB700 Caller ID     	
"  	                  |Bush Heritage Pine Collection 5-Shelf Bookcase, Albany Pine Finish, *Special Order      	
"  	                  |Col-Erase® Pencils with Erasers  
"  	                  |Crate-A-Files™  
"  	                  |Eldon Expressions Punched Metal & Wood Desk Accessories, Black & Cherry   	
"  	                  |Fellowes PB300 Plastic Comb Binding Machine   
"  	                  |Fellowes PB500 Electric Punch Plastic Comb Binding Machine with Manual Bind  
"  	                  |GBC Recycled Regency Composition Covers    	
"  	                  |Heavy-Duty E-Z-D® Binders     	
"  	                  |Imation 3.5", RTS 247544 3M 3.5 DSDD, 10/Pack 
"  	                  |Kensington 7 Outlet MasterPiece Power Center            	
"  	                  |Lifetime Advantage™ Folding Chairs, 4/Carton   
"  	                  |Logitech Internet Navigator Keyboard	
"  	                  |Microsoft Natural Multimedia Keyboard           	
"  	                  |R280  	
"  	                  |Rediform Wirebound "Phone Memo" Message Book, 11 x 5-3/4           	
"  	                  |Rush Hierlooms Collection Rich Wood Bookcases          	
"  	                  |SAFCO Arco Folding Chair         	
"  	                  |Sanford Prismacolor® Professional Thick Lead Art Pencils, 36-Color Set   
"  	                  |Staples SlimLine Pencil Sharpener        	
"  	                  |Verbatim DVD-R, 4.7GB, Spindle, WE, Blank, Ink Jet/Thermal, 20/Spindle          
"  	                  |Xerox 1906      	
"  	                  |Xerox 1966      	
"  	                  |Xerox 197        	
"  	                  |Xerox 213        	

Insight: *The top three customers Emily Phan (₦34,005.44), Deborah Brumfield (₦31,121.22), and Grant Carroll (₦27,977.29) consistently purchase a wide range of premium office equipment and electronics.*

### 7. **Small business customer that had the highest sales**
```sql
SELECT TOP 1 customer_name, customer_segment, SUM(sales) AS total_sales
FROM kms_table
GROUP BY Customer_Segment, customer_name
HAVING customer_segment = 'Small Business'
ORDER BY total_sales DESC
```

* **Customer:** Dennis Kane
* **Sales:** ₦75,967.59

### 8. **corporate customer that placed the most number of orders in 2009-2012**
```sql
SELECT TOP 1 customer_name, customer_segment, SUM(Order_Quantity) AS total_order, Order_Date
FROM kms_table
GROUP BY Customer_Segment, customer_name, Order_Date
HAVING customer_segment = 'corporate' AND Order_Date BETWEEN '2009-01-01' AND '2012-12-31'
ORDER BY total_order DESC
```
Findings
* **Customer:** Laurel Elliston
* **Orders:** 148
Insight: *Demonstrates loyalty and purchasing consistency.*

### 9. **Most Profitable Consumer Customer**
```sql
SELECT TOP 1 customer_name, customer_segment, SUM(profit) AS total_profit
FROM kms_table
GROUP BY Customer_Segment, customer_name
HAVING customer_segment = 'consumer'
ORDER BY total_profit DESC
```
Findings:
* **Customer:** Emily Phan
* **Profit:** ₦34,005.44
Insight: *High-value individual customer, suitable for loyalty programs.*

### 10. **Customer that returned items, and the segment to which they belong**
--**Assuming negative profit means returns**
```sql
SELECT TOP 10 customer_name, customer_segment, COUNT(*) AS number_of_returns
FROM kms_table
WHERE Profit <= 0
GROUP BY Customer_Segment, customer_name
ORDER BY number_of_returns DESC
```
Findings
Customer_name                       |customer_segment                   |number_of_returns
:-----------------------:|:-------------------------------:|:----------------------------------------:
Patrick Jones                       |Home Office                     |18 
Brad Thomas                         |Home Office                     |17
Christy Brittain 	                  |Consumer  	                     |16
Ed Braxton	                         |Home Office                     |16
Justin Knight                       |Corporate 	                     |15
Adam Hart 	                         |Corporate 	                     |14
Roy Skaria	                         |Corporate 	                     |14
Denise Monton                       |Home Office                     |13
Doug Bickford                       |Corporate 	                     |13
John Lee  	                         |Corporate 	                     |13

* **Top Returner:** Patrick Jones (18 returns)
* **Segments:** Mostly Home Office and Corporate

### 11. **Did the company appropriately spend shipping cost based on order priority**
```sql
SELECT Order_Priority, ship_mode, COUNT(*) AS num_orders, SUM(shipping_cost) AS total_shipping_cost
FROM kms_table
GROUP BY Order_Priority, Ship_Mode
ORDER BY Order_Priority, Ship_Mode
```

order_priority                       |ship_mode                   |num_orders                     |total_shipping_cost
:-----------------------:|:-------------------------------:|:----------------------------------------:|:------------------------------------:
Critical                       |Delivery Truck                     |228                                |10783.8199481964 	   		
Critical  	                    |Express Air                        |200	                               |1742.09998804331
Critical  	                    |Regular Air                        |1180                               |8586.75996172428
High                           |Delivery Truck   	                 |248	                               |11206.8799371719
High                           |Express Air                        |212	                               |1453.5299910903
High                           |Regular Air                        |1308                               |10005.0099598169
Low	                           |Delivery Truck   	                 |250	                               |11131.6099338531
Low	                           |Express Air                        |190	                               |1551.62999778986
Low	                           |Regular Air                        |1280                               |10263.619956553
Medium                         |Delivery Truck   	                 |205	                               |9461.61997509003
Medium                         |Express Air                        |201	                               |1633.58999282122
Medium                         |Regular Air                        |1225                               |9418.71996569633
Not Specified                  |Delivery Truck   	                 |215	                               |9388.00994300842
Not Specified                  |Express Air                        |180	                               |1470.05999219418
Not Specified                  |Regular Air                        |1277                               |9734.07996362448

Insight: No, the company did not appropriately spend shipping cost based on order priority. "Critical" and "High" priority orders are being shipped via Delivery Truck rather than Express Air. This suggests a misalignment between priority level and shipping method.
 
## **VII. Recommendations**
1. Improve profitability from bottom 10 customers by limiting discounts and enforcing better return policies.
2. Double down on Technology and West/Ontario sales via promotions or inventory expansion.
3.  Review why costlier delivery is being used for lower-priority shipments (e.g., "Low" via Delivery Truck at ₦11k+).
4. Introduce smarter logistics planning — especially in aligning shipping cost with priority.
5. Retain top customers through reward programs and personalized engagement.
6. Target high-performing segments (e.g., small business and corporate) with customized deals.
7. Use insights from returns to fix product or delivery issues.
  
## **VII. Conclusion**
Kultra Mega Stores has a strong foundation with high-value customers and standout product categories. However, profitability is hampered by inefficiencies in shipping practices, high return rates, and a group of unprofitable customers. By refining logistics, targeting top segments, and enforcing smarter sales policies, KMS can significantly boost revenue and long-term performance.























