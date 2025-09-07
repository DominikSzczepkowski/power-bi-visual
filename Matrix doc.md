# 📊 Power BI Documentation: Matrix Visualization

## Matrix Setup

### General
- **Properties**:  
  - Height → 540  
  - Position → 150  
- **Padding**: 0 for all  
- **Values**: add `Unit Sold`, plus **Sparklines** by `Month-Year`, `Variable Period`, and `Top Product`  
- **Effects**: Background → Off  

---

### Visual

#### Layout & Style
- **Style preset**: Minimal  
- **Layout**: Tabular  

#### Grid
- Horizontal → Off  
- Border → No border (all options off)  
- Row padding → 4px  

#### Blank Rows
- Enabled → On  
- Color → `#f6f6f6`  

---

### Formatting

#### Values
- Font → Segoe UI, 10px  
- Color → `#333333`  
- Text wrap → Off  

#### Column Headers
- Font → Segoe UI, Bold, 10px  
- Color → `#333333`  
- Background color → `#f6f6f6`  
- Text wrap → On  
- Auto-size width → Off  

#### Row Headers
- Font → Segoe UI, 10px  
- Color → `#333333`  
- Text wrap → On  
- Icons → Off  

#### Subtitles
- Column subtitles → Off  
- Row subtitles → Off  

---

### Specific Columns
- All six measures → Alignment: Left  

---

### Cell Elements
- **Unit Sold**:  
  - Data bar → On  
  - Colors → Positive `#ffcfb8`, Negative `#ffcfb8`  
- **MoM**:  
  - Background color → Rules  
    - Min → 0 → Light Red  
    - 0 to Max → Light Green  
  - Icon → On (same format as above)  

---

### Images
- Image size → 100px  

---

### Sparklines
- Marker → Show highest point  
- Marker color → Black  
- Marker type → Triangle  
- Marker size → 3  

---

## Measures

### Customer Detail
```DAX
Customer Detail =
(
    'gold dim_customers'[country] &
    UNICHAR(10) & UNICHAR(10) &
    "✳️ Country: " & 'gold dim_customers'[country] &
    UNICHAR(10) &
    "✳️ Gender: " & 'gold dim_customers'[gender]
)
```
```DAX
Unit Sold MoM% Dynamic =
VAR _LatestVisiblePeriod = MAX(DIM_Date[Date])
VAR _CurrentMonthStart =
    CALCULATE(
        MIN(DIM_Date[Date]),
        DIM_Date[Date] = _LatestVisiblePeriod
    )
VAR _PreviousMonthSales =
    CALCULATE(
        [Unit Sold],
        REMOVEFILTERS(DIM_Date),
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH(_CurrentMonthStart, -2) + 1,
            EOMONTH(_CurrentMonthStart, -1)
        )
    )
VAR _CurrentMonthSales =
    CALCULATE(
        [Unit Sold],
        REMOVEFILTERS(DIM_Date),
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH(_CurrentMonthStart, -1) + 1,
            EOMONTH(_CurrentMonthStart, 0)
        )
    )
VAR _Result =
    IF(
        _PreviousMonthSales > 0,
        (_CurrentMonthSales - _PreviousMonthSales) / _PreviousMonthSales,
        BLANK()
    )
RETURN _Result

```
```DAX
Variance Period MoM =
VAR _LatestVisiblePeriod = MAX(DIM_Date[Date])
VAR _MonthStart =
    CALCULATE(
        MIN(DIM_Date[Date]),
        DIM_Date[Date] = _LatestVisiblePeriod
    )
VAR _PreviousCompletedMonth =
    CALCULATE(
        MAX(DIM_Date[Date]),
        REMOVEFILTERS(DIM_Date),
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH(_MonthStart, -2) + 1,
            EOMONTH(_MonthStart, -1)
        )
    )
VAR _LatestCompletedMonth =
    CALCULATE(
        MAX(DIM_Date[Date]),
        REMOVEFILTERS(DIM_Date),
        DATESBETWEEN(
            DIM_Date[Date],
            EOMONTH(_MonthStart, -1) + 1,
            EOMONTH(_MonthStart, -0)
        )
    )
VAR _Result =
    "MoM% | " & FORMAT(_LatestCompletedMonth, "mmm YY") &
    " vs " & FORMAT(_PreviousCompletedMonth, "mmm YY")
RETURN _Result

```
```DAX
Unit Sold Top Product =
VAR ProductUnits =
    ADDCOLUMNS(
        SUMMARIZE(
            'gold fact_sales',
            'gold dim_products'[product_name]
        ),
        "TotalUnits", [Unit Sold]
    )
VAR MaxUnits =
    MAXX(ProductUnits, [TotalUnits])
VAR MaxUnitsProduct =
    MAXX(
        FILTER(ProductUnits, [TotalUnits] = MaxUnits),
        'gold dim_products'[product_name]
    )
RETURN MaxUnitsProduct

```