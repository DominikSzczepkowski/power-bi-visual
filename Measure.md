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
```DAX
Net Profit % to Goal Text Symbol = 
SWITCH(
    TRUE(),
    [Net Profit % to Goal] = BLANK(), "-",
    [Net Profit % to Goal] < 0.99 && [Net Profit % to Goal] > 0.4, "⚠️ ",
    [Net Profit % to Goal] < 0.4, "🚨 ",
    "✔ "
    )
```
```DAX
Gross Sales % to Avg. Month = 
DIVIDE( [Gross Sales],[Gross Sales Avg. Month Total],0 )
```
```DAX
Gross Sales % to Avg. Text Symbol = 
SWITCH(
    TRUE(),
    [Gross Sales % to Avg. Month] = BLANK(), "-",
    [Gross Sales % to Avg. Month] < 0.99 && [Gross Sales % to Avg. Month] > 0.4, "⚠️ ",
    [Gross Sales % to Avg. Month] < 0.4, "🚨 ",
    "✔ "
    )
```
```DAX
Gross Sales All = 
CALCULATE(
    [Gross Sales],
    ALL(
        DIM_Region
    )
)
```
```DAX
Gross Sales Avg. Month = 
CALCULATE(
    AVERAGEX(
        VALUES( DIM_Calendar[Month Year] ),
        [Gross Sales]
    ),
    FILTER( ALLSELECTED( DIM_Calendar ), [Gross Sales] <> BLANK() )
)
```
```DAX
Gross Sales Avg. Month Total = 
VAR _GrossSalesAvg = 
CALCULATE(
    AVERAGEX(
        VALUES( DIM_Calendar[Month Year] ),
        [Gross Sales]
    ),
    FILTER( ALL( DIM_Calendar ), [Gross Sales] <> BLANK() )
)
VAR __GrossSalesAvgTable = 
FILTER (
    ADDCOLUMNS(
        SUMMARIZE(
            DIM_Calendar,
            DIM_Calendar[Month Year]
        ),
    "GrossSalesAverage",_GrossSalesAvg,
    "GrossSales",[Gross Sales]
    ),
    [GrossSales] <> BLANK()
)
RETURN
CALCULATE( SUMX(  __GrossSalesAvgTable,[GrossSalesAverage] ))
```
```DAX
Gross Sales MoM % Color = 
VAR _Value_ = [Gross Sales MoM% Dynamic]
VAR _Result = 
    SWITCH( 
        TRUE(),
        ISBLANK(_Value_),BLANK(),
        _Value_ < 0, "#FAC8C6", // light red
        _Value_ >= 0, "#DFF6F3", //pale teal
        "#FFFFF" //white
    )
RETURN _Result
```
```DAX
Gross Sales MoM% Dynamic = 

VAR _LatestVisiblePeriod = CALCULATE( MAX( DIM_Calendar[Month Year] ),FILTER( ALLSELECTED( DIM_Calendar),[Products Sold] <> BLANK() ) )
VAR _CurrentMonthStart = 
    CALCULATE(
        MIN( DIM_Calendar[Date] ), 
        DIM_Calendar[Month Year] = _LatestVisiblePeriod 
    )

VAR _CurrentMonthSales = 
    CALCULATE(
        [Gross Sales], 
        REMOVEFILTERS( DIM_Calendar), 
        DATESBETWEEN(
            DIM_Calendar[Date],
            EOMONTH( _CurrentMonthStart, -2 ) + 1, 
            EOMONTH( _CurrentMonthStart, -1 ) 
        )
    )

VAR _PreviousMonthSales= 
CALCULATE(
    [Gross Sales], 
    REMOVEFILTERS( DIM_Calendar), 
    DATESBETWEEN(
        DIM_Calendar[Date],
        EOMONTH( _CurrentMonthStart, -3 ) + 1, 
        EOMONTH( _CurrentMonthStart, -2 ) 
    )
)
VAR _Result =
IF(
    _PreviousMonthSales > 0, 
    (_CurrentMonthSales   - _PreviousMonthSales ) / _PreviousMonthSales , BLANK() 
)
RETURN _Result
```
```DAX
Gross Sales MoM% Dynamic = 

VAR _LatestVisiblePeriod = CALCULATE( MAX( DIM_Calendar[Month Year] ),FILTER( ALLSELECTED( DIM_Calendar),[Products Sold] <> BLANK() ) )
VAR _CurrentMonthStart = 
    CALCULATE(
        MIN( DIM_Calendar[Date] ), 
        DIM_Calendar[Month Year] = _LatestVisiblePeriod 
    )

VAR _CurrentMonthSales = 
    CALCULATE(
        [Gross Sales], 
        REMOVEFILTERS( DIM_Calendar), 
        DATESBETWEEN(
            DIM_Calendar[Date],
            EOMONTH( _CurrentMonthStart, -2 ) + 1, 
            EOMONTH( _CurrentMonthStart, -1 ) 
        )
    )

VAR _PreviousMonthSales= 
CALCULATE(
    [Gross Sales], 
    REMOVEFILTERS( DIM_Calendar), 
    DATESBETWEEN(
        DIM_Calendar[Date],
        EOMONTH( _CurrentMonthStart, -3 ) + 1, 
        EOMONTH( _CurrentMonthStart, -2 ) 
    )
)
VAR _Result =
IF(
    _PreviousMonthSales > 0, 
    (_CurrentMonthSales   - _PreviousMonthSales ) / _PreviousMonthSales , BLANK() 
)
RETURN _Result
``````DAX
Gross Sales MoM% Dynamic Label = 
VAR _MoM = [Gross Sales MoM% Dynamic]
VAR _Result = 
SWITCH(TRUE(),
    _MoM = BLANK(),"",
    _MoM < 0,"🠋 "&FORMAT( _MoM,"0.0"&"%" ),
    _MoM >= 0,"🠉 "&FORMAT( _MoM,"0.0"&"%" )
)
RETURN _Result
```
