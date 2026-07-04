# Dataset contract - VanArsdel Sales Performance

What `dataset/raw/` must contain for this report. This is the agreement between the data producer and the semantic model: keep the files in the shape below and the dashboard can be refreshed without re-interpreting the data. Source = the VanArsdel retail sample (Actuals 2015-2020, Budget/Forecast 2020-2021).

> Status: source data profiled and reconciled with the handoff docs. See [docs/data-profile.md](../docs/data-profile.md).

## Files consumed by THIS report

### `VanArsdel_Actuals.xlsx` -> dimensions + fact `Sales`

| Sheet | Model table | Grain | Rows | Key columns |
|-------|-------------|-------|-----:|-------------|
| Date | `Date` | one row per date | 2,192 | Date, MonthNo, MonthName, MonthID, Month, Quarter, Year (2016-01-01 -> 2021-12-31) |
| Campaign | `Campaign` | one row per channel/device | 22 | CampaignID, TrafficChannel, Device |
| Customer | `Customer` | one row per customer | 282,597 | CustomerID (text), ZipCode (-> Geo), Email Name (hide) |
| Product | `Product` | one row per SKU | 212 | ProductID, Product, Category, Segment, Manufacturer (all = VanArsdel), Unit Cost, Unit Price |
| Geo | `Geo` | one row per ZIP | 39,948 | Zip, City, State, Region (East/Central/West), District, Country (USA only) |
| Sales | `Sales` | one sales line | 675,368 | ProductID, Date, CustomerID, CampaignID, Units (2015-01-01 -> 2020-06-30) |

Fact `Sales` foreign keys: `ProductID -> Product`, `Date -> Date`, `CustomerID -> Customer`, `CampaignID -> Campaign`. `Units` is integer and currently all = 1. There is no OrderID/invoice key - count lines with `COUNTROWS(Sales)`, do not label it "orders".

Derived in the model: Channel Type / Is Digital (from Campaign, pending stakeholder rule) and CategorySegment ID = `Category | Segment` (joins Product and Plan to the shared planning grain).

### `VanArsdel_Budget_Forecast.xlsx`, `Sheet1` -> fact `Plan`

Wide monthly plan: row 1 is a title (skip it), row 2 is the header `Type, Year, Month` + one column per Category-Segment. `Type` in {Budget, Forecast} (Budget 2020-2021, Forecast 2021). The model unpivots the 9 Category-Segment columns, so 36 source rows become 324 `Plan` rows, then splits each label on the first `-` into Category + Segment and builds `CategorySegment ID`. The unpivoted value column is `Plan Amount` (assumed revenue target - confirm).

| Category-Segment column | Category | Segment |
|-------------------------|----------|---------|
| Accessory-Accessory | Accessory | Accessory |
| Mix-All Season | Mix | All Season |
| Mix-Productivity | Mix | Productivity |
| Rural-Select | Rural | Select |
| Urban-Convenience | Urban | Convenience |
| Urban-Extreme | Urban | Extreme |
| Urban-Moderation | Urban | Moderation |
| Urban-Regular | Urban | Regular |
| Youth-Youth | Youth | Youth |

### Power Query helper files (reference only)
`Actuals_Path.txt`, `Budget_Path.txt`, `Number_Days.txt` show the intended load pattern; they are not model tables. Note `Budget_Path.txt` uses `Csv.Document(...)` but the supplied budget is `.xlsx` - reconcile if you rebuild Power Query.

## Files NOT used by this report
No other source files were supplied. If VanArsdel later provides returns, discounts, tax, promo spend, inventory, or order-header data, add it here before extending the model.

## Refresh rules
- Keep file names, sheet names, and column names/order stable; if any change, update this contract, the [data dictionary](../dictionary/data-dictionary.md), and the queries together.
- Preserve text keys with leading zeroes (`CustomerID`, `ZipCode`, `ProductID`, `Zip`) - do not auto-convert to numbers.
- Trim/standardize Campaign text (`Affliliate`, `Deskop`, trailing spaces) on load.
- Extend `Date` to cover the full Sales range (2015 sales currently fall outside the 2016-start Date table) before publishing time-intelligence.
- If new Category-Segment plan columns appear, rebuild `CategorySegment` and recheck Actual-vs-Budget.

## Data quirks to confirm with stakeholders
| Issue | Impact | Decision needed |
|-------|--------|-----------------|
| Actuals include 2015 but `Date` starts 2016 | 2015 sales will not join to `Date` | Extend Date to 2015, or exclude 2015 |
| `Budget_Path.txt` reads CSV but file is `.xlsx` | Helper does not match source format | Fix helper/query or export CSV |
| `Plan Amount` basis unstated | Drives Budget Attainment KPI | Confirm gross/net revenue target |
| Product has `Rural-Productivity`, budget has `Rural-Select` | Category-Segment mismatch | Confirm intended plan coverage |
| Currency unstated; no discount/tax/returns | Revenue is gross sales-line only | Confirm formatting and scope |

Actual-vs-Budget is only valid at Month x Category-Segment or higher - never at Product/Customer/Campaign grain.