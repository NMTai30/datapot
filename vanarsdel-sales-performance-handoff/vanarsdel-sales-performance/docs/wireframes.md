# Wireframes — VanArsdel Sales Performance Dashboard

## Layout conventions

* Canvas: 1280 x 720 for each actual report page.
* The SVG overview is a larger composite canvas showing all planned pages together.
* Slicers: consistent top strip or left panel.
* KPI cards: top row for page-level summary.
* Main insight visual: center-left / largest area.
* Supporting breakdowns: right side and bottom area.
* Detail tables/matrices: bottom half or dedicated detail pages.
* Every visual must state both the metric and the dimension/axis/legend it uses.
* Power BI Matrix visuals are shown as pivot-style tables: indented hierarchy rows, expand/collapse markers (`[-]` / `[+]`), subtotal or grand total rows, and optional conditional formatting cells. They are not chart visuals.


## Page coverage summary

| Page | Primary user | Main decision | Must-have visuals |
|---|---|---|---|
| Executive Overview | Leadership | Is the business growing, and what are the main drivers? | KPI cards, revenue trend, category/channel/geo breakdowns, Power BI Matrix for category-segment |
| Product & Segment | Category managers | Which categories, segments, and SKUs drive revenue and margin? | KPI cards, category-segment bar, product scatter, Power BI Matrix for product hierarchy |
| Marketing Channel & Device | Marketing Ops | Which channels/devices drive revenue and digital mix? | KPI cards, channel mix trend, channel-device bar, digital/offline trend, Power BI Matrix for channel-device |
| Geo Performance | Sales Ops / Regional managers | Which regions/states/districts need action? | KPI cards, map/state view, region-state bar, Power BI Matrix for geo hierarchy, top/bottom list |
| Budget & Forecast | Finance / FP&A | Where are actuals above/below budget, and what is the 2021 plan? | KPI cards, actual-vs-budget combo, variance bar, Power BI Matrix for budget variance, budget-vs-forecast view |

## Page 1 — Executive Overview

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ VanArsdel Sales Performance — Executive Overview                              │
│ Slicers: Year | Quarter/Month | Category | Region | Traffic Channel           │
├────────────┬────────────┬────────────┬────────────┬────────────┬─────────────┤
│ KPI Revenue│ KPI GP     │ KPI GM %   │ KPI Units  │ KPI Cust.  │ KPI YoY %   │
│ [Revenue]  │ [GP]       │ [GM%]      │ [UnitsSold]│ [Customers]│ [Rev YoY%]  │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ Line chart: Revenue by Month                  │ Bar: Revenue by Category      │
│ Metric: [Revenue], optional [Revenue PY]      │ Metric: [Revenue]             │
│ Axis: Date[Month]                             │ Axis: Product[Category]       │
├──────────────────────────────┬───────────────┴───────────────────────────────┤
│ Donut/stacked: Channel share │ Power BI Matrix: Category/Segment performance   │
│ Metric: [Revenue Share %]    │ Metrics: Revenue, GP, GM%, YoY%                 │
│ Legend: Traffic Channel      │ Rows: Category > Segment; subtotal/total         │
└──────────────────────────────┴───────────────────────────────────────────────┘
```

## Page 2 — Product & Segment Performance

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Product & Segment Performance                                                 │
│ Slicers: Year | Category | Segment | Region | Traffic Channel                 │
├──────────────┬──────────────┬──────────────┬──────────────┬──────────────────┤
│ KPI Revenue  │ KPI GP       │ KPI GM %     │ KPI ASP      │ KPI Units         │
├──────────────────────────────┬───────────────────────────────────────────────┤
│ Bar hierarchy: Revenue by    │ Scatter: Product Revenue vs Gross Margin %      │
│ Category → Segment           │ X: [Revenue], Y: [GM%], Size: [Units Sold]      │
├──────────────────────────────┴───────────────────────────────────────────────┤
│ Power BI Matrix: Category > Segment > Product | Revenue | Units | ASP | GM%    │
│ Sort default: Revenue descending; Top N control default 20                    │
└──────────────────────────────────────────────────────────────────────────────┘
```

## Page 3 — Marketing Channel & Device Performance

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Marketing Channel & Device Performance                                        │
│ Slicers: Year | Traffic Channel | Device | Category | Region                  │
├──────────────┬──────────────┬──────────────┬──────────────┬──────────────────┤
│ KPI Revenue  │ KPI Customers│ Digital Rev% │ KPI ASP      │ KPI GM %          │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ 100% stacked column: Revenue mix by Month     │ Bar: Revenue by Channel/Device│
│ Metric: [Revenue]; Legend: Traffic Channel    │ Axis: Channel, Device         │
├──────────────────────────────────────────────┴───────────────────────────────┤
│ Power BI Matrix: Traffic Channel > Device | Revenue | Units | Customers | Share%│
└──────────────────────────────────────────────────────────────────────────────┘
```

## Page 4 — Geo Performance

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Geo Performance                                                               │
│ Slicers: Year | Region | State | Category | Traffic Channel                   │
├──────────────┬──────────────┬──────────────┬──────────────┬──────────────────┤
│ KPI Revenue  │ KPI GP       │ KPI GM %     │ KPI Customers│ KPI YoY %         │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ Map/Filled map: Revenue by State/City         │ Bar: Revenue by Region/State  │
│ Metric: [Revenue]                             │ Metric: [Revenue]             │
├──────────────────────────────────────────────┴───────────────────────────────┤
│ Power BI Matrix: Region → State → District | Revenue | Units | GM% | YoY%      │
└──────────────────────────────────────────────────────────────────────────────┘
```

## Page 5 — Budget & Forecast

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Budget & Forecast                                                             │
│ Slicers: Year | Month | Category | Segment                                     │
├──────────────┬──────────────┬──────────────┬──────────────┬──────────────────┤
│ KPI Revenue  │ KPI Budget   │ Attainment % │ Variance $   │ Variance %        │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ Combo: Actual Revenue vs Budget by Month      │ Bar: Budget Variance by       │
│ Metrics: [Revenue], [Budget]                  │ Category-Segment              │
├──────────────────────────────────────────────┴───────────────────────────────┤
│ Power BI Matrix: Category → Segment × Month | Revenue | Budget | Var $ | Var % │
├──────────────────────────────────────────────────────────────────────────────┤
│ Separate plan-only section: 2021 Budget vs 2021 Forecast by Month             │
└──────────────────────────────────────────────────────────────────────────────┘
```

## SVG overview wireframe

See `docs/assets/wireframe-overview.svg` for the primary multi-page visual mockup. It covers all 5 planned pages and labels the metric, axis/dimension, legend, and filter intent for each visual.

## Visual annotation checklist for DA

| Visual type | Minimum required fields |
|---|---|
| KPI card | Measure, comparison indicator, selected period context. |
| Line chart | Date axis sorted by MonthID; measure [Revenue]; optional prior year. |
| Category bar | Product Category/Segment axis; [Revenue], [Gross Profit], or [GM%]. |
| Channel mix | Campaign Traffic Channel and Device; [Revenue] and [Revenue Share %]. |
| Geo visual | Geo Region/State/District; [Revenue], [Customers], [GM%]. |
| Budget combo | Date Month axis; [Revenue] and [Budget]; filter to overlap periods. |
| Power BI Matrix | Rows/columns clearly defined; show hierarchy indentation, expand/collapse markers, subtotal/grand total rows, and conditional formatting on variance/margin. |
