# brewmetrics-bi

Version-controlled Power BI solution for BrewMetrics Coffee Co. sales across 4 cities in South India: Bengaluru, Chennai, Hyderabad and Coimbatore.

# Project description

BrewMetrics managers wanted to understand two things from the sales data: the seasonal spike in Cold Brew sales in April-May, and why some cities sell more than others. This project builds a star schema model, 4 DAX measures and a one-page dashboard in Power BI Desktop, saved as a Power BI Project (.pbip) so every change is tracked in Git.

The data covers 1 April to 1 July 2026. July only has one day (1 July), so the dashboard is filtered to April-June.

# How to open
Clone this repository.
Open BrewMetrics.pbip in Power BI Desktop.
If Power BI cannot find the data, go to Transform data > Data source settings and point it to brewmetrics_sales (1).csv in the repo folder, then Refresh.

Keep BrewMetrics.pbip, BrewMetrics.Report and BrewMetrics.SemanticModel together in the same folder.

Data model (star schema)

Fact table:

Fact_Sales: one row per sale (sale_id, date, StoreKey, item, quantity, unit_price, sales_amount)

Dimension tables:

Dim_Date: Date, Month, MonthNo, Day, Weekday (created with DAX, marked as date table)
Dim_Product: item and category (10 items, 3 categories)
Dim_Store: city, store_format and StoreKey (12 stores), with a city > store_format hierarchy for drill-down

# Relationships (many-to-one, single direction):

Fact_Sales[date] -> Dim_Date[Date]
Fact_Sales[item] -> Dim_Product[item]
Fact_Sales[StoreKey] -> Dim_Store[StoreKey]
DAX measures
Total Sales: sum of sales_amount (base measure)
Sales MoM %: month-over-month growth using DATEADD
Running Total: cumulative sales over time, respects slicers (ALLSELECTED)
Item Rank: ranks items by Total Sales with RANKX, blank on the Total row (ISINSCOPE)
Cold Brew Share %: Cold Brew sales as a % of Total Sales


# Dashboard

One page, filtered to April-June:

Cold Brew Share vs Total Sales by Month (column and line chart)
Sales by City, with drill-down to Store Format (bar chart)
Product Ranking by Sales (table with Item Rank)
Cumulative Sales (line chart with Running Total)
Month-over-Month Sales Growth (table)
Slicers: Product Category and City

A PDF export of the dashboard is submitted with the repo link.

# Key findings
Cold Brew is the best-selling item (rank 1). Its share of sales is about 21% in April and May and drops to 15.5% in June, which confirms the April-May peak.

Bengaluru has the highest sales (about 1.11M) and Coimbatore the lowest (about 0.82M), a gap of roughly 290K over the quarter.
Flagship stores sell the most and Kiosks the least.

Total sales grew 9.5% in May and fell 16.2% in June, in line with the end of the Cold Brew season.
Repo structure

brewmetrics-bi/
├─ BrewMetrics.pbip             Power BI project file (open this)
├─ BrewMetrics.Report/          Report pages and visuals
├─ BrewMetrics.SemanticModel/   Tables, relationships and DAX measures (TMDL)
├─ brewmetrics_sales (1).csv    Source data
├─ NOTES.md                     Copilot suggestions and my corrections
├─ REFLECTION.md                Reflection on the project
└─ README.md

# Author
Joyce Laetitia Malenou Weudji - 24ADC001 Business Intelligence, KCT