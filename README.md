# Kultra-Mega-Stores-Inventory-Analysis
## 📝 Overview
This project was focused on analyzing past sales data from Kultra Mega Stores (KMS), a company that sells office supplies and furniture across different customer types — individual, small businesses, and corporate clients. I was asked to look into the Abuja division using the order records from 2009 to 2012. My task was to go through the Excel file, run some SQL queries, and bring out useful insights that can help the business improve sales and manage costs better.
## 🎯 OBJECTIVES
* Which product category brought in the most sales
* The top 3 and bottom 3 regions in terms of sales
* Total appliance sales in Ontario
* Suggestions on how to increase revenue from low-performing customers
* Which shipping method cost the most
* Who the most valuable customers are and what they usually buy
* Small business customer with the highest sales
* Corporate customer that placed the most orders
* Consumer customer with the highest profit
* Customers who returned items and the segment they belong to
* Whether shipping choices made sense compared to order priority
  ## 🧹 DATA PREPARATION
* Cleaned up the Excel file and made sure all the columns were readable
* Used SQL to run the main queries (GROUP BY, SUM, COUNT, ORDER BY)
* Used Excel for quick checks, visualizations, and totals
* Filtered, grouped, and sorted the data to make it easier to find answers
  ## 🔍 My Findings
### Case Scenario I
1. *Top Category by Sales*
  * One category clearly outperformed others based on total sales.
2. *Top 3 and Bottom 3 Regions*## 🧹 What I Did First
 * Cleaned up the Excel file and made sure all the columns were readable
 * Used SQL to run the main queries (GROUP BY, SUM, COUNT, ORDER BY)
 * Used Excel for quick checks, visualizations, and totals
 * Filtered, grouped, and sorted the data to make it easier to find answers
 * I listed all regions by total sales and picked the highest and lowest three.
3. *Appliances in Ontario*
 * Found and added up only the appliance sales in Ontario — this was straightforward.
4. *Bottom 10 Customers – Suggestions*
 * Some customers barely ordered anything. I suggested things like discounts, bundle offers, or follow-ups to boost their interest.
5. *Highest Shipping Cost*
 * One shipping method was responsible for most of the total shipping cost — could be re-evaluated.
### Case Scenario II
6. *Most Valuable Customers*
 * Found based on total money spent. Also noted what they usually buy.
7. *Small Business with Highest Sales*
 * Picked out the customer in the small business segment with the highest purchase total.
8. *Corporate Client with Most Orders*
 * Checked order counts and found the corporate client that ordered the most from 2009–2012.
9. *Most Profitable Consumer*
 * Looked at profit column to find the top consumer.
10. *Returned Items*
 * Pulled all customers who returned something and checked their segment.
11. *Shipping vs. Order Priority*
 * Compared shipping methods with the urgency level of the order.
 * Delivery Truck is cheap but slow
  * Express Air is fast but expensive
  * Found a few cases where the expensive option was used for a low-priority order — might be wasting money there.
## INSIGHTS
---1. PRODUCT CATEGORY WITH THE HIGHEST SALES-----
SELECT TOP 1 Product_Category, SUM(Sales) AS Total_Sales
FROM KMS_Inventory
GROUP BY Product_Category
ORDER BY Total_Sales DESC

----2. TOP 3 AND BUTTOM 3REGION IN TERMS OF SALES-----
---TOP 3 REGION--
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM KMS_Inventory
GROUP BY Region
ORDER BY Total_Sales DESC
---BUTTOM 3--
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM KMS_Inventory
GROUP BY Region
ORDER BY Total_Sales ASC

---3. TOTAL OF SALE OF APPLIANCES IN ONTARIO---
SELECT SUM(SALES) AS Total_Sales 
FROM KMS_Inventory
WHERE Product_Category = 'Appliance'
AND Region = 'Ontario'

---4. ADVISE TO INCREASE REVENUE FOR THE BUTTOM 10 CUSTOMER----
SELECT TOP 10 Customer_Name, 
SUM(Sales) AS Lowest_Customer_Sales, 
SUM(Profit) AS Total_Profit,
SUM(Shipping_Cost) AS Total_Shipping_Cost,
SUM(Discount) AS Total_Discount,
COUNT(Order_ID) AS Total_Order
FROM KMS_Inventory
GROUP BY Customer_Name
ORDER BY Lowest_Customer_Sales ASC

---5. WHICH SHIPPING METHOD INCURED THE MOST SHIPPING COST----
SELECT TOP 1 Ship_Mode, COUNT(Shipping_Cost) AS Highest_Shipping_Cost
FROM KMS_Inventory
GROUP BY Ship_Mode
ORDER BY Highest_Shipping_Cost ASC

---6. MOST VALUABLE CUSTOMERS AND PRODUCTS OR SERVICES FREQUESTLY PURCHASED
-----STEP 1: GETING THE TOP 5 CUSTOMERS AND HOW MUCH THEY SPENT---
SELECT TOP 5 Customer_Name,
SUM(Sales) AS Highest_Spent,
COUNT(Order_ID) AS Total_Order
FROM KMS_Inventory
GROUP BY Customer_Name
ORDER BY Highest_Spent DESC

----STEP 2: USING SUBQUERRY TO JOIN THE TOP CUSTOMERS AND WHAT TEHY PURCHASED----
WITH Top_Customers AS
(SELECT TOP 5
Customer_Name,
SUM(Sales) AS Total_Spent
FROM KMS_Inventory ki
GROUP BY Customer_Name
ORDER BY Total_Spent DESC
)
SELECT ki.Customer_Name,
		ki.Product_Name,
COUNT(*) AS Purchase_Count,
SUM(ki.Sales) AS Total_Spent_On_Products
FROM KMS_Inventory ki
JOIN Top_Customers tc 
ON ki.Customer_Name = tc.Customer_Name
GROUP BY ki.Customer_Name, ki.Product_Name
ORDER BY Total_Spent_On_Products DESC

------7. SMALL BUSINESS CUSTOMERS WITH THE HIGHEST SALES----
SELECT TOP 1 Customer_Name, SUM(Sales) AS Total_Sales
FROM KMS_Inventory
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name
ORDER BY Total_Sales DESC

----8. CORPORATE CUSTOMER THAT PLACED THE MOST ORDER BETWEEN 2009 AND 2012
SELECT TOP 1 Customer_Name, 
COUNT (Order_ID) AS Total_Orders
FROM KMS_Inventory
WHERE Customer_Segment = 'Corporate' AND 
Order_Date BETWEEN '2009-01-01' AND '2012-01-01'
GROUP BY Customer_Name
ORDER BY Total_Orders DESC

-----9. THE MOST PROFITABLE CONSUMER CUSTOMER----
SELECT TOP 1 Customer_Name, SUM(Sales) AS Total_Sales
FROM KMS_Inventory
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY Total_Sales DESC

----10. CUSTOMERS THAT RETURNED ITEMS ANE WHICH SEGMENT THEY BELONG TO----
SELECT Customer_Name,
Customer_Segment
FROM KMS_Inventory
WHERE Sales < 0

----11. COMPARISON BETWEEN SHIPPING MODE(EXPRESS AIIR AND DELIVERY TRUCK) AS REGARDSSHIPPING COST AND ORDER PRIORITY---
SELECT Ship_Mode,
Order_Priority,
COUNT(*) AS Total_Orders,
SUM(Shipping_Cost) AS Total_Shipping_Cost,
AVG(Shipping_Cost) AS Average_Shipping_Cost
FROM KMS_Inventory
WHERE Ship_Mode IN ('Express Air', 'Delivery Truck')
GROUP BY Ship_Mode, Order_Priority
ORDER BY Ship_Mode, Order_Priority
## 📊 Tools I Used
* *SQL* – For querying and finding totals, groupings, and rankings
* *Excel* – For visuals (charts and tables) and filtering data
* *Charts Used*:
 * Bar charts for region performance
 * Tables for customer insights
 * Slicers for easier filtering
   ## ✅ My Suggestions
* Promote products in the top-selling category
* Improve sales in weaker regions with better offers
* Talk to low-paying customers and offer deals to get them to order more
* Watch shipping costs — use faster methods only when necessary
* Track return patterns and maybe improve product handling or quality
* Keep strong customers happy with better support or loyalty perks
  ## 📁 Files I Worked On
* *KMS\_InventoryAnalysis.xlsx* – for final outputs and visualizations
* *SQL\_Queries\_KMS.sql* – all the queries I used for analysis














  


