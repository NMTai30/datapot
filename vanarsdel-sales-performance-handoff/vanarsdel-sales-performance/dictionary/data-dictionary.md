# Data dictionary — VanArsdel Sales Performance

Lock-step with the semantic model built from `dataset/raw/`. Currency = **USD** (assumed — see [data gaps](../docs/data-gaps-and-stakeholder-questions.md)). Hidden columns are not shown to report users (keys, raw fact inputs). `★` = table key; `→ Table` marks a foreign key.

## Dimensions

### `Date`  (Actuals `Date` sheet · 2,192 rows · 2016-01-01→2021-12-31)
| Model column | Source | Type | Hidden | Description |
|---|---|---|---|---|
| Date ★ | Date | dateTime | no | Calendar date / relationship key (Excel serial in source). Extend to 2015 to cover early Sales, or exclude 2015. |
| Month Number | MonthNo | int64 | yes | Month number 1–12 (sort key) |
| Month Name | MonthName | string | no | Sorted by Month Number |
| Month ID | MonthID | int64 | yes | `YYYYMM` numeric key |
| Month | Month | string | no | User-facing label, e.g. `Jan-21` |
| Quarter | Quarter | string | no | Q1–Q4 |
| Year | Year | int64 | no | Calendar year |

### `Campaign`  (Actuals `Campaign` sheet · 22 rows)
| Model column | Source | Type | Hidden | Description |
|---|---|---|---|---|
| Campaign ID ★ | CampaignID | int64 | yes | Channel/device lookup key |
| Traffic Channel | TrafficChannel | string | no | Marketing/source channel; trim & fix typo `Affliliate` |
| Device | Device | string | no | Desktop / Mobile / Tablet / Paper; fix typo `Deskop` |
| Channel Type | ChannelType (derived) | string | no | `Digital` for all except Mail; `Offline` for Mail |
| Is Digital | IsDigital (derived) | boolean | no | TRUE when Channel Type = Digital |

### `Customer`  (Actuals `Customer` sheet · 282,597 rows)
| Model column | Source | Type | Hidden | Description |
|---|---|---|---|---|
| Customer ID ★ | CustomerID | string | yes | Preserve leading zeroes, e.g. `000001` |
| Zip Code | ZipCode | string | yes | → Geo; preserve leading zeroes |
| Customer Contact | Email Name | string | yes | Contact-like text; hidden for privacy |

### `Product`  (Actuals `Product` sheet · 212 rows)
| Model column | Source | Type | Hidden | Description |
|---|---|---|---|---|
| Product ID ★ | ProductID | string | yes | Product / SKU identifier |
| Product | Product | string | no | Product / SKU name |
| Category | Category | string | no | Accessory / Mix / Rural / Urban / Youth |
| Segment | Segment | string | no | Sub-group within Category |
| Category-Segment ID | (derived) | string | yes | → CategorySegment; `Category\|Segment` |
| Manufacturer | Manufacturer | string | no | All rows = VanArsdel (no competitor data) |
| Manufacturer ID | ManufacturerID | string | yes | Only value `7` in current file |
| Unit Cost | Unit Cost | double | no | Cost per unit — basis for COGS |
| Unit Selling Price | Unit Price | double | no | Price per unit — basis for Revenue |

### `CategorySegment`  (derived bridge · one row per Category-Segment)
| Model column | Source | Type | Hidden | Description |
|---|---|---|---|---|
| Category-Segment ID ★ | (derived) | string | yes | `Category\|Segment`; links Product and Plan |
| Category | (derived) | string | no | Product category |
| Segment | (derived) | string | no | Product segment |
| Category-Segment | (derived) | string | no | Planning label, e.g. `Urban-Moderation` |

*Filter path: a Category-Segment slicer filters `Plan` directly and `Sales` via `Product`, so actual-vs-plan is valid at Month × Category-Segment or higher.*

### `Geo`  (Actuals `Geo` sheet · 39,948 rows · USA only)
| Model column | Source | Type | Hidden | Description |
|---|---|---|---|---|
| Zip ★ | Zip | string | yes | Joined from Customer Zip Code; preserve leading zeroes |
| City | City | string | no | City label |
| State | State | string | no | US state abbreviation |
| Region | Region | string | no | East / Central / West |
| District | District | string | no | Sales district |
| Country | Country | string | no | USA only in current data |

## Facts

### `Sales`  (Actuals `Sales` sheet · 675,368 rows · 1 line per Product × Date × Customer × Campaign)
Covers 2015-01-01→2020-06-30. No OrderID/invoice key — count lines with `COUNTROWS`, do not call them orders.

| Model column | Source | Type | Hidden | Description |
|---|---|---|---|---|
| Product ID | ProductID | string | yes | → Product |
| Transaction Date | Date | dateTime | yes | → Date (Excel serial in source) |
| Customer ID | CustomerID | string | yes | → Customer |
| Campaign ID | CampaignID | int64 | yes | → Campaign |
| Units Sold | Units | int64 | no | Quantity sold; currently all rows = 1 |

### `Plan`  (Budget/Forecast `Sheet1`, unpivoted · 324 rows · 1 row per Type × Month × Category-Segment)
Budget 2020–2021; Forecast 2021. Wide source is unpivoted (36 rows × 9 Category-Segment columns → 324 rows).

| Model column | Source | Type | Hidden | Description |
|---|---|---|---|---|
| Plan Type | Type | string | no | Budget / Forecast |
| Month Start Date | (derived) | dateTime | yes | → Date (1st of month) |
| Month ID | (derived) | int64 | yes | `YYYYMM` |
| Category-Segment ID | (derived) | string | yes | → CategorySegment |
| Category-Segment | (from wide header) | string | no | Planning label, e.g. `Urban-Moderation` |
| Plan Amount | (unpivoted value) | double | no | Monetary plan; assumed revenue target (confirm) |

## Measures  (all on `Key Measures`)

| Measure | Folder | Format | DAX |
|---|---|---|---|
| Sales Lines | 01 Volume | #,0 | `COUNTROWS(Sales)` |
| Units Sold | 01 Volume | #,0 | `SUM(Sales[Units Sold])` |
| Customers | 01 Volume | #,0 | `DISTINCTCOUNT(Sales[Customer ID])` |
| Revenue | 02 Revenue & Profit | $#,0 | `SUMX(Sales, Sales[Units Sold] * RELATED(Product[Unit Selling Price]))` |
| COGS | 02 Revenue & Profit | $#,0 | `SUMX(Sales, Sales[Units Sold] * RELATED(Product[Unit Cost]))` |
| Gross Profit | 02 Revenue & Profit | $#,0 | `[Revenue] - [COGS]` |
| Gross Margin % | 02 Revenue & Profit | 0.0% | `DIVIDE([Gross Profit], [Revenue])` |
| Average Selling Price | 02 Revenue & Profit | $0.00 | `DIVIDE([Revenue], [Units Sold])` |
| Budget | 03 Planning | $#,0 | `CALCULATE(SUM(Plan[Plan Amount]), Plan[Plan Type]="Budget")` |
| Forecast | 03 Planning | $#,0 | `CALCULATE(SUM(Plan[Plan Amount]), Plan[Plan Type]="Forecast")` |
| Budget Attainment % | 03 Planning | 0.0% | `DIVIDE([Revenue], [Budget])` |
| Budget Variance | 03 Planning | $#,0 | `[Revenue] - [Budget]` |
| Budget Variance % | 03 Planning | 0.0% | `DIVIDE([Budget Variance], [Budget])` |
| Forecast Variance | 03 Planning | $#,0 | `[Revenue] - [Forecast]` |
| Digital Revenue | 04 Channel & Mix | $#,0 | `CALCULATE([Revenue], Campaign[Is Digital]=TRUE)` |
| Digital Revenue Share % | 04 Channel & Mix | 0.0% | `DIVIDE([Digital Revenue], [Revenue])` |
| Revenue Share % | 04 Channel & Mix | 0.0% | `DIVIDE([Revenue], CALCULATE([Revenue], ALLSELECTED()))` |
| Revenue PY | 05 Time Intelligence | $#,0 | `CALCULATE([Revenue], DATEADD('Date'[Date], -1, YEAR))` |
| Revenue YoY % | 05 Time Intelligence | 0.0% | `DIVIDE([Revenue] - [Revenue PY], [Revenue PY])` |
