```DAX 
Gross Sales MoM% Dynamic = 

VAR _LatestVisiblePeriod = MAX( DIM_Date[Reporting Period] )

VAR _CurrentMonthStart = 
    CALCULATE(
        MIN( DIM_Date[Date] ), 
        DIM_Date[Reporting Period] = _LatestVisiblePeriod 
    )

VAR _PreviousMonthSales= 
    CALCULATE(
        [Gross Sales], 
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _CurrentMonthStart, -2 ) + 1, 
            EOMONTH( _CurrentMonthStart, -1 ) 
        )
    )

VAR _CurrentMonthSales = 
    CALCULATE(
        [Gross Sales], 
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _CurrentMonthStart, -1 ) + 1, 
            EOMONTH( _CurrentMonthStart, 0 ) 
        )
    )

VAR _Result =
IF(
    _PreviousMonthSales  > 0, 
    (_CurrentMonthSales  - _PreviousMonthSales ) / _PreviousMonthSales, BLANK() )

RETURN _Result
```

```DAX
Gross Sales MoM% Dynamic Text = 

VAR _MoM = [Gross Sales MoM% Dynamic]

VAR _Result = 
SWITCH(TRUE(),
    _MoM = BLANK(),"",
    _MoM < 0,"▼ "&FORMAT( _MoM,"0.0"&"%" ),
    _MoM >= 0,"▲ "&FORMAT( _MoM,"0.0"&"%" )
)

RETURN _Result
```
```DAX
Gross Sales YoY% Dynamic = 

VAR _LatestVisiblePeriod = MAX( DIM_Date[Reporting Period] )

VAR _CurrentMonthStart = 
    CALCULATE(
        MIN( DIM_Date[Date] ), 
        DIM_Date[Reporting Period] = _LatestVisiblePeriod 
    )

VAR _Previous12MonthSales= 
    CALCULATE(
        [Gross Sales], 
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _CurrentMonthStart, -24 ) + 1, 
            EOMONTH( _CurrentMonthStart, -12 ) 
        )
    )

VAR _Current12MonthSales = 
    CALCULATE(
        [Gross Sales], 
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _CurrentMonthStart, -12 ) + 1, 
            EOMONTH( _CurrentMonthStart, 0 ) 
        )
    )

VAR _Result =
IF(
    _Previous12MonthSales  > 0, 
    (_Current12MonthSales  - _Previous12MonthSales ) / _Previous12MonthSales, BLANK() )

RETURN _Result
```
```DAX
Gross Sales YoY% Dynamic Text = 

VAR _MoM = [Gross Sales YoY% Dynamic]

VAR _Result = 
SWITCH(TRUE(),
    _MoM = BLANK(),"",
    _MoM < 0,"▼ "&FORMAT( _MoM,"0.0"&"%" ),
    _MoM >= 0,"▲ "&FORMAT( _MoM,"0.0"&"%" )
)

RETURN _Result
```
```DAX
Variance Period MoM = 
VAR _LatestVisiblePeriod = MAX( DIM_Date[Reporting Period] )
VAR _MonthStart = 
    CALCULATE(
        MIN( DIM_Date[Date] ), 
        DIM_Date[Reporting Period] = _LatestVisiblePeriod 
    )
VAR _PreviousCompletedMonth = 
    CALCULATE(
        MAX( DIM_Date[Reporting Period]),
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _MonthStart, -2 ) + 1, 
            EOMONTH( _MonthStart, -1 ) 
        )
    )
VAR _LatestCompletedMonth = 
    CALCULATE(
        MAX( DIM_Date[Reporting Period] ),
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _MonthStart, -1 ) + 1,
            EOMONTH( _MonthStart, -0 ) 
        )
    )

VAR _Result = "MoM% | "&FORMAT( _LatestCompletedMonth,"mmm YY" ) &" vs "& FORMAT( _PreviousCompletedMonth,"mmm YY" )

RETURN
_Result
```
```DAX
Varience Period YoY = 
//MAX
VAR _LatestVisiblePeriod = MAX( DIM_Date[Reporting Period] )
VAR _MonthStart = 
    CALCULATE(
        MIN( DIM_Date[Date] ), 
        DIM_Date[Reporting Period] = _LatestVisiblePeriod 
    )
VAR _Previous12Months= 
    CALCULATE(
        MAX( DIM_Date[Reporting Period]),
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _MonthStart, -24 ) + 1, 
            EOMONTH( _MonthStart, -12 ) 
        )
    )
VAR _Latest12Months = 
    CALCULATE(
        MAX( DIM_Date[Reporting Period] ),
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _MonthStart, -12 ) + 1,
            EOMONTH( _MonthStart, 0 ) 
        )
    )

//MIN
VAR _LatestVisiblePeriodMIN = MIN( DIM_Date[Reporting Period] )
VAR _MonthStartMIN = 
    CALCULATE(
        MIN( DIM_Date[Date] ), 
        DIM_Date[Reporting Period] = _LatestVisiblePeriod 
    )
VAR _Previous12MonthsMIN= 
    CALCULATE(
        MIN( DIM_Date[Reporting Period]),
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _MonthStart, -24 ) + 1, 
            EOMONTH( _MonthStart, -12 ) 
        )
    )
VAR _Latest12MonthsMIN = 
    CALCULATE(
        MIN( DIM_Date[Reporting Period] ),
        REMOVEFILTERS( DIM_Date ), 
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH( _MonthStart, -12 ) + 1,
            EOMONTH( _MonthStart, -0 ) 
        )
    )

VAR _Result = 
    "( " &
    ( FORMAT( _Latest12MonthsMIN,"mmm YY" ) ) &
     "-" &
     ( FORMAT( _Latest12Months,"mmm YY" ) ) &
    " ) vs ( " & 
    FORMAT( _Previous12MonthsMIN,"mmm YY" ) & 
    "-"
    & ( FORMAT( _Previous12Months,"mmm YY" ) ) &
    " )"

RETURN
_Result
```
```DAX
Date = 
VAR firstOrderDate = MIN('Internet Sales'[OrderDate])
VAR lastOrderDate = MAX('Internet Sales'[OrderDate])
RETURN
ADDCOLUMNS(
    CALENDAR(firstOrderDate, lastOrderDate),
    "DateKey", VALUE(FORMAT([Date], "yyyyMMdd")),
    "Year", YEAR([Date]),
    "Quarter", "Q" & QUARTER([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "Month Short", FORMAT([Date], "MMM"),
    "MonthOrder", MONTH([Date]),
    "Week", "Wk " & WEEKNUM([Date]),
    "Day", FORMAT([Date], "dddd"),
    "Day Short", FORMAT([Date], "ddd"),
    "DayOfMonth", DAY([Date]),
    "DayOrder", WEEKDAY([Date], 2),
    "YearMonth", FORMAT([Date], "yyyy-MM")
)
```
