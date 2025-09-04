# 📊 Power BI: Line Chart 

A step-by-step guide to creating a fully formatted and dynamic line chart in Power BI using slicers, parameters, and DAX measures.

---

## 🧱 Step 1: Add Custom Background (Optional)

Use **PowerPoint** to design your report background:
- Add layout elements (grid, color blocks, etc.)
- Export as `.png`
- Import into Power BI via `Insert > Image`

---

## 🧮 Step 2: Create a Date Parameter (for X-Axis Toggle)

### 1. Create a New Parameter

- Go to `Modeling > New Parameter > Fields`
- Set the name to: `_Date_Par`
- In **"Add and reorder fields"**, select:
  - `DIM_Date[Month]`
  - `DIM_Date[Quarter]`
- Uncheck: `Add slicer to this page`

> 💡 Optional: Rename fields in the new parameter table

---

## 🎛️ Step 3: Insert and Format a Slicer

### 1. Add Slicer Visual

- Use `_Date_Par` as the field
- Choose "Slicer" from visualizations

### 2. Format Slicer

**General:**
- `Padding:` Right 20, Top 6, Bottom 6, Left 0
- `Title:` Off
- `Header Icon:` Off
- `Background:` Off

**Visual:**
- `Slicer Settings > Force Selection:` On
- `Shape:` Rounded Rectangle (Corners: 5px)
- `Layout:` Max Rows: 1, Columns: 2, Space Between Buttons: 10px
- `Callout Values:`
  - Default: Segoe UI, Size 12, Color White, Transparency 20%
  - Hover: Segoe UI Semibold, Transparency 0%
  - Selected: Segoe UI Semibold, Transparency 0%
- `Selection Icon:`
  - Default: On, Color White, Transparency 20%, Size 15px, Spacing 20px
  - Hover/Selected: Transparency 0%

- `Buttons:`
  - Default:
    - Padding: Right 10, Left 10
    - Border: Off
    - Fill: White, Transparency 100%
  - Hover: Fill: White, Transparency 90%
  - Selected: Fill: White, Transparency 80%

---

## 🏷️ Step 4: Add Year Range Label (Card Visual)

### Measure: `Current Year Title`

```dax
Current Year Title = 
VAR Part1 = "CY" & MAX(DIM_Date[Year])
VAR Part2 = FORMAT(CALCULATE(MIN(DIM_Date[Date]), DIM_Date[Y Rank] = 1), "mmm. yyyy")
VAR Part3 = FORMAT(CALCULATE(MAX(DIM_Date[Date]), DIM_Date[Y Rank] = 1), "mmm. yyyy")
RETURN Part1 & " - ( " & Part2 & " - " & Part3 & " )"
```
## 📈 Step 5: Create and Format Line Chart

### 1. Set Up the Axis

- **X-Axis:** `_Date_Par`
- **Y-Axis:** `Gross Sales CY`, `Gross Sales PY`

---

### 2. Create Measures

```dax
Gross Sales CY = 
CALCULATE(
    [Gross Sales],
    DIM_Date[Y Rank] = 1
)
```
```dax
Gross Sales PY = 
CALCULATE(
    [Gross Sales],
    DIM_Date[Y Rank] = 2
)
```
```dax
Y Rank = 
RANKX(DIM_Date, DIM_Date[Year], , DESC, Dense)
```
### 3. Format Line Chart Visual

#### General 

`Properties`
- **Padding:** 10 on all sides  
- **Responsive:** Off
- **Effects:**  Background: Off
- **Title:** Use dynamic measure:
```dax
Gross Sales CY vs Goal Line Chart Title = 
SWITCH(
    TRUE(),
    SELECTEDVALUE('_Date_Par.'[_Date_Par. Order]) = 0,  "Gross Sales - Current Year vs Goal - by Month",
    SELECTEDVALUE('_Date_Par.'[_Date_Par. Order]) = 1,  "Gross Sales - Current Year vs Goal - by Quarter",
    ""
)
```
#### Visual:
- **X-Axis:**
  - Font: Segoe UI
  - Size: 10
  - Color: #333333
  - Title: Off
- **Y-Axis:**
  - Font: Segoe UI Semibold
  - Size: 10
  - Color: #333333
  - Axis Position: Switched On
  - Title: Off
- **Legend:** Off
- **Gridlines:** Horizontal Off
- **Lines:**
  - All Series:
    - Width: 4px
    - Interpolation: Smooth (Type: Cardinal, Tension: 0%)
  - Gross Sales CY:
    - Color: #5C85AD
  - Gross Sales PY:
    - Style: Dashed
    - Width: 1px
    - Color: #B3B3B3 (30% darker white)
- **Shade Area:**
  - Transparency: 95%
- **Markers – Gross Sales CY:**
  - Show: On
  - Size: 4px
  - Color: #333333
  - Border: Off
- **Data Labels:**
  - Gross Sales PY: Value Off
  - Gross Sales CY: Value Off
    - Detail: On (use this measure):
      - Font: Segoe UI Semibold
      - Size: 10
      - Conditional Formatting (Color):
      - Min = 0: #D55D6F
      - 0–Max = #32B18B
```dax
Gross Sales CY vs Goal Text = 
VAR one = [Gross Sales CY vs Goal %]
VAR two = 
SWITCH(
    TRUE(),
    one = BLANK(), "",
    one < 0, "🡇 " & FORMAT(one, "0" & "%"),
    one >= 0, "🡅 " & FORMAT(one, "0" & "%")
)
RETURN two
```
## 🎯 Step 6: Add Monthly Goal Line Using Error Bars

### 📐 Lower Bound Measure

Create a DAX measure to define the monthly goal for the current year:

```dax
Gross Sales Monthly Goal CY = 
VAR GrossSalesMonthGoalCFY = 
    CALCULATETABLE(
        ADDCOLUMNS(
            SUMMARIZE(
                DIM_Date,
                DIM_Date[Month]
            ),
            "Goal", 40000 
        ),
        DIM_Date[Y Rank] = 1
    )
RETURN
    SUMX(GrossSalesMonthGoalCFY, [Goal])
```
### ⚙️ Error Bars Configuration

Apply the error bars to the **Gross Sales CY** series using the measure above:

- **Series:** Gross Sales CY  
- **Error Bars:** Enabled  
- **Lower Bound:** Use the `Gross Sales Monthly Goal CY` measure  
- **Bars:** On  
- **Color:** #666666
- **Error Band:** On  
  - **Style:** Line  
  - **Match Series Color:** Off  
  - **Band Color:** #32B188
  - **Transparency:** 0%  


