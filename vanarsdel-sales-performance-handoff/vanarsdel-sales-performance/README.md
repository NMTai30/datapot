# 01 — VanArsdel Sales Performance

> Executive dashboard handoff pack for Actuals + Budget/Forecast analysis (retail sales domain).

| | |
|---|---|
| **Domain** | Retail — sales, margin, channel, geography, budget tracking |
| **Status** | Documentation handoff ready |
| **Audience** | Commercial leadership, Sales Ops, Marketing Ops, Product/Category Managers, FP&A |
| **Semantic model** | Specified in `docs/model-design.md` |
| **Report** | Specified in `docs/report-spec.md` and `docs/wireframes.md` |
| **Data** | VanArsdel dataset in `dataset/raw/` |
| **Refresh** | Monthly (after stakeholder confirms source refresh cadence) |

## Contents
- `dataset/` — source files + [DATASET-CONTRACT.md](dataset/DATASET-CONTRACT.md)
- `dictionary/` — [business-glossary.md](dictionary/business-glossary.md) · [data-dictionary.md](dictionary/data-dictionary.md) · [data-dictionary.csv](dictionary/data-dictionary.csv)
- `docs/` — [business-requirements-document](docs/business-requirements-document.md) · [report-spec](docs/report-spec.md) · [model-design](docs/model-design.md) · [data-profile](docs/data-profile.md) · [data-gaps](docs/data-gaps-and-stakeholder-questions.md) · [wireframes](docs/wireframes.md) · [build-log](docs/build-log.md)

## Model at a glance
- **Facts:** `Sales` (actuals), `Plan` (Budget/Forecast normalized).
- **Dimensions:** `Date`, `Product`, `Campaign`, `Customer`, `Geo`, plus `CategorySegment` bridge.
- **Key outputs:** Revenue, COGS, Gross Profit, Gross Margin %, Revenue YoY %, Budget Attainment %, Budget Variance.

## How to use this handoff
1. Confirm `dataset/raw/` has the required files from [DATASET-CONTRACT.md](dataset/DATASET-CONTRACT.md).
2. Read in order: glossary → data profile → data dictionary → data gaps → BRD → report spec → wireframes → model design.
3. Use `docs/model-design.md`, `docs/report-spec.md`, and `docs/wireframes.md` to build the Power BI report manually.
4. Validate assumptions with stakeholders before final publish (currency, revenue definition, plan comparison scope).

## Important assumptions
- Revenue is derived as `Sales[Units] × Product[Unit Price]` (gross, pending finance confirmation).
- COGS is derived as `Sales[Units] × Product[Unit Cost]`.
- Budget/Forecast are assumed to be monthly revenue targets by `Category-Segment`.
- Actuals end at `2020-06-30`; 2021 Forecast is plan-only unless newer actuals are loaded.
- Date dimension in source starts at `2016-01-01` while Sales starts at `2015-01-01`; extend Date or exclude 2015.
- `Budget_Path.txt` currently uses CSV logic (`Csv.Document`) and is not a direct loader for the supplied budget `.xlsx`.

## Status notes
- ✅ Completed: glossary, data dictionary, data profile, data gaps, BRD, report spec, wireframes, model design.
- Pending: DA builds the Power BI report from the handoff documents and stakeholder signs off open questions.
