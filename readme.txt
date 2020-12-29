Here is an example of creating visualization for a sales dashboard.

Key metrics:
1. Total number of products
2. Average basket size (in piece)
3. Total sales amount
4. Average basket size (in monetary value)
5. YOY, QOQ, MOM sales growth
6. YTD, MTD, QTD (in case the present year data is given)
7. Discount = (price - list price)/ list price 

Key dimensions:
1. Geolocation
2. Product category
3. Date



Example of DAX
Dim Date = 
VAR MinYear = YEAR ( MIN ( 'Sales Figuresall_location'[Order Date]) )
VAR MaxYear = YEAR ( MAX ('Sales Figuresall_location'[Payment Date]) )
RETURN
ADDCOLUMNS (
    FILTER (
        CALENDARAUTO( );
        AND ( YEAR ( [Date] ) >= MinYear; YEAR ( [Date] ) <= MaxYear )
    );
    "Year"; YEAR ( [Date] );
    "Month Name"; FORMAT ( [Date]; "mmmm" );
    "Month Number"; MONTH ( [Date] );
    "Year Month";YEAR ( [Date] ) & "-" & FORMAT ( [Date]; "mm" ) &"-01";
    "Year Month num";YEAR ( [Date] )&FORMAT ( [Date]; "mm" );
    "Weekday"; FORMAT ( [Date]; "dddd" );
    "Weekday number"; WEEKDAY( [Date] );
    "Quarter";  TRUNC ( ( MONTH ( [Date] ) - 1 ) / 3 ) + 1
)

YOY% = 
VAR thisYear =
    SUM ( 'Sales Figuresall_location'[Sales Amount] )
VAR lastYear =
    CALCULATE ( SUM ( 'Sales Figuresall_location'[Sales Amount] );SAMEPERIODLASTYEAR ( 'Dim Date'[Date]) )
RETURN
    DIVIDE ( thisYear - lastYear; lastYear; 0 )
    
    
    
QOQ% = 
VAR thisQ =
    SUM ( 'Sales Figuresall_location'[Sales Amount] )
VAR lastQ =
    CALCULATE (
        SUM ( 'Sales Figuresall_location'[Sales Amount] );
        DATESINPERIOD ( 'Dim Date'[Date]; MIN ( 'Dim Date'[Date] );-1; QUARTER )
    )
RETURN
    DIVIDE ( thisQ - lastQ; lastQ; 0 )
    
    
    
    
    

