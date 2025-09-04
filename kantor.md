# 📑 Struktura raportu Power BI dla kantoru

## 1. Dashboard główny (KPI na pierwszy rzut oka)
**Cel:** szybki „executive view”

**Karty KPI (góra strony):**
- Łączna sprzedaż (PLN) w bieżącym roku
- Dynamika sprzedaży r/r (%)
- Średnia wartość transakcji (PLN)
- Liczba klientów ogółem
- Udział powracających klientów (%)

**Wykresy:**
- Liniowy: trend miesięcznej sprzedaży (ostatnie 24 miesiące)
- Pierścieniowy: struktura walut (udział w sprzedaży)
- Wskaźnik jakościowy: NPS lub średnia ocena obsługi

---

## 2. Analiza sprzedaży
**Cel:** wolumeny, marża, waluty

- Tabela + słupki: top 10 walut wg sprzedaży i marża
- Wykres liniowy: średnia wartość transakcji w czasie
- Heatmapa: waluty vs średni spread (PLN/%)
- Porównanie m/m lub r/r: słupki zestawione (obecny rok vs poprzedni)

---

## 3. Analiza klientów (behawioralna)
**Cel:** lojalność i częstotliwość transakcji

**Karty KPI:**
- Liczba nowych klientów
- Liczba powracających klientów
- Średnia częstotliwość transakcji / klient

**Wykresy:**
- Pierścieniowy: nowi vs powracający (%)
- Słupkowy: podział wg kanału (online/offline)
- Analiza kohortowa: % klientów wracających w kolejnych miesiącach

---

## 4. Sezonowość i trendy
**Cel:** pokazanie naturalnych rytmów sprzedaży

**Miara:**
```DAX
SumaSprzedazy = SUM(Sprzedaz[WartoscTransakcji])
Kolumny pomocnicze:

DAX
Skopiuj kod
DzienTygodnia = FORMAT(Sprzedaz[DataTransakcji], "dddd")
Godzina = HOUR(Sprzedaz[DataTransakcji])
NumerDnia = WEEKDAY(Sprzedaz[DataTransakcji], 2)  // do sortowania dni
Wykresy:

Heatmapa: sprzedaż wg dni tygodnia / godzin

Wykres liniowy: wakacje vs okres zimowy

Porównanie z poprzednimi latami

5. Kanały sprzedaży
Cel: pokazanie nowoczesności

Słupki zestawione: udział online vs offline (miesiąc po miesiącu)

Trend wzrostu online

6. Podsumowanie strategiczne
Cel: wizualizacja dla zarządu

3–4 kluczowe KPI w dużych kartach: sprzedaż, dynamika r/r, udział powracających klientów, marża

Narracja tekstowa: „Dzięki wzrostowi udziału klientów powracających do 42% i rosnącej średniej wartości transakcji o 12%, kantor odnotował stabilny wzrost sprzedaży mimo trudnych warunków rynkowych.”

Wykres kombinowany: sprzedaż + liczba klientów