# Report Spec — VanArsdel Sales Performance

## Purpose & audience

* Audience: Commercial leadership, Sales Ops, Marketing Ops, Product/Category Managers, Regional Managers, Finance/FP&A.
* Decisions supported: revenue growth tracking, category/product prioritization, channel investment, regional focus, budget/forecast performance management.
* Refresh / latency: monthly refresh is sufficient after source data refresh cadence is agreed. During development, refresh manually from `dataset/raw/`.

## Key questions

1. How much revenue, gross profit, margin, units, and customer activity did VanArsdel generate in the selected period?
2. Is revenue growing or declining over time? Which months/quarters drive the change?
3. Which Categories and Segments drive revenue, gross profit, and margin?
4. Which Products are top and bottom performers?
5. Which Traffic Channels and Devices drive the most revenue and active customers?
6. What is the digital vs offline/direct-mail revenue mix?
7. Which Regions, States, and Districts are strongest or weakest?
8. Where is actual revenue above or below Budget?
9. What 2021 Budget/Forecast exists, and how should it be viewed given actuals stop at 2020-06-30?

## KPIs

| KPI | Measure | Target / benchmark |
|---|---|---|
| Revenue | `[Revenue]` | Compare to prior period, prior year, Budget where available. |
| Gross Profit | `[Gross Profit]` | Higher is better; use by Product/Category. |
| Gross Margin % | `[Gross Margin %]` | Benchmark against selected total or prior year. |
| Units Sold | `[Units Sold]` | Compare to prior period/prior year. |
| Active Customers | `[Customers]` | Compare to prior period/prior year. |
| Revenue YoY % | `[Revenue YoY %]` | Positive growth is favorable. |
| Budget | `[Budget]` | Finance target. |
| Budget Attainment % | `[Budget Attainment %]` | 100% is on target; >100% favorable. |
| Budget Variance | `[Budget Variance]` | Positive is above budget. |
| Budget Variance % | `[Budget Variance %]` | Positive is favorable. |
| Digital Revenue Share % | `[Digital Revenue Share %]` | Benchmark to channel strategy after Marketing confirms. |
| Average Selling Price | `[Average Selling Price]` | Monitor mix/price movement. |

## Grain & dimensions

* Fact grain:
  * `Sales`: Product × Transaction Date × Customer × Campaign sales line.
  * `Plan`: Plan Type × Month × Category × Segment.
* Required dimensions / slicers:
  * Date: Year, Quarter, Month, Date.
  * Product: Category, Segment, Product.
  * Campaign: Traffic Channel, Device, Channel Type (if created).
  * Geography: Region, State, District, City.
  * Plan: Plan Type for specialized views; do not require users to filter Plan Type for base measures.
* Comparison grain:
  * Actual vs Budget/Forecast should happen at Month × Category × Segment or higher.
  * Product-level plan variance is out of scope without allocation rules.

## Pages

### Page 1 — Executive Overview

* Purpose: Provide leadership with a one-screen summary of actual sales performance, trend, and major drivers.
* Visuals:
  * KPI cards: Revenue, Gross Profit, Gross Margin %, Units Sold, Active Customers, Revenue YoY %.
  * Line chart: Revenue by Month with optional prior-year comparison.
  * Clustered column or bar: Revenue by Category.
  * Donut or stacked bar: Revenue Share by Traffic Channel.
  * Map or filled shape alternative: Revenue by Region/State.
  * Matrix: Top Categories/Segments with Revenue, Gross Profit, Gross Margin %, YoY %.
* Slicers: Year, Quarter/Month, Category, Region, Traffic Channel.
* Notes: Show a subtitle such as `Actuals through 2020-06-30` until newer actuals are loaded.

### Page 2 — Product & Segment Performance

* Purpose: Help category/product managers identify portfolio winners, margin issues, and mix shifts.
* Visuals:
  * KPI cards: Revenue, Gross Profit, Gross Margin %, Average Selling Price.
  * Bar chart: Revenue by Category → Segment hierarchy.
  * Scatter chart: Product Revenue vs Gross Margin %, bubble size = Units Sold.
  * Matrix: Product detail with Product, Category, Segment, Revenue, Units, ASP, Gross Profit, Gross Margin %.
  * Waterfall or decomposition tree: Revenue contribution by Category then Segment.
* Slicers: Year, Category, Segment, Region, Traffic Channel.
* Notes: If a Product visual is used, do not show Budget at Product level unless stakeholder approves allocation.

### Page 3 — Marketing Channel & Device Performance

* Purpose: Let Marketing Ops evaluate which channels/devices drive sales and customer reach.
* Visuals:
  * KPI cards: Revenue, Active Customers, Digital Revenue Share %, Average Selling Price.
  * 100% stacked column: Revenue mix by Traffic Channel over Month.
  * Bar chart: Revenue by Traffic Channel and Device.
  * Matrix: Traffic Channel × Device with Revenue, Units, Customers, Revenue Share %, Gross Margin %.
  * Trend line: Digital vs Offline Revenue by Month.
* Slicers: Year, Traffic Channel, Device, Category, Region.
* Notes: Digital definition defaults to all channels except Mail; confirm with Marketing.

### Page 4 — Geo Performance

* Purpose: Identify regional/district/state performance patterns for sales focus.
* Visuals:
  * KPI cards: Revenue, Gross Profit, Gross Margin %, Active Customers.
  * Map / Azure map: Revenue by State or City if geocoding is available.
  * Bar chart: Revenue by Region, then State.
  * Matrix: Region → State → District with Revenue, Units, Customers, Gross Margin %, YoY %.
  * Top/bottom list: Top 10 states or districts by Revenue and Bottom 10 by YoY %.
* Slicers: Year, Region, State, Category, Traffic Channel.
* Notes: Country is USA only; use Region/State/District for meaningful geo analysis.

### Page 5 — Budget & Forecast

* Purpose: Support Finance/FP&A and commercial leaders in plan tracking.
* Visuals:
  * KPI cards: Revenue, Budget, Budget Attainment %, Budget Variance, Budget Variance %.
  * Combo chart: Actual Revenue vs Budget by Month for 2020 YTD where actuals exist.
  * Bar chart: Budget Variance by Category-Segment.
  * Matrix: Category → Segment by Month with Revenue, Budget, Variance, Variance %, conditional formatting.
  * Plan-only line/column: 2021 Budget vs 2021 Forecast by Month and Category-Segment.
* Slicers: Year, Month, Category, Segment.
* Notes: Separate “Actual vs Budget” from “2021 Budget vs Forecast”. Do not imply 2021 under/over-performance without actuals.

## Filters

* Report-level:
  * Date range from Date table.
  * **Default filter values:** Year = `2020`, Month = `Jan–Jun` (latest complete actual period).
  * Exclude blank/missing Product, Customer, Campaign, Date relationships after validation.
* Page-level:
  * Page 5 Actual vs Budget visuals should filter to periods where Actual Revenue and Budget are both present (Year = 2020, Months 1–6).
  * Page 5 Forecast visuals should default to 2021 plan-only view.
* Visual-level:
  * Top/bottom product lists: Top N configurable default = 10.
  * Budget variance matrix: hide blank Budget unless intentionally showing unplanned actuals.

## Design notes

* Theme: `shared/themes/datapot-theme.json`. Page 1280×720. Format via theme, not per-visual.
* Use a consistent left-side or top filter panel across pages.
* Keep executive pages high-level; move dense matrices to detail pages.
* Use business-friendly names: Revenue, Gross Profit, Gross Margin %, Units Sold, Active Customers.
* Technical IDs should be hidden unless needed for drill-through.
* Add report info tooltip with assumptions:
  * Revenue = Units × Unit Price.
  * Currency assumed USD.
  * Actuals through 2020-06-30.
  * Budget/Forecast values assumed revenue targets.
* Conditional formatting:
  * Budget Attainment: below 95% = needs attention, 95–105% = near target, above 105% = favorable. Adjust thresholds after stakeholder review.
  * Gross Margin %: use relative color scale by visual context.

## Build-ready measure list

| Measure | Required for pages | Notes |
|---|---|---|
| Sales Lines | All pages optional | Use only if line count is useful; do not label as Orders. |
| Units Sold | Pages 1–4 | Base volume metric. |
| Customers | Pages 1, 3, 4 | Distinct active customers in Sales. |
| Revenue | All pages | Core actual metric. |
| COGS | Page 2 optional | Can be hidden but required for Gross Profit. |
| Gross Profit | Pages 1, 2, 4 | Profitability. |
| Gross Margin % | Pages 1–4 | Profitability rate. |
| Average Selling Price | Pages 2–3 | Price/mix indicator. |
| Revenue PY | Pages 1, 4 | Required for YoY. |
| Revenue YoY % | Pages 1, 4 | Use only where prior-year data exists. |
| Budget | Page 5 | Plan metric. |
| Forecast | Page 5 | Plan metric. |
| Budget Attainment % | Page 5 | Actual vs budget. |
| Budget Variance | Page 5 | Actual − Budget. |
| Budget Variance % | Page 5 | Variance ÷ Budget. |
| Digital Revenue | Page 3 | Revenue excluding Mail. |
| Digital Revenue Share % | Page 3 | Digital mix. |
