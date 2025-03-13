# Power BI KPI Measures

Bu dosya, Power BI'da kullanılan çeşitli KPI ölçümlerini içermektedir. Aşağıda **Profit YOY**, **Quantity YOY**, **Revenue YOY**, **Revenue Target**, **Target Orders**, **Total Orders**, **Total Profit**, **Total Quantity**, ve **Total Revenue** hesaplamaları bulunmaktadır.

---

## KPI: Profit Year Over Year (YOY)

```DAX
/**
 * This function calculates the year-over-year (YOY) profit change.
 * It compares the current year's profit with the previous year's profit.
 * The result is returned as a percentage difference with an indicator.
 */
VAR LastYearRevenue = CALCULATE([total profit], SAMEPERIODLASTYEAR(date_table[Date]))
VAR CurrentYearRevenue = CALCULATE([total profit], DATESYTD(date_table[Date]))
VAR diff = CurrentYearRevenue - LastYearRevenue
VAR diff_ratio = DIVIDE(diff, LastYearRevenue, 0)
VAR abs_diff = ABS(diff_ratio)
VAR ResultText = 
    IF(
        ISBLANK(LastYearRevenue), 
        "no sale last year",
        IF(
            diff_ratio > 0, 
            FORMAT(abs_diff, "0.0%") & " more than last year ▲",
            FORMAT(abs_diff, "0.0% ") & "less than last year ▼"
        )
    )
RETURN ResultText
```

---

## KPI: Quantity Year Over Year (YOY)

```DAX
/**
 * This function calculates the year-over-year (YOY) quantity change.
 * It compares the current year's quantity with the previous year's quantity.
 * The result is formatted in thousands (K) with an indicator.
 */
VAR LastYearQty = CALCULATE([total qty], SAMEPERIODLASTYEAR(date_table[Date]))
VAR CurrentYearQty = CALCULATE([total qty], DATESYTD(date_table[Date]))
VAR diff = CurrentYearQty - LastYearQty
VAR abs_diff_k = ABS(diff) / 1000
VAR ResultText = 
    IF(
        ISBLANK(LastYearQty), 
        "No sale last year", 
        IF(
            diff > 0, 
            FORMAT(abs_diff_k, "0.0") & "K more than last year ▲", 
            FORMAT(abs_diff_k, "0.0") & "K less than last year ▼"
        )
    )
RETURN ResultText
```

---

## KPI: Revenue Year Over Year (YOY)

```DAX
/**
 * This function calculates the year-over-year (YOY) revenue change.
 * It compares the current year's revenue with the previous year's revenue.
 * The result is returned as a percentage difference with an indicator.
 */
VAR LastYearRevenue = CALCULATE([total revenue], SAMEPERIODLASTYEAR(date_table[Date]))
VAR CurrentYearRevenue = CALCULATE([total revenue], DATESYTD(date_table[Date]))
VAR diff = CurrentYearRevenue - LastYearRevenue
VAR diff_ratio = DIVIDE(diff, LastYearRevenue, 0)
VAR abs_diff = ABS(diff_ratio)
VAR ResultText = 
    IF(
        ISBLANK(LastYearRevenue), 
        "no sale last year",
        IF(
            diff_ratio > 0, 
            FORMAT(abs_diff, "0.0%") & " more than last year ▲",
            FORMAT(abs_diff, "0.0%") & " less than last year ▼"
        )
    )
RETURN ResultText
```

---

## Revenue Target Calculation

```DAX
/**
 * This function calculates the revenue target.
 * If there was no sale last year, it defaults to 70,000.
 * Otherwise, it sets the target as 150% of last year's revenue.
 */
VAR LastYearSale = CALCULATE([total revenue], SAMEPERIODLASTYEAR(date_table[Date]))
RETURN IF(ISBLANK(LastYearSale), 70000, LastYearSale * 1.5)
```

---

## Target Orders Calculation

```DAX
/**
 * This function calculates the target number of orders.
 * If there was no sale last year, it defaults to 15,000.
 * Otherwise, it sets the target as 150% of last year's orders.
 */
VAR LastYearOrder = CALCULATE([total orders], SAMEPERIODLASTYEAR(date_table[Date]))
RETURN IF(ISBLANK(LastYearOrder), 15000, LastYearOrder * 1.5)
```

---

## Total Orders Calculation

```DAX
/**
 * This function calculates the total distinct orders.
 */
TOTAL_ORDERS = DISTINCTCOUNT('global sales'[order_id])
```

---

## Total Profit Calculation

```DAX
/**
 * This function calculates the total profit.
 */
TOTAL_PROFIT = SUM('global sales'[profit])
```

---

## Total Quantity Calculation

```DAX
/**
 * This function calculates the total quantity.
 */
TOTAL_QTY = SUM('global sales'[qty])
```

---

## Total Revenue Calculation

```DAX
/**
 * This function calculates the total revenue.
 */
TOTAL_REVENUE = SUM('global sales'[net_sale])
```

---
