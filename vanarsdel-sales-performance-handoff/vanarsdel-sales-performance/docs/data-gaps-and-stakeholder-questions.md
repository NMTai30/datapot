# Data Gaps & Stakeholder Questions — VanArsdel

## Purpose

The open items to clarify with VanArsdel before the semantic model and report are treated as final. This file is the **single authoritative list of stakeholder questions**, each with a proposed default so the DA can proceed even if an answer is pending.

Data-quality/cleaning handling, scope, and source-contract details are not repeated here — see **Related documents** at the end.

## Stakeholder questions (with proposed defaults)

Grouped by business impact (P1 highest). Each row is a question the DA needs answered; the "proposed default" is what to build if the stakeholder is unavailable.

| Priority | Area | Gap / ambiguity | Why it matters | Proposed default if stakeholder is unavailable | Question to stakeholder |
|---|---|---|---|---|---|
| **P1** | Revenue definition | No Revenue column exists in Sales. | KPI is core to dashboard; calculation method affects all comparisons. | Revenue = Units × Unit Price. | Is Revenue supposed to be gross list-price revenue, net revenue after discount, or invoiced revenue? |
| **P1** | Currency | Currency is not stated. | Labels and formatting require currency. | Use USD `$` because Geo is USA only. | What currency should all monetary fields use? Are amounts tax-inclusive? |
| **P1** | Actuals date coverage | Sales actuals run from 2015-01-01 to 2020-06-30, but Date dimension starts at 2016-01-01. | 2015 sales will not join to Date; time intelligence and visuals can be wrong. | Extend Date to 2015-01-01 through 2021-12-31. | Should 2015 actuals be included in scope, or should reporting start in 2016? |
| **P2** | Plan/actual alignment | Budget exists for 2020–2021; Forecast exists for 2021; Actuals stop at 2020-06-30. | 2021 forecast cannot be compared to actuals in current data. | Use Budget Attainment only for periods with actuals and budget; show 2021 Forecast as plan-only. | Are there actuals after 2020-06-30? If not, should 2021 be shown only as future plan? |
| **P2** | Budget/Forecast unit | Plan values are numeric but not labeled as revenue, units, or profit. | Plan comparison may be invalid if units/profit. | Treat as revenue targets because magnitude aligns with derived actual revenue. | Are Budget/Forecast values revenue, units, gross profit, or another metric? |
| **P2** | Budget Attainment basis | Comparison window (monthly vs YTD) is not specified. | Attainment reads differently on a monthly vs cumulative axis. | Provide both monthly and YTD, only for periods where actuals exist. | Should Budget Attainment be tracked monthly, year-to-date, or both? |
| **P2** | Product plan grain | Plan is at Category-Segment, not Product. | Product-level plan variance cannot be built without allocation rules. | Compare actual vs plan only at Category-Segment or above. | Do you need product-level budget allocation? If yes, what allocation rule should be used? |
| **P3** | Digital channel definition | The set of channels counted as "Digital" is not formally defined. | Digital Revenue Share % depends on it. | Digital = all Traffic Channels except Mail (Paper). | Confirm which Traffic Channels count as Digital vs Offline. |
| **P3** | Discounts/tax/returns | No discount, tax, refund, or return columns exist. | Net revenue and margin may be overstated if omitted. | Out of scope; use gross revenue based on unit price. | Are discounts, returns, rebates, shipping, or tax included elsewhere? |
| **P3** | `Rural-Productivity` missing from plan | Product table contains Rural Productivity, but Budget/Forecast does not. | Actuals may have small revenue with no plan, producing blank budget. | Keep as unplanned actuals and flag in data quality. | Should Rural Productivity have a plan, or is it intentionally excluded? |
| **P3** | Manufacturer | Product table has ManufacturerID and Manufacturer but all rows are VanArsdel. | Competitor/manufacturer analysis is not possible. | Hide ManufacturerID; keep Manufacturer as descriptive only. | Is VanArsdel selling only its own products, or should competitor products be added later? |
| **P3** | Customer privacy | `Email Name` contains identifiable-looking emails/names. | Could expose PII-like data in report. | Hide field from report view and exclude from visuals. | Is customer contact detail allowed in the dashboard? |

## Related documents

To avoid duplication, issues that are already handled elsewhere are not repeated in the table above:

| Topic | Where it lives |
|---|---|
| Data-quality/cleaning tasks (trailing spaces, `Affliliate`/`Deskop` typos, text keys, `Units` all `1`, `Budget_Path.txt` CSV-vs-xlsx) | [docs/data-profile.md](data-profile.md) — Key observations & Recommended cleaning checklist |
| Source-file contract quirks and refresh rules | [dataset/DATASET-CONTRACT.md](../dataset/DATASET-CONTRACT.md) — Data quirks to confirm |
| In/out of scope, assumptions, and sign-off owners | [docs/business-requirements-document.md](business-requirements-document.md) — Scope, Assumptions, Open items for sign-off |
| Resolved KPI definitions (e.g. Active Customers = DISTINCTCOUNT, Sales Lines not Orders) | [dictionary/data-dictionary.md](../dictionary/data-dictionary.md) — Measures |
| Modeling decision that removes plan double-counting (CategorySegment bridge) | [docs/model-design.md](model-design.md) |
