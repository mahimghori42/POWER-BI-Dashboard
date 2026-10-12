# AtliQ Hardware: Finance, Sales & Supply Chain Dashboard (Power BI)

An end-to-end Power BI report that gives a hardware company a single, consistent view of its P&L, customer and product profitability, and demand-forecast accuracy, with year-over-year comparisons throughout.

> **Dataset:** AtliQ Hardware, from the Codebasics Resume Project Challenge.
> The data model, DAX measures and report design are my own work.

---

## Preview

![Finance View](screenshots/finance-view.png)
![Sales View](screenshots/sales-view.png)
![Supply Chain View](screenshots/supply-chain-view.png)

**Live report / PDF:** [add link here]

---

## Business problem

AtliQ Hardware sells computer hardware and peripherals across 4 regions and 27 markets. Leadership needed answers to three questions from one source of truth:

- **Finance:** How does gross sales turn into net sales, gross margin and net profit, and how does that compare with last year?
- **Sales:** Which customers and markets bring the most revenue and margin?
- **Supply chain:** How accurate is the demand forecast, and where is there a risk of excess inventory or stock-outs?

## Report pages

| Page | What it shows |
|---|---|
| **Finance View** | KPI cards for Net Sales, Gross Margin % and Net Profit % against last year. A dynamic P&L matrix with selected year, last year and YoY change. A trend area chart. Top/bottom N tables by market, customer and product. |
| **Sales View** | Customer performance table (Net Sales, GM $, GM %). Scatter of net sales vs GM % by customer and region. P&L composition donuts. Product drill-down (segment > category > product). |
| **Supply Chain View** | KPI cards for Forecast Accuracy %, Net Error and Absolute Error vs last year. Customer and product risk tables flagging **Excess Inventory** vs **Out of Stock**. Combo chart of forecast error and accuracy over time. |

All pages share the same slicers: fiscal year, region / market, customer, product hierarchy, quarter and YTD / YTG.

## Data model

- **Star schema** with 3 fact tables (actuals + forecast, monthly forecast, post-invoice deductions) and dimensions for customer, product, market and date.
- **Scale:** about 1.86M actuals rows and 1.89M forecast rows, with 209 customers, 397 products and 27 markets.
- **Custom fiscal calendar:** the fiscal year starts in September, with fiscal month, quarter and a YTD / YTG flag.
- **Actuals + forecast:** the actuals table combines historical sales with the remaining forecast months, so the latest fiscal year is labelled "Est".
- **Sources:** MySQL for sales, forecast, customer, product, market and cost tables; Excel for operating-expense percentages.

## Technical highlights

- **40+ DAX measures** organised in a dedicated `Key Measures` table.
- **Full P&L build-up in DAX:** Gross Sales → Pre-invoice deductions → Net Invoice Sales → Post-invoice deductions → Net Sales → COGS → Gross Margin → Operating Expense → Net Profit. The source provided percentages, so I calculated the dollar amounts in the fact table.
- **Dynamic P&L matrix:** disconnected `P & L Rows` and `P & L Columns` tables with a `SWITCH`-driven measure, so one visual serves every line item and column (selected year, LY, YoY change).
- **Time intelligence:** `SAMEPERIODLASTYEAR` for LY and YoY across sales, margin, profit and forecast KPIs.
- **Forecast accuracy:** absolute error computed at month × product grain with `SUMX`, then `Accuracy = 1 − Abs Error %`. Net error alone would let over- and under-forecasts cancel out.
- **Inventory risk flag:** Net Error > 0 gives "Excess Inventory", Net Error < 0 gives "Out of Stock".
- **Cut-off logic:** a `LastSalesMonth` table so forecast comparisons only cover months with actual sales.
- **Dynamic titles:** visual titles change with the selected P&L line.

## Tools

Power BI Desktop · DAX · Power Query (M) · MySQL · Excel

## How to view this project

The full `.pbix` is over 270 MB and connects to a local MySQL server and a local Excel file, so it can't be hosted on GitHub or refreshed on another machine. Instead this repo contains:

| File | Purpose |
|---|---|
| `screenshots/` | Images of each report page |
| `AtliQ_Dashboard.pdf` | PDF export of the report *(add if you have it)* |
| `POWER_BI_DASHBOARD_AtliQ_bit.pbit` | Power BI template with the full model, measures and report layout, but **no data** |

**Opening the `.pbit`:** open it in Power BI Desktop to inspect the data model, relationships, DAX and Power Query steps. It will prompt for data-source details. Without access to the original MySQL databases and Excel file, the refresh will fail, but the model and report design can still be explored.

## What I'd improve next

- Move the Excel operating-expense inputs into the database.
- Add incremental refresh for the large fact tables.
- Add row-level security by region.
- Add drill-through pages for individual customers.

## Author

MAHIM A G
https://www.linkedin.com/in/mahim-ghori-00b241262/
mahimghori42@gmail.com
