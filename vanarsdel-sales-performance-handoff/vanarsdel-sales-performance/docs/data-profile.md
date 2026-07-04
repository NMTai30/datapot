# Data Profile — VanArsdel Source Files

## Source files inspected

| File | Tables / sheets used | Notes |
|---|---|---|
| `VanArsdel_Actuals.xlsx` | Date, Campaign, Customer, Product, Geo, Sales | Main actuals workbook. |
| `VanArsdel_Budget_Forecast.xlsx` | Sheet1 | Monthly plan workbook in wide format. |
| `Actuals_Path.txt`, `Budget_Path.txt`, `Number_Days.txt` | Power Query snippets | Reference only; not source facts/dimensions. |

## Row counts and coverage

| Table | Rows excluding header | Grain | Key coverage / date coverage |
|---|---:|---|---|
| Date | 2,192 | One row per calendar date | 2016-01-01 to 2021-12-31 |
| Campaign | 22 | One row per campaign channel/device combination | CampaignID 1–22 |
| Customer | 282,597 | One row per customer | CustomerID is text with leading zeroes |
| Product | 212 | One row per product | All products are Manufacturer `VanArsdel` |
| Geo | 39,948 | One row per zip code | USA only; Region values: East, Central, West |
| Sales | 675,368 | One sales line per Product × Date × Customer × Campaign | Actual transaction dates 2015-01-01 to 2020-06-30 |
| Budget/Forecast | 36 data rows before unpivot | One row per Type × Year × Month, wide by Category-Segment | Budget 2020–2021; Forecast 2021 |

## Key observations

1. `Sales[Units]` is already numeric (`int64`) and all values are `1` in the current file.
2. Sales actuals begin in 2015, but the Date dimension begins in 2016. This will break date joins for 2015 unless the Date table is extended or 2015 is intentionally excluded.
3. Budget/Forecast is monthly and at Category-Segment grain, while Sales is daily/product/customer/campaign grain. DA must aggregate actual revenue to Month × Category × Segment before comparing to plan.
4. Budget exists for 2020 and 2021; Forecast exists only for 2021. Actuals stop on 2020-06-30, so 2021 forecast is a future-looking view only unless newer actuals are loaded.
5. Product table contains only VanArsdel products; `Manufacturer` does not enable competitor analysis in the current dataset.
6. Campaign data has text quality issues: trailing spaces, `Affliliate`, and `Deskop`.
7. Customer table contains email/display-name strings. Hide this field unless the dashboard explicitly requires customer-level contact detail.
8. Budget/Forecast includes `Rural-Select` but not `Rural-Productivity`; Product table contains both. This needs stakeholder confirmation.
9. `Budget_Path.txt` reads CSV in its current M code, while the supplied budget source is Excel (`VanArsdel_Budget_Forecast.xlsx`). This helper is likely a legacy snippet unless budget is exported to CSV.

## Actual vs Budget reasonableness check

Budget values appear to represent monetary revenue targets. For example, January 2020 actual revenue by Category-Segment is directionally close to January 2020 budget values:

| Category-Segment | Jan 2020 actual revenue | Jan 2020 budget |
|---|---:|---:|
| Accessory-Accessory | 47,893.08 | 52,269.27 |
| Mix-All Season | 11,433.22 | 12,227.37 |
| Mix-Productivity | 18,422.39 | 19,160.37 |
| Rural-Select | 328.11 | 334.68 |
| Urban-Convenience | 125,012.23 | 129,397.14 |
| Urban-Extreme | 17,482.05 | 18,422.57 |
| Urban-Moderation | 285,568.14 | 289,792.45 |
| Urban-Regular | 385.98 | 417.30 |
| Youth-Youth | 2,371.46 | 2,569.91 |

## Recommended cleaning checklist

| Issue | Proposed action | Owner |
|---|---|---|
| Date mismatch for 2015 actuals | Extend Date table to start at 2015-01-01 or exclude 2015 from reporting scope. | DA + stakeholder |
| Campaign typos/spaces | Trim and standardize values in Power Query or model layer. | DA |
| Budget/Forecast wide format | Unpivot to normalized `Plan` fact. | DA |
| Customer contact field | Hide by default; consider removing from report dataset if not needed. | DA + governance |
| Currency ambiguity | Confirm with VanArsdel before publishing KPI labels. | Business owner |
