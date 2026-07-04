# Business glossary — VanArsdel

Shared business definitions for the VanArsdel Sales Performance report, written the way business users understand them. Field-level column detail lives in [dictionary/data-dictionary.md](data-dictionary.md); open questions and assumptions live in [docs/data-gaps-and-stakeholder-questions.md](../docs/data-gaps-and-stakeholder-questions.md).

## Products & portfolio

| Term | Definition |
|------|------------|
| **VanArsdel** | The retailer whose sales this dashboard tracks. Every product in the data is a VanArsdel product. |
| **Manufacturer** | The company that makes a product. All products here are VanArsdel, so there is no competitor comparison. |
| **Product** | A single sellable item (SKU) - the lowest level of the product hierarchy. |
| **Category** | Top level of the product hierarchy, used for executive portfolio views: Accessory, Mix, Rural, Urban, Youth. |
| **Segment** | The level below Category, used for decisions within a category (e.g. Urban = Convenience, Extreme, Moderation, Regular). |
| **Category-Segment** | The combined Category + Segment level at which budgets and forecasts are planned. |

## Marketing & channels

| Term | Definition |
|------|------------|
| **Campaign** | A traffic-source setup pairing a marketing channel with a device - not a named promotion. |
| **Traffic Channel** | The marketing route a sale is attributed to: Organic Search, SEO, Banner, Affiliate, SEM, Email, SMO, Mail. |
| **Device** | The device or medium of the attributed campaign: Desktop, Mobile, Tablet, Paper. |
| **Digital channel** | Any traffic channel except direct Mail. |
| **Offline channel** | Direct Mail, delivered on Paper. |

## Customers & geography

| Term | Definition |
|------|------------|
| **Customer** | A customer account tied to sales lines. Distinct customers are counted by customer ID. |
| **Region** | Grouping of US geography for regional analysis: East, Central, West. |
| **District / State / ZIP** | Progressively finer levels of customer geography under Region. |

## Money & performance

| Term | Definition |
|------|------------|
| **Units** | Quantity sold on a sales line. |
| **Unit Price** | Selling price per unit - the basis for Revenue. |
| **Unit Cost** | Cost per unit - the basis for COGS. |
| **Revenue** | Value of actual sales = Units x Unit Price. There is no stored revenue column; it is calculated. |
| **COGS** | Cost of goods sold = Units x Unit Cost. |
| **Gross Profit** | Revenue - COGS. |
| **Gross Margin %** | Gross Profit / Revenue. |
| **Currency** | Monetary amounts default to USD unless confirmed otherwise (see data gaps). |

## Planning (budget & forecast)

| Term | Definition |
|------|------------|
| **Budget** | Approved revenue target for a month and Category-Segment (covers 2020-2021). |
| **Forecast** | Latest expected revenue for a period (covers 2021). |
| **Plan** | Umbrella term for Budget and Forecast values once combined into one table. |
| **Budget Attainment** | Actual Revenue / Budget, for periods where both exist. |
| **Variance** | Actual Revenue - Plan, shown as a value and/or percentage. |

## Time & structure

| Term | Definition |
|------|------------|
| **MoM / YoY** | Change versus the previous month / the same month last year. |
| **YTD** | Cumulative from Jan 1 to the current period. |
| **Grain** | The level of detail of one fact row (e.g. a sales line = product x date x customer x campaign). |
| **Actual-vs-Budget grain** | Actual and Budget are comparable only at Month x Category-Segment or higher. |
