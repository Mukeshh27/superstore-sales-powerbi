# 📐 DAX Measures — Superstore Sales Analysis

All custom DAX measures used in this Power BI project are documented below, organized by category.

---

## 📦 Sales Measures

```dax
-- Total revenue from all orders
Total Sales = 
    SUM(Orders[Sales])


-- Total number of orders placed
Total Orders = 
    DISTINCTCOUNT(Orders[Order ID])


-- Average revenue per order
Avg Order Value = 
    AVERAGE(Orders[Sales])


-- Total quantity of products sold
Total Quantity = 
    SUM(Orders[Quantity])
```

---

## 💰 Profit Measures

```dax
-- Total profit across all orders
Total Profit = 
    SUM(Orders[Profit])


-- Profit as a percentage of total sales
Profit Margin % = 
    DIVIDE([Total Profit], [Total Sales], 0)


-- Average profit per order
Avg Profit Per Order = 
    DIVIDE([Total Profit], [Total Orders], 0)
```

---

## 📅 Time Intelligence Measures

```dax
-- Sales from the same period in the previous year
LY Sales = 
    CALCULATE(
        [Total Sales],
        SAMEPERIODLASTYEAR('Date'[Date])
    )


-- Year-over-Year growth percentage
YoY Growth % = 
    DIVIDE(
        [Total Sales] - [LY Sales],
        [LY Sales],
        0
    )


-- Cumulative (running total) sales from start of year to current date
YTD Sales = 
    TOTALYTD([Total Sales], 'Date'[Date])


-- Quarter-to-date sales
QTD Sales = 
    TOTALQTD([Total Sales], 'Date'[Date])
```

---

## 🎯 Segment & Category Measures

```dax
-- Sales filtered to Consumer segment only
Consumer Sales = 
    CALCULATE([Total Sales], Orders[Segment] = "Consumer")


-- Sales filtered to Corporate segment only
Corporate Sales = 
    CALCULATE([Total Sales], Orders[Segment] = "Corporate")


-- Sales for Technology category
Technology Sales = 
    CALCULATE([Total Sales], Orders[Category] = "Technology")


-- Profit for Furniture category (to highlight negative margin)
Furniture Profit = 
    CALCULATE([Total Profit], Orders[Category] = "Furniture")
```

---

## 📉 Discount Analysis Measures

```dax
-- Average discount rate applied
Avg Discount % = 
    AVERAGE(Orders[Discount])


-- Count of orders where discount > 20%
High Discount Orders = 
    CALCULATE(
        [Total Orders],
        Orders[Discount] > 0.20
    )


-- Profit margin on high-discount orders
High Discount Margin = 
    CALCULATE(
        [Profit Margin %],
        Orders[Discount] > 0.20
    )
```

---

## 🗺️ Regional Measures

```dax
-- Sales for West Region
West Region Sales = 
    CALCULATE([Total Sales], Orders[Region] = "West")


-- Profit for East Region
East Region Profit = 
    CALCULATE([Total Profit], Orders[Region] = "East")


-- % of total sales from selected region (used in visuals)
Region Sales % = 
    DIVIDE(
        [Total Sales],
        CALCULATE([Total Sales], ALL(Orders[Region])),
        0
    )
```

---

## 🏆 Ranking Measures

```dax
-- Rank sub-categories by sales (used for sorted bar chart)
Sub-Category Sales Rank = 
    RANKX(
        ALL(Orders[Sub-Category]),
        [Total Sales],
        ,
        DESC,
        Dense
    )


-- Top N flag (TRUE if sub-category is in Top 5 by profit)
Is Top 5 Profit = 
    IF(
        RANKX(
            ALL(Orders[Sub-Category]),
            [Total Profit],
            ,
            DESC,
            Dense
        ) <= 5,
        "Top 5",
        "Others"
    )
```

---

## 📊 KPI Threshold Measures (for conditional formatting)

```dax
-- Traffic light color for Profit Margin KPI
Margin Color = 
    IF([Profit Margin %] >= 0.15, "Green",
        IF([Profit Margin %] >= 0.05, "Yellow", "Red"))


-- Indicates whether current YoY growth is positive
Growth Flag = 
    IF([YoY Growth %] > 0, "▲ Growth", "▼ Decline")
```

---

> 💡 **Tip:** All measures are stored in a dedicated `_Measures` table in the Power BI data model for easy organization.
