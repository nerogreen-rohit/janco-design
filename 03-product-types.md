[← Back to Index](00-index.md)

# 3. Product Types Supported

| # | Product Type | Typical Term | Key Differentiator |
|---|---|---|---|
| 1 | **Buy to Let (BTL)** | 2–5 years | ICR stress test, rental income, EPC |
| 2 | **Bridging Finance** | 1–24 months | Speed, exit strategy, auction deadlines |
| 3 | **Commercial Mortgage** | 3–25 years | Business accounts, lease terms |
| 4 | **Development Finance** | 6–24 months | GDV, build cost, drawdown schedule |
| 5 | **Refurbishment Loan** | 6–18 months | Scope of works, schedule of works, GDV |
| 6 | **Auction Finance** | 28 days typical | Auction date, legal pack, speed |

## Product-Specific Fields on Case Record

| Field Group | Applies To | Key Fields |
|---|---|---|
| **BTL** | BTL | MonthlyRent, ICRMultiplier, CalculatedMaxLoan, EpcRating |
| **Bridge / Auction** | Bridging, Auction | AuctionDate, CompletionDeadline, WorksRequired, WorksBudget |
| **Development** | Dev Finance | BuildCost, ProfitOnGDV, EstimatedGDV |
| **Refurbishment** | Refurb | WorksRequired, WorksBudget, EstimatedGDV |

The internal app displays product-specific sections **conditionally** based on the case's ProductType.
