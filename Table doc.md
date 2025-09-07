# 📊 Power BI Documentation: Table & Card Visualization

## STEP 1: Table Visualization

### General
- **Effects**: Background → Off

### Visual
- **Style preset**: Minimal  
- **Grid**:  
  - Horizontal → On  
  - Width → 1px  
  - Color → `#e7e7e7`  
  - Vertical grid → Off  
- **Border**: All options → Off (no borders)  
- **Row padding**: 10  

### Values
- **Font**: Segoe UI, 10px  
- **Color**: `#333333`

### Column Header
- **Font**: Segoe UI, Bold, 10px  
- **Background color**: `#f5f5f5`  
- **Auto-size width**: Off  

### Totals
- Totals → Off  

### Specific Columns
- **Total column**: Show all → On, Alignment → Left  
- **Items column**: Show all → On, Alignment → Center  
- **Profit % column**: Show all → On, Alignment → Left  

### Cell Elements
- **Profit %**:  
  - **Icon**: On  
  - **Icon type**: Triangle  
    - If value ≤ 0 → Red triangle  
    - If value > 0 → Green triangle  
  - **Data Bar**: On  
    - Positive values → Light Green  
    - Negative values → White  
    - Minimum → Custom → 0  

### Images
- **Image size**: 22px  

---

## STEP 2: Card Visualization

### Card (New)
- **Call Values**:  
  - Series → Period  
  - Label → Font size 9 (apply same setting for others)  
- **Image icon**:  
  - Spacing → 20px  
  - Size → 25px  
  - Position → Left of text  

---

## Filters
- Można tutaj również stworzyć **filtry**.  

---

## Bookmarks
- Dla bookmarków można zaznaczyć **Data**, jeśli chcemy, aby działały **tylko dla jednej strony**.  

---

## Measures

### Profit %
```DAX
Profit % =
DIVIDE(
    [Profit],
    [Total Amount],
    0
)
```

```DAX
LCM =
CALCULATE(
    COUNTROWS(Orders),
    YEAR('Date'[Date]) = YEAR(MAX('Date'[Date])),
    MONTH('Date'[Date]) = MONTH(MAX('Date'[Date]))
)
```
```DAX
PM =
VAR _CurrentMonth = MONTH(MAX('Date'[Date]))
VAR _CurrentYear = YEAR(MAX('Date'[Date]))
VAR _PreviousMonth =
    IF(
        _CurrentMonth = 1,
        12,
        _CurrentMonth - 1
    )
VAR _PreviousYear =
    IF(
        _CurrentMonth = 1,
        _CurrentYear - 1,
        _CurrentYear
    )
RETURN
    CALCULATE(
        COUNTROWS('Orders'),
        MONTH('Date'[Date]) = _PreviousMonth,
        YEAR('Date'[Date]) = _PreviousYear
    )
```
```DAX
LAST vs Prev =
VAR _Result =
    DIVIDE(
        [LCM] - [PM],
        [PM]
    )
VAR Percentage =
    IF(
        ISBLANK(_Result),
        BLANK(),
        IF(
            _Result > 0,
            FORMAT(_Result, "0.00%") & "🔺",
            FORMAT(_Result, "0.00%") & "🔻"
        )
    )
RETURN
Percentage
```