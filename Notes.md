NOTES - DAX measures with Copilot

I used Copilot Chat  in VS Code , and asked for all 4 measures in one prompt. Then I created each one in Power BI and tested it in a table before committing.

# Measure 1 - Total Sales and Sales MoM %

Copilot gave me:

Total Sales =
SUM ( Fact_Sales[sales_amount] )

Sales MoM % =
VAR CurrentSales = [Total Sales]
VAR PrevMonthSales =
    CALCULATE( [Total Sales], DATEADD( Dim_Date[Date], -1, MONTH ) )
RETURN
    DIVIDE( CurrentSales - PrevMonthSales, PrevMonthSales )

 I used MoM because there is only one year of data. Copilot didn't mention that July only has 1 day in the data, so July shows -13% (it compares 1 July with 1 June only). I filter July out of the visuals.

# Measure 2 - Running Total

Copilot gave me:

Running Total =
CALCULATE(
    [Total Sales],
    FILTER( ALLSELECTED( Dim_Date[Date] ), Dim_Date[Date] <= MAX( Dim_Date[Date] ) )
)

Worked first time, no changes. ALLSELECTED keeps the slicer filters, so it still works when a city is selected. June reaches 3 926 080,33.

# Measure 3 - Item Rank (RANKX) - real CORRECTION

Copilot gave me:

Item Rank =
RANKX( ALLSELECTED( Dim_Product[item] ), [Total Sales], , DESC, DENSE )

The Total row  showed rank 1, which makes no sense. I fixed it with ISINSCOPE so it only ranks when an item is on the row


# CORECTED :
Item Rank =
IF (
    ISINSCOPE ( Dim_Product[item] ),
    RANKX ( ALLSELECTED ( Dim_Product[item] ), [Total Sales], , DESC, DENSE )
)

Now the Total row is empty.

# Measure 4 - Cold Brew Share % (my own choice) CORRECTION

Copilot gave me 2 measures:

Cold Brew Sales =
CALCULATE(
    [Total Sales],
    FILTER( ALLSELECTED( Dim_Product ), CONTAINSSTRING( LOWER( Dim_Product[item] ), "cold brew" ) )
)

Cold Brew Share % =
DIVIDE( [Cold Brew Sales], [Total Sales] )

I tested them and the numbers were right, but it was more complicated than needed. It searches the text "cold brew" in the item name, so a future item like "Cold Brew Tonic" would also be counted. I deleted the helper and used one measure with an exact match:

# CORRECTED:
Cold Brew Share % =
DIVIDE (
    CALCULATE ( [Total Sales], Dim_Product[item] = "Cold Brew" ),
    [Total Sales]
)

Same results: April 21.2%, May 21.1%, June 15.5%, so the Cold Brew peak is in April-May.

