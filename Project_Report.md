# 📄 Project Report — Superstore Sales & Revenue Analysis

**Author:** Anantha Mukesh Paandithevan  
**Programme:** Bachelor of Computer Science (Data Science)  
**Tool:** Microsoft Power BI Desktop  
**Dataset:** Sample Superstore (Excel)  
**Date:** 2024  

---

## 1. Introduction

This project presents an end-to-end Business Intelligence (BI) solution for analyzing retail sales data using Microsoft Power BI. The Sample Superstore dataset — a widely used retail dataset covering orders, customers, products, and regions — was chosen as it provides a realistic scenario for practicing data analytics and dashboard development.

The primary goal of this project is to transform raw transactional data into meaningful visual insights that help business stakeholders make data-driven decisions.

---

## 2. Problem Statement

Retail businesses generate large volumes of transactional data daily. Without proper analysis, this data sits unused while key opportunities for revenue growth and cost reduction are missed. This project addresses three core business questions:

1. Which product categories and regions drive the most revenue and profit?
2. Are our current discounting strategies helping or hurting profitability?
3. How has the business performed over time, and what trends can we expect going forward?

---

## 3. Dataset Description

| Attribute | Details |
|---|---|
| **Source** | Sample Superstore (built-in Power BI / Tableau dataset) |
| **Format** | Microsoft Excel (.xlsx) |
| **Rows** | 9,994 orders |
| **Columns** | 21 fields |
| **Date Range** | January 2020 – December 2023 |
| **Geography** | United States (4 Regions, 49 States) |

### Key Columns Used:
- `Order ID`, `Order Date`, `Ship Date`, `Ship Mode`
- `Customer ID`, `Customer Name`, `Segment`
- `Region`, `State`, `City`
- `Category`, `Sub-Category`, `Product Name`
- `Sales`, `Quantity`, `Discount`, `Profit`

---

## 4. Data Cleaning & Transformation (Power Query)

All data preparation was done using **Power Query (M Language)** inside Power BI Desktop. The following steps were applied:

### Steps Performed:
1. **Loaded** the Excel file into Power Query Editor
2. **Checked data types** — corrected Order Date and Ship Date to Date type, Sales/Profit to Decimal Number
3. **Removed duplicate rows** using the Remove Duplicates function
4. **Handled null values** — no significant nulls were found in key columns
5. **Extracted date components** — created Year, Month Number, Month Name, Quarter columns from Order Date
6. **Renamed columns** for clarity (e.g., `Sub-Category` standardized across tables)
7. **Created a Calendar/Date Table** using the following M formula:

```m
= List.Dates(#date(2020, 1, 1), 
    Duration.Days(#date(2023, 12, 31) - #date(2020, 1, 1)) + 1, 
    #duration(1, 0, 0, 0))
```

8. **Established relationships** in the Data Model:
   - `Orders[Order Date]` → `Date[Date]` (Many-to-One)
   - `Orders[Product ID]` → `Products[Product ID]` (Many-to-One)
   - `Orders[Customer ID]` → `Customers[Customer ID]` (Many-to-One)

---

## 5. Data Model (Star Schema)

The data model follows a **Star Schema** design:

```
                    ┌─────────────┐
                    │  Products   │
                    │  (Dim)      │
                    └──────┬──────┘
                           │
┌─────────────┐     ┌──────┴──────┐     ┌─────────────┐
│  Customers  │────▶│   ORDERS    │◀────│    Date     │
│  (Dim)      │     │  (Fact)     │     │  (Dim)      │
└─────────────┘     └──────┬──────┘     └─────────────┘
                           │
                    ┌──────┴──────┐
                    │   Region    │
                    │  (Dim)      │
                    └─────────────┘
```

**Benefit:** Star schema enables faster query performance and simpler DAX calculations with proper filter context.

---

## 6. Dashboard Design

The report consists of **4 interactive pages**, each addressing a specific analytical theme:

### Page 1: Sales Overview
- **KPI Cards:** Total Sales ($2.30M), Total Profit ($286K), Total Orders (9,994), Profit Margin (12.5%)
- **Bar Chart:** Sales by Category (Technology > Furniture > Office Supplies)
- **Line Chart:** Monthly Sales Trend (2023)
- **Map Visual:** Sales by State (bubble map)
- **Slicers:** Year, Region, Customer Segment

### Page 2: Profit Analysis
- **Horizontal Bar Chart:** Profit by Sub-Category (sorted, with negative bars highlighted in red)
- **Scatter Chart:** Discount % vs Profit (shows inverse relationship clearly)
- **Table:** Top 10 Products by Profit with conditional formatting

### Page 3: Regional Performance
- **Filled Map:** Sales intensity by State
- **Table:** Region-wise breakdown of Sales, Profit, Orders
- **Drill-through:** Click any state to see order-level details

### Page 4: Time Intelligence
- **Card:** YoY Growth % (dynamic based on year slicer)
- **Column Chart:** Sales by Quarter (with prior year comparison)
- **Matrix:** Month × Year sales heat map

---

## 7. Key Insights & Findings

### Insight 1 — Technology Dominates Sales
Technology contributes 36% of total sales ($830K) and has the highest average order value. Phones ($330K) and Machines ($189K) are the top sub-categories within Technology.

### Insight 2 — West Region is the Strongest Market
The West region generates $725K in sales, making it the top-performing region. California alone accounts for $457K (19.9% of total sales) — the highest of any individual state.

### Insight 3 — Furniture Has Profitability Issues
Despite generating $742K in sales, the Furniture category yields only $18K in profit (a 2.4% margin). Tables ($-17K) and Bookcases ($-3K) actually operate at a net loss, primarily driven by discount rates averaging 28–32%.

### Insight 4 — Corporate Segment is Most Profitable Per Order
The Consumer segment brings the most orders (52%), but the Corporate segment generates 33% higher profit per order on average. Home Office customers have the highest margin percentage at 14.3%.

### Insight 5 — Q4 is the Peak Revenue Season
November and December together represent ~30% of annual revenue. This is a critical window for sales campaigns and inventory planning.

### Insight 6 — Discounting Hurts the Bottom Line
A clear negative correlation exists between discount rate and profit. Orders with discounts above 20% average a profit margin of –3.8% compared to +18.2% for non-discounted orders. This suggests the current discount strategy needs to be reassessed.

---

## 8. Recommendations

Based on the analysis, the following business actions are recommended:

1. **Focus marketing budget on Technology and Office Supplies** — they offer better profit margins than Furniture.
2. **Cap discount rates at 15%** across all categories to protect profitability.
3. **Target Corporate customers** with premium product bundles to maximize profit per transaction.
4. **Invest in Q4 inventory and promotions** to capitalize on peak-season demand.
5. **Review pricing for Tables and Bookcases** — consider discontinuing deep discounts or adjusting cost structure.
6. **Expand West Region presence** — it already leads in performance; more investment here will yield strong returns.

---

## 9. Conclusion

This project demonstrated a complete end-to-end data analytics workflow using Power BI — from raw data import and cleaning in Power Query, through data modeling and DAX calculations, to interactive dashboard creation and business insight generation.

The Superstore Sales Analysis dashboard provides a business-ready tool that allows stakeholders to monitor KPIs in real time, filter by various dimensions, and drill into region- or product-level details as needed.

---

## 10. References

- Microsoft Power BI Documentation: https://docs.microsoft.com/power-bi
- Sample Superstore Dataset: Available via Power BI Desktop sample data
- DAX Reference: https://docs.microsoft.com/dax
- Power Query M Language Reference: https://docs.microsoft.com/powerquery-m
