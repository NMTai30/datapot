# Build log — VanArsdel Sales Performance

Dated decisions and updates. Newest on top.

## 2026-07-04 - Synced data dictionary CSV to the reformatted .md
- Regenerated `dictionary/data-dictionary.csv` so it matches the reworked `data-dictionary.md` and the reference report CSV header (object_type,table,model_name,source_column,data_type,hidden,key,description_or_dax).
- Added the missing `CategorySegment` bridge and the `Category-Segment ID` keys on Product/Plan; dropped the stale Plan `Year`/`Month Name` columns.
- Expanded the measure list from 3 to all 19 measures with real DAX (commas inside DAX replaced with spaces so the CSV needs no quoting, matching the reference).

## 2026-07-04 - Trimmed data-gaps to remove cross-doc overlap
- Reduced `docs/data-gaps-and-stakeholder-questions.md` to a single authoritative stakeholder-question table (was 4 overlapping sections).
- Removed the Data quality questions section (fully covered by data-profile.md observations + cleaning checklist), the Scope risks section (covered by the BRD scope + the question table), and resolved/duplicated KPI questions (Revenue, Forecast Variance, Customer/Order count already answered in the table, BRD, or data dictionary).
- Migrated the two genuinely-open KPI decisions (Budget Attainment monthly-vs-YTD; Digital channel definition) into the main table as P2/P3 rows.
- Added a Related documents table pointing to where cleaning, contract quirks, scope/assumptions, and resolved KPI definitions live.

## 2026-07-04 - Aligned data dictionary to the reference report
- Reformatted `dictionary/data-dictionary.md` to match the example report's style (it had been following the bare template instead).
- Grouped tables under `## Dimensions` / `## Facts`; added source sheet + row count to each table heading.
- Switched to the 5-column layout (Model column | Source | Type | Hidden | Description); marked keys with `*` and foreign keys as `-> Table`, dropping the separate Key/Example columns and standalone Relationships table.
- Rewrote all measures as real DAX with numbered display folders (01 Volume ... 05 Time Intelligence).

## 2026-07-04 - Trimmed business glossary
- Rewrote `dictionary/business-glossary.md` to match the reference template's compact, business-concept-only style (~51 lines vs the reference ~53).
- Reduced to simple themed `Term | Definition` tables: Products, Marketing/channels, Customers/geography, Money/performance, Planning, Time.
- Moved out redundant detail: column/dataset mappings now point to the data dictionary; typo/currency/confirmation items live in the data gaps doc and dataset contract.

## 2026-07-04 - Right-sized dataset contract
- Recreated `dataset/DATASET-CONTRACT.md` (it had been lost during an interrupted edit).
- Matched the reference report's compact style: one schema table per source file, an inline Budget/Forecast unpivot summary (36 rows x 9 columns -> 324 Plan rows), and the 9 Category-Segment mapping.
- Kept only the high-value VanArsdel specifics: derived columns, refresh rules, a short "data quirks to confirm" table, and the Month x Category-Segment Actual-vs-Budget grain rule.
- Dropped the over-long acceptance-checks-with-severity and separate exceptions tables to keep it scannable (~47 lines vs the reference ~50).

## 2026-07-04 - Completed matrix subtotal values
- Filled missing subtotal values in Product, Channel, and Geo matrix placeholders.
- Ensured every subtotal/total row has numeric values under the displayed metric columns.
- Rechecked the SVG and confirmed there are no remaining `All` or `Grand total` placeholder values.

## 2026-07-04 - Corrected matrix total labels
- Replaced confusing `Grand total`/`All` placeholder text with `Total` and numeric sample values.
- Ensured matrix total rows show aggregated measures under Revenue, GP, GM %, Budget, and variance columns.

## 2026-07-04 - Clarified Power BI Matrix visuals
- Reworked matrix placeholders so they look like Power BI Matrix/pivot visuals instead of generic tables.
- Added indented hierarchy rows, expand/collapse markers, subtotal or grand total rows, and variance/margin cues.
- Updated `docs/wireframes.md` to explain the matrix convention in business/build terms.

## 2026-07-04 - Fixed matrix text overlap
- Removed duplicate legacy matrix grid lines that were rendering over annotation text.
- Adjusted matrix column separators to start below each header row so labels remain readable.
- Revalidated `docs/assets/wireframe-overview.svg` after the cleanup.

## 2026-07-04 - Made matrix placeholders explicit
- Added visible matrix/table placeholders with headers and grid cells to all matrix sections.
- Increased matrix line contrast so the tables are clearly visible in the SVG preview.
- Kept the matrix drawings separated from annotation text.

## 2026-07-04 - Reworked compact wireframe visuals
- Rebuilt compact Channel share as a small right-side donut with no label below it, preventing overlap with the legend text.
- Reworked short matrix boxes so grid thumbnails are right-aligned and do not cross annotation text.
- Regenerated all visual placeholders with a reserved annotation area at the top of each box.

## 2026-07-04 - Fixed wireframe chart overlaps and empty placeholders
- Moved all placeholder chart drawings below their title/annotation area so bars/lines do not overlap text.
- Added placeholder graphics to previously text-only boxes: scatter, stacked mix, map, top/bottom list, plan-only Budget vs Forecast, and matrices.
- Kept Channel share donut small and right-aligned inside its visual box.

## 2026-07-04 - Rebuilt dashboard wireframe overview
- Replaced `docs/assets/wireframe-overview.svg` with a multi-page mockup covering Executive, Product, Marketing, Geo, and Budget/Forecast pages.
- Added metric and dimension/axis/legend annotations directly inside each visual placeholder.
- Updated `docs/wireframes.md` with page coverage summary and clearer visual annotation expectations.

## 2026-07-04 - Removed generated Power BI project from final handoff
- Removed the generated Power BI project because it did not show useful information when opened in Power BI Desktop.
- Final deliverable is documentation-first: BRD, report spec, data dictionary, model design, data gaps, and wireframes so a DA can build the report manually and reliably.

## 2026-07-04 — BA Review & Fixes
- Added priority levels (P1/P2/P3) to data gaps and stakeholder questions for clear escalation.
- Split KPI definitions into Primary (must-have), Secondary (important), and Supporting (optional) tiers.
- Added explicit default filter values in report spec: Year = 2020, Month = Jan-Jun.
- Added `Channel Type` and `Is Digital` derived columns to Campaign table in data dictionary.
- Updated wireframe Page 1 to include Revenue YoY % KPI card (6 cards total).
- Added Digital Channel and Offline Channel definitions to business glossary.

## 2026-07-04 — Standardized to report-01 style
- Restructured report root `README.md` to match the style of `01-branch-channel-performance`:
  metadata table, contents, model-at-a-glance, usage steps, assumptions, status notes.
- Rewrote `dataset/DATASET-CONTRACT.md` into a contract-first format:
  files consumed, transformation contract, helper snippets, acceptance checks, output model tables.
- Added this `build-log.md` file so the docs package follows the same lifecycle documentation pattern.

## 2026-07-04 — Reconciled docs to provided Dataset.zip
- Validated source files from user-provided `Dataset.zip`:
  `VanArsdel_Actuals.xlsx`, `VanArsdel_Budget_Forecast.xlsx`, `Actuals_Path.txt`,
  `Budget_Path.txt`, `Number_Days.txt`.
- Corrected documentation mismatch: `Sales[Units]` is numeric in source (not text).
- Documented helper mismatch: `Budget_Path.txt` uses `Csv.Document(...)` while delivered budget source is `.xlsx`.
- Copied provided raw files into `dataset/raw/` for reproducible handoff.
