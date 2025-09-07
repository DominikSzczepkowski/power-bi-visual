# 📊 Dokumentacja tworzenia wizualizacji w Power BI

## Spis treści
1. [Stacked bar chart (Profit)](#1-stacked-bar-chart-profit)
2. [Duplikacja wykresu (Net Profit)](#2-duplikacja-wykresu-net-profit)
3. [Dodanie flagi (ikonka SVG)](#3-dodanie-flagi-ikonka-svg)
4. [Dodanie karty (Profit)](#4-dodanie-karty-profit)
5. [Dodanie shape (wskaźnik koloru)](#5-dodanie-shape-wskaźnik-koloru)
6. [Dodanie kart wskaźników (% goal)](#6-dodanie-kart-wskaźników-goal)
7. [Slicer](#7-slicer)
8. [Miary DAX](#8-miary-dax)
9. [Efekt końcowy](#9-efekt-końcowy)

---

## 1. Stacked bar chart (Profit)

### Ustawienia wykresu
- **Measure**: Profit
- **Constant line**:
  - Nazwa: Profit Goal
  - Wartość: `[Profit Goal]`
  - Kolor: lighter black 20%
  - Przezroczystość: 0%
  - Styl linii: dotted
  - Skala z szerokością: On
  - Szerokość: 3 px
  - Pozycja: in front
  - **Data label**:
    - Poziom: left
    - Pion: under
    - Kolor: lighter black 20%
    - Jednostki: None
    - Wartość: 0 decimal

### Formatowanie ogólne
- Padding: left 20, right 20, bottom 15, top 5
- Effects: background off, header icon off, title off

### Oś X
- Values: off
- Title: off

### Oś Y
- Min: 0
- Max: Auto
- Invert range: off
- Values: off
- Title: off

### Kolory barów (format fx)
- 0 – 0.4 → #FBD0D3
- 0.4 – 1 → #F6E9BC
- 1 – max → #CAF3E7

### Dodatkowe
- W tabeli `dim_date` utwórz kolumnę obliczaną:

```DAX
record = "1"
```
## 2. Duplikacja wykresu (Net Profit)

- Skopiuj wykres i podmień miarę na `Net profit (bar chart)`
- **Formatowanie**:
  - Legend: off
  - Kolor:
    - Profit → biały
    - Net Profit (bar chart) → szary
- **Wyrównanie**:
  - Align top
  - Align left
- Upewnij się, że pierwszy wykres (Profit) jest na wierzchu w Selection

---

## 3. Dodanie flagi (ikonka SVG)

- Wstaw **Blank**
- **Styl**:
  - Border: off
- Wstaw ikonę **flag SVG**
- Przenieś ikonę obok wykresu

---

## 4. Dodanie karty (Profit)

- Wstaw kartę z miarą `Profit`
- **Formatowanie**:
  - Background: off
  - Header icon: off
  - Padding: 20,5,5,5
- **Callout value**:
  - Font: Segoe UI Bold, 14 px
- **Label**:
  - Font: Segoe UI, 14 px
- **Spacing**: 3 px

---

## 5. Dodanie shape (wskaźnik koloru)

- Wstaw kształt: 35 x 100 px
- **Padding**: 0,0,0,0
- **Wypełnienie (`fx`)**:
  - 0 – 0.4 → #FBD0D3
  - 0.4 – 1 → #F6E9BC
  - 1 – max → #CAF3E7
- **Border (`fx`)**:
  - 0 – 0.4 → #BC101C
  - 0.4 – 1 → #B49218
  - 1 – max → #26A680

---

## 6. Dodanie kart wskaźników (% goal)

1. Wstaw kartę z miarą `Net profit % goal`  
   - Kolor tekstu = kolor granicy z shape (dynamiczny)
2. Skopiuj kartę i podmień miarę na `Net profit % goal text symbol`  
   - Wyświetla: ✔️, ⚠️, 🚨, -

---

## 7. Slicer

- Dodaj **slicer** wg uznania  
- W zwykłym slicerze nie można zmieniać koloru tła ani innych efektów formatowania
## 8. Miary DAX
```DAX
Net profit % goal =
DIVIDE(
    [Profit],
    [Net Profit Goal], 
    0
)

Net profit (bar chart) =
SWITCH(
    TRUE(),
    [Profit] < 0, [Net Profit Goal],
    [Net Profit Goal] - [Profit]
)

Net profit % goal text symbol =
SWITCH(
    TRUE(),
    [Net profit % goal] = BLANK(), "-",
    [Net profit % goal] < 1 && [Net profit % goal] > 0.4, "⚠️",
    [Net profit % goal] < 0.4, "🚨",
    "✔️"
)
```