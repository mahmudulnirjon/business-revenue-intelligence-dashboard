# Business Revenue Intelligence Dashboard

An end-to-end revenue analysis of the Superstore retail dataset, built with **Excel** (data audit, cleaning, Pivot Tables) and **Power BI** (data model, DAX, interactive dashboard). The project identifies when demand peaks, which product categories actually make money, and how discounting affects profit.

![Power BI Dashboard](../Screenshots/01_powerbi_dashboard.png)

---

## Key Findings

| # | Finding | Evidence |
|---|---------|----------|
| 1 | **Demand is strongly seasonal.** September, November and December together generate about 43% of total revenue. | Same three months rank in the top 3 in every year (2014-2017). January and February are the weakest. |
| 2 | **Furniture sells but barely earns.** Furniture revenue is close to Office Supplies, but its profit margin is only 2.49% vs 17.04%. | Furniture: $741.7K revenue, $18.5K profit. Office Supplies: $719.0K revenue, $122.5K profit. |
| 3 | **Heavy discounting destroys profit.** Profit turns negative at discount levels of 30% and above. | Orders with no discount earn about a 29.5% margin; orders discounted above 20% lose money overall. |
| 4 | **Growth is real but uneven.** Revenue dipped 2.78% in 2015, then grew 29.47% in 2016 and 20.36% in 2017. | Year-over-year Pivot, cross-checked with DAX. |

### Headline numbers (cleaned data)

| Metric | Value |
|--------|-------|
| Total Revenue | $2,296,919.49 |
| Total Profit | $286,409.08 |
| Overall Profit Margin | 12.47% |
| Units Sold | 37,871 |
| Order Lines | 9,993 |

---

## Tools and Skills Demonstrated

- **Excel:** Tables, Data Validation checks, `ISNUMBER`, `COUNTIF(S)`, `SUMPRODUCT`, `IF` / `IFS`, Pivot Tables, Calculated Fields, Show Values As (% of Column Total, % Difference From)
- **Power BI:** Power Query data typing, Calendar table, Star-style relationship, DAX measures, Time Intelligence, slicers, interactive visuals
- **Analysis:** Seasonality, Year-over-Year growth, margin analysis, discount impact

---

## Dataset

- **Source:** Kaggle, [Superstore Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)
- **Size:** 9,994 rows, 21 columns, orders from 2014 to 2017
- **Note:** `Sales` is already net of discount, so all margins in this project are measured on post-discount revenue.

---

## Methodology

### 1. Data audit (Excel)
Before any analysis, the raw data was validated in a dedicated `Audit` sheet:
- Row count matches expectation (9,994)
- All dates are true date values; no blank cells in columns A to U
- Region totals reconcile to the row count
- A duplicate-row check found **one pair of identical rows**

### 2. Cleaning decision
- `Raw_Data` is kept **untouched** as the source of truth.
- One row of the duplicate pair was removed **only in `Cleaned_Data`**, leaving 9,993 rows.
- The effect is small (about $281 of revenue) and is documented in the cleaning log.

### 3. Calculated columns (marked "(Calculated)" in headers)
Year, Month No, Month Name, Weekday, Shipping Days, Gross Sales, Unit List Price, Price Band, Discount Band.
Gross Sales (`Sales / (1 - Discount)`) and Unit List Price are **estimates**, not source data, and are labelled as such.

### 4. Excel analysis (Pivot Tables)
- Monthly revenue, demand and profit
- Seasonality (each month as % of its year's revenue)
- Year-over-year growth
- Revenue, profit and margin by Category
- Profit by discount level

Every Pivot was reconciled to the source totals with `SUM` and row-count checks.

### 5. Power BI dashboard
- Calendar table marked as a date table, related to `Order Date`
- DAX measures: Total Revenue, Total Profit, Profit Margin %, Total Units, Total Orders (`DISTINCTCOUNT`), Revenue PY (`SAMEPERIODLASTYEAR`), Revenue YoY %
- Dashboard values were reconciled against the Excel results (Revenue, Profit, Margin and Units all match)

```
Profit Margin % = DIVIDE ( [Total Profit], [Total Revenue] )
Revenue PY      = CALCULATE ( [Total Revenue], SAMEPERIODLASTYEAR ( 'Calendar'[Date] ) )
Revenue YoY %   = DIVIDE ( [Total Revenue] - [Revenue PY], [Revenue PY] )
```

---

## Why the Margin Uses a Weighted Average

The overall 12.47% is total profit divided by total revenue, not the simple average of category margins. Categories differ in size, so a simple average would misstate the true margin. Pivot Calculated Fields and the DAX `DIVIDE` measure both compute it correctly.

---

## Limitations

- The dataset has **no inventory data**, so stock-level and stockout analysis was not possible.
- There is **no competitor data**, so competitor benchmarking was not performed.
- Gross Sales and Unit List Price are derived estimates.
- Order-level growth and forecasting models are out of scope for this version.

## Possible Next Steps

- Product velocity classification (fast / medium / slow movers) and Pareto analysis
- Region and Sub-Category profitability deep dive
- Simple demand forecast using the seasonal pattern

---

## Repository Structure

```
business-revenue-intelligence-dashboard/
├── dataset/          raw_dataset.csv
├── excel/            revenue_analysis.xlsx
├── powerbi/          revenue_dashboard.pbix
├── screenshots/      dashboard and Excel screenshots
└── documentation/    README.md
```

## How to Use

1. Open `excel/revenue_analysis.xlsx` to see the audit, cleaned data and Pivot Tables.
2. Open `powerbi/revenue_dashboard.pbix` in Power BI Desktop and use the Year and Region slicers.
