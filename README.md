Sales, Inventory & Fulfillment Performance Dashboard (Tableau)

An end-to-end business intelligence project that analyzes order, shipment, inventory and warehouse fulfillment data (2015–2017) to surface sales performance, profitability, delivery reliability and stock-availability insights in an interactive Tableau dashboard.

Table of Contents
Project Overview
Business Objectives
Dataset Description
Tools and Technologies
Key Business KPIs
Data Preparation Notes
Dashboard Structure
How to Use
Key Findings
Future Enhancements
Project Overview

This project transforms three operational datasets into a single, decision-ready Tableau dashboard. The goal is to give business stakeholders one view of how the company sells, ships, and stocks its products, so they can spot profitability drivers, delivery bottlenecks, and inventory gaps without manually reconciling spreadsheets.

The analysis covers 30,871 order line items placed between January 2015 and December 2017, across 113 products, 7,987 customers, and five customer markets.

Business Objectives
Monitor revenue and profitability over time, by product department, market, and region.
Measure delivery performance by comparing actual shipment timing against scheduled shipping commitments for each shipment mode.
Evaluate warehouse fulfillment speed across the product catalogue to identify slow-moving operational bottlenecks.
Track inventory health to reduce stockouts and understand the cost of carrying stock.
Support pricing and discount decisions by understanding how discount levels relate to order value and profit.
Dataset Description

The project uses three source files. Each file is joined on the Product Name field where cross-dataset analysis is required.

1. orders_and_shipments.csv (30,871 rows × 24 columns)

Transaction-level data for each order line item, including order timing, customer geography, product hierarchy, shipment details, and financial measures.

Column group	Fields
Identifiers	Order ID, Order Item ID, Customer ID
Order timing	Order YearMonth, Order Year, Order Month, Order Day, Order Time
Product	Product Department, Product Category, Product Name
Customer	Customer Market, Customer Region, Customer Country
Logistics	Warehouse Country, Shipment Year/Month/Day, Shipment Mode, Shipment Days – Scheduled
Financial	Order Quantity, Gross Sales, Discount %, Profit
2. fulfillment.csv (118 rows × 2 columns)

Average warehouse order fulfillment time per product.

Field	Description
Product Name	Product identifier
Warehouse Order Fulfillment (days)	Average number of days the warehouse takes to fulfill an order for that product
3. inventory.csv (4,200 rows × 4 columns)

Monthly warehouse stock position per product from January 2015 to December 2017 (36 months × 118 products).

Field	Description
Product Name	Product identifier
Year Month	Period in YYYYMM format
Warehouse Inventory	Units in stock at month end
Inventory Cost Per Unit	Unit cost of the inventory held
Tools and Technologies
Tool	Purpose
Tableau (Desktop / Public)	Dashboard design, calculated fields, interactivity, and visualization
Python (pandas)	Data profiling, cleaning, encoding fixes, and KPI validation
Microsoft Excel / CSV	Source data format and quick reconciliation checks
Git / GitHub	Version control and project documentation
Key Business KPIs

All figures below are computed from the uploaded source files. Currency values are shown in the units recorded in the dataset.

Sales and Profitability
KPI	Definition	Value
Total Gross Sales	Sum of Gross Sales across all order lines	6.18M
Total Profit	Sum of Profit across all order lines	3.32M
Profit Margin %	Total Profit ÷ Total Gross Sales	53.7%
Total Orders	Count of distinct Order IDs	11,072
Total Units Sold	Sum of Order Quantity	66,367
Average Order Value (AOV)	Total Gross Sales ÷ Total Orders	558.30
Average Profit per Order	Total Profit ÷ Total Orders	299.63
Active Customers	Count of distinct Customer IDs	7,987
Average Discount %	Mean of Discount % (valid records)	10.7%
Year-over-Year Gross Sales Growth	(Current year − Prior year) ÷ Prior year	2016: +4.8%, 2017: −16.2%
Delivery and Fulfillment
KPI	Definition	Value
On-Time Delivery Rate	Share of shipments where actual ship time ≤ scheduled days	56.5%
Late Delivery Rate	Share of shipments where actual ship time > scheduled days	43.5%
Late Rate by Shipment Mode	Late share calculated per shipping method	First Class 89.7%, Second Class 56.7%, Standard Class 29.0%, Same Day 21.9%
Average Warehouse Fulfillment Time	Mean of Warehouse Order Fulfillment (days) across products	5.3 days
Scheduled Shipping Days (average)	Mean of Shipment Days – Scheduled	3.1 days
Inventory
KPI	Definition	Value
Average Monthly Inventory Value	Monthly sum of (Warehouse Inventory × Unit Cost), averaged over 36 months	2,400.86
Stockout Rate	Share of product-months with zero warehouse inventory	60.1%
Average Units on Hand per Product-Month	Mean of Warehouse Inventory	16.9 units
Suggested Additional KPIs for the Dashboard
Inventory Turnover = Units Sold ÷ Average Inventory Units, by product
Profit by Product Department and Market (for drill-down analysis)
Discount Impact: Average Profit per Order by Discount % band
Data Preparation Notes

The following steps were taken during data preparation:

Encoding: orders_and_shipments.csv is saved in Latin-1 (Windows-1252) encoding, not UTF-8. It must be read with encoding='latin-1' (or the equivalent setting in Tableau's data source dialog).
Header cleanup: Several column names contain leading or trailing spaces (for example, " Order ID ", " Customer ID "). Headers were trimmed before joining or calculating.
Date construction: Order and shipment dates were built from their separate Year, Month, and Day columns to calculate actual shipping duration.
Discount field: Approximately 1,749 records in Discount % contain values that do not parse as numbers. These were excluded from the average discount calculation. These rows should be reviewed at the source.
Late delivery calculation: Late status = (Shipment Date − Order Date) > Shipment Days – Scheduled.
Product key: Product Name is used as the join key across the three files. All 118 products in fulfillment.csv and inventory.csv match the product catalogue in the orders data.

Data quality flag: The average actual shipping duration calculated from order and shipment dates (about 22.4 days) is much higher than the average scheduled duration (about 3.1 days). This gap should be validated with the data owner before the late-delivery KPI is used for operational decisions, since it may reflect how the shipment date field was recorded.

Dashboard Structure

The Tableau workbook is organized into the following views:

Executive Summary: headline KPIs (Gross Sales, Profit, Margin, Orders, AOV) with year-over-year comparison.
Sales Trend: monthly and yearly gross sales and profit, with a trend line.
Product & Department Performance: profit and sales by department, category, and product, with top-N filters.
Market & Region Analysis: a map and bar charts showing sales and profit by customer market and region.
Delivery Performance: on-time vs. late shipments by shipment mode and market.
Fulfillment Speed: warehouse fulfillment days by product, highlighting the slowest items.
Inventory Health: monthly inventory value, stockout trend, and products with persistent zero stock.

Global filters for Year, Market, Department, and Shipment Mode apply across all views.

How to Use
Clone or download this repository.
Open the Tableau workbook ([workbook_name].twbx) in Tableau Desktop or Tableau Public.
If the data sources show a missing-file warning, re-point each source to the CSV files in the data/ folder (adjust the path to match your repository layout).
Use the filters at the top of the dashboard to narrow by year, market, department, or shipment mode.
Hover over any mark to see tooltips with the underlying values.

Note: If the orders file fails to load, confirm the encoding is set to Latin-1 in the data connection settings.

Key Findings
Revenue declined in 2017. Gross Sales fell 16.2% from 2016, after a modest 4.8% increase the year before. Profit followed the same pattern.
Fan Shop is the largest profit driver, contributing about 1.51M of the 3.32M total profit.
LATAM and Europe together account for roughly 58% of total profit, making them the two most important markets.
First Class shipments are late 89.7% of the time, which suggests the premium shipping promise is not being met operationally.
Stock availability is a major concern. Warehouse stock was zero in 60.1% of product-months, which points to a material risk of lost sales.
Discounts are moderate on average (10.7%). The relationship between discount level and profit per order is worth exploring further in the discount analysis view.
Future Enhancements
Add a demand forecast by product using time-series models (for example, Prophet or ARIMA in Python).
Connect the Tableau data source to a live database rather than static CSV exports.
Introduce a reorder-point and safety-stock calculation to reduce stockouts.
Add a customer segmentation view using RFM (Recency, Frequency, Monetary) analysis.
Validate and correct the shipment date field, then re-run the late-delivery analysis.
Author

[Your Name] [Your role / degree and institution] [Your email or LinkedIn]

License

This project is intended for educational and portfolio purposes. Add a license file (for example, MIT) if you plan to share it publicly.# SALES-VS-DEMAND
