# Business Requirements Document — VanArsdel Sales Performance Dashboard

## 1. Objective

Build a Power BI dashboard that helps VanArsdel commercial stakeholders understand sales performance, profitability, product/category mix, marketing channel contribution, regional performance, and budget attainment using the Actuals and Budget/Forecast data provided.

The dashboard should translate raw product/customer/campaign sales data into business KPIs and actionable breakdowns. It should be precise enough for a DA to build without repeatedly asking for metric definitions, grains, or visual intent.

## 2. Audience and decisions supported

| Audience / role | Decisions supported |
|---|---|
| Commercial / Sales Leadership | Is revenue growing? Which categories, regions, or channels are driving performance? Are actuals on track against budget? |
| Sales Operations | Which regions/districts need attention? How are sales lines, units, and active customers trending? |
| Product / Category Managers | Which categories/segments/products contribute most to revenue and gross profit? Where is margin strongest or weakest? |
| Marketing Operations | Which traffic channels and devices are driving revenue/customers? What is the digital/offline mix? |
| Finance / FP&A | Are actuals aligned with Budget? Where are positive/negative variances by month and Category-Segment? |

## 3. Business questions the dashboard must answer

1. What is total revenue, units sold, gross profit, gross margin %, and active customers over the selected period?
2. How is revenue trending by month/quarter/year, and what is the YoY growth rate?
3. Which product categories and segments are driving revenue, profit, and margin?
4. Which individual products are top/bottom performers by revenue, units, and margin?
5. How much revenue comes from each marketing channel and device?
6. What share of revenue is digital vs offline/direct mail?
7. Which regions, states, and districts contribute most to sales and customers?
8. How do actuals compare with Budget at Month × Category-Segment level?
9. Where are budget over/under-performance pockets that need commercial action?
10. What forecast exists for 2021, and how should it be interpreted given actuals currently stop at 2020-06-30?

## 4. KPI and metric definitions

### 4.1 Primary KPIs (must-have for MVP)

| KPI | Business definition | Formula / expected measure logic | Grain for valid interpretation | Notes |
|---|---|---|---|---|
| Revenue | Actual sales monetary value. | `SUMX(Sales, Sales[Units] × RELATED(Product[Unit Price]))` | Product/date/customer/campaign actuals | **Core metric.** Assumed gross revenue; confirm net/gross. |
| Gross Profit | Revenue after product cost. | `Revenue − COGS` | Same as Revenue | Excludes operating expense/marketing spend. |
| Gross Margin % | Profitability rate. | `Gross Profit ÷ Revenue` | Any aggregation with Revenue | Show blank when Revenue is zero. |
| Units Sold | Total quantity sold. | `SUM(Sales[Units])` | Any aggregation of Sales | Current source Units all equal 1. |
| Active Customers | Unique customers with at least one sales line. | `DISTINCTCOUNT(Sales[CustomerID])` | Selected period and filters | Uses Sales fact, not full Customer table count. |
| Budget | Approved revenue target. | `SUM(Plan[Plan Amount])` where `Plan Type = Budget` | Month × Category × Segment | Plan values assumed revenue. |
| Budget Attainment % | Actual revenue as percent of budget. | `Revenue ÷ Budget` | Month/YTD × Category-Segment or higher | Show only where Actual and Budget both exist. |

### 4.2 Secondary KPIs (important but build after primary)

| KPI | Business definition | Formula / expected measure logic | Grain for valid interpretation | Notes |
|---|---|---|---|---|
| COGS | Cost of goods sold. | `SUMX(Sales, Sales[Units] × RELATED(Product[Unit Cost]))` | Same as Revenue | Uses Product Unit Cost. Hidden; required for Gross Profit. |
| Revenue YoY % | Year-over-year revenue growth. | `(Revenue − Revenue PY) ÷ Revenue PY` | Date context with complete prior year | Requires Date table coverage. |
| Budget Variance | Dollar gap to budget. | `Revenue − Budget` | Month/YTD × Category-Segment or higher | Positive = above budget. |
| Budget Variance % | Percent gap to budget. | `Budget Variance ÷ Budget` | Same as Budget Variance | Use conditional formatting. |
| Digital Revenue Share % | Digital revenue as percent of total. | `Digital Revenue ÷ Revenue` | Channel/device/time | Digital = all channels except Mail. |
| Average Selling Price | Average revenue per unit. | `Revenue ÷ Units Sold` | Product/category/period | Weighted by units. |

### 4.3 Supporting metrics (optional / derived)

| KPI | Business definition | Formula / expected measure logic | Grain for valid interpretation | Notes |
|---|---|---|---|---|
| Sales Lines | Count of sales fact rows. | `COUNTROWS(Sales)` | Product × Date × Customer × Campaign line | Do not call this Orders; no OrderID exists. |
| Forecast | Latest revenue forecast. | `SUM(Plan[Plan Amount])` where `Plan Type = Forecast` | Month × Category × Segment | Forecast exists only for 2021. |
| Revenue Share % | Contribution to selected total. | `Revenue ÷ Revenue over selected total` | Category/channel/geo views | Use for mix analysis. |
| Digital Revenue | Revenue through digital channels. | Revenue where `Is Digital = TRUE` | Channel/device | Requires derived `Is Digital` column on Campaign. |

## 5. Scope

### In scope

* Actual sales performance from source Sales data.
* Product/category/segment performance.
* Campaign traffic channel/device performance.
* Geography performance by Region, State, District, City/Zip where useful.
* Budget attainment using monthly Budget values where actuals and budget overlap.
* Forecast display as plan-only future view unless additional actuals are loaded.
* Business glossary, data dictionary, report spec, and wireframe handoff.

### Out of scope

* Competitor/manufacturer market share; current Product table includes only VanArsdel.
* Customer-level contact outreach workflows.
* Net revenue after taxes, returns, discounts, shipping, rebates, or marketing spend unless new fields are supplied.
* Product-level budget allocation unless stakeholder provides allocation rules.
* Building the final Power BI report in this documentation handoff pack.

## 6. Assumptions

1. Currency is USD.
2. `Unit Price` is the selling price to use for actual revenue.
3. `Unit Cost` is the cost basis to use for COGS.
4. Budget/Forecast values represent revenue targets/forecast amounts.
5. `Mail` + `Paper` is an offline/direct-mail channel; all other channels are digital.
6. Sales lines are not the same as customer orders because no order ID exists.
7. CampaignID represents Channel × Device, not a creative/campaign-name hierarchy.
8. Customer contact data should be hidden from general report users.
9. Date should be extended to include all Sales and Plan dates unless reporting explicitly starts in 2016.

## 7. Success criteria

A DA can build the report if:

* All fact/dimension relationships are explicit.
* Revenue, budget, forecast, variance, and margin formulas are clear.
* Report pages specify visuals, dimensions, metrics, and filters.
* Known gaps are documented and either resolved or implemented with default assumptions.
* The model prevents double counting when comparing Actuals to Budget/Forecast.

## 8. Open items for sign-off

| Item | Owner | Needed by |
|---|---|---|
| Confirm revenue and currency definitions. | Finance / FP&A | Before measure build finalization |
| Confirm actuals date scope and whether 2015 should be included. | Business owner | Before Date table build |
| Confirm Budget/Forecast unit and comparison rules. | FP&A | Before Budget Attainment page build |
| Confirm campaign label corrections. | Marketing Ops | Before final report labels |
| Confirm privacy treatment of Customer Contact. | Data owner | Before publish |
