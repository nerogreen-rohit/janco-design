# Task P5-10: Dashboard — Commission Dashboard

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a manager, I want a Commission Dashboard showing income totals by type, product, and tax year so that I can monitor revenue and outstanding commissions  
**Priority:** 🟡 High  
**Estimated effort:** 2–3 hours  
**Depends on:** Commissions list (P5-01), Dashboard Screen Layout (P5-12)

---

## Step-by-Step Guide

### Step 1 — Create the Commission Dashboard Container

1. Open the **Specialist Lending Staff App** in Power Apps Studio
2. Navigate to the **Dashboard** screen (created in P5-12)
3. Add a container visible when `varDashboardTab = "Commission"`
4. Name: `conCommissionDashboard`

### Step 2 — Load Commission Data

Add to the Dashboard screen's `OnVisible`:

```
ClearCollect(
    colAllCommissions,
    Commissions
)
```

### Step 3 — Income Summary by Commission Type

Create a collection grouped by CommissionType:

```
ClearCollect(
    colIncomeByType,
    AddColumns(
        GroupBy(colAllCommissions, "CommissionType", "TypeGroup"),
        "ExpectedTotal", Sum(TypeGroup, ExpectedAmount),
        "ReceivedTotal", Sum(Filter(TypeGroup, IsReceived), ActualAmount),
        "ReceivedPercent",
            If(
                Sum(TypeGroup, ExpectedAmount) > 0,
                RoundUp(Sum(Filter(TypeGroup, IsReceived), ActualAmount) / Sum(TypeGroup, ExpectedAmount) * 100, 1),
                0
            )
    )
)
```

Display as a summary table:

| Type | Expected | Received | % |
|---|---|---|---|
| Proc Fee | £XX,XXX | £XX,XXX | XX% |
| Advisory Fee (AdvFee) | £XX,XXX | £XX,XXX | XX% |
| Referral Fee | £XX,XXX | £XX,XXX | XX% |

Add a **Gallery**:
- Name: `galIncomeByType`
- Items: `colIncomeByType`
- Template:
  - Type: `ThisItem.CommissionType.Value`
  - Expected: `Text(ThisItem.ExpectedTotal, "£#,##0")`
  - Received: `Text(ThisItem.ReceivedTotal, "£#,##0")`
  - Percent: `ThisItem.ReceivedPercent & "%"`

### Step 4 — Metric Cards: Overall Totals

| Card | Formula |
|---|---|
| Total Expected | `Text(Sum(colAllCommissions, ExpectedAmount), "£#,##0")` |
| Total Received | `Text(Sum(Filter(colAllCommissions, IsReceived), ActualAmount), "£#,##0")` |
| Outstanding | `Text(Sum(Filter(colAllCommissions, !IsReceived), ExpectedAmount), "£#,##0")` |

### Step 5 — Income by Product Type

```
ClearCollect(
    colIncomeByProduct,
    AddColumns(
        GroupBy(colAllCommissions, "ProductType", "ProductGroup"),
        "ExpectedTotal", Sum(ProductGroup, ExpectedAmount),
        "ReceivedTotal", Sum(Filter(ProductGroup, IsReceived), ActualAmount)
    )
)
```

Add a **Gallery** or bar chart:
- Items: `Sort(colIncomeByProduct, ExpectedTotal, SortOrder.Descending)`
- Template: Product name, expected bar, received bar

### Step 6 — Filter by Tax Year

1. Add a Tax Year dropdown at the top:
   - Name: `ddDashTaxYear`
   - Items: `["All", "2024/25", "2025/26", "2026/27"]`

2. Update `colAllCommissions` to respect the filter:

```
ClearCollect(
    colAllCommissions,
    If(
        ddDashTaxYear.Selected.Value = "All",
        Commissions,
        Filter(Commissions, TaxYear = ddDashTaxYear.Selected.Value)
    )
)
```

3. Recalculate all dependent collections on dropdown change (`OnChange`):

```
// Re-run the collection builds from Steps 3 and 5
```

### Step 7 — Outstanding Commissions List

Add a gallery showing all outstanding (unreceived) commissions:

1. Add a **Gallery**:
   - Name: `galOutstandingCommissions`
   - Items: `Filter(colAllCommissions, !IsReceived)`
   - Template:
     - CaseID | CaseName | CommissionType | ExpectedAmount | LenderName

2. Add a count label: `CountRows(Filter(colAllCommissions, !IsReceived)) & " outstanding"`

### Step 8 — Wireframe Reference

```
╔══════════════════════════════════════════════════════════════════════╗
║  DASHBOARDS                                                          ║
║  [Pipeline] [Performance] [Commission] [Compliance]                   ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                       ║
║  Tax Year: [2025/26 ▼]                                                ║
║                                                                       ║
║  ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐     ║
║  │ TOTAL EXPECTED   │ │ TOTAL RECEIVED   │ │ OUTSTANDING      │     ║
║  │ £87,450          │ │ £62,300          │ │ £25,150          │     ║
║  └──────────────────┘ └──────────────────┘ └──────────────────┘     ║
║                                                                       ║
║  INCOME BY TYPE                           INCOME BY PRODUCT          ║
║  ┌────────────────────────────────┐      ┌──────────────────────┐   ║
║  │ Type          │ Exp    │ Rec % │      │ BTL        █████ £42K│   ║
║  │ Proc Fee      │ £45.2K │  82%  │      │ Bridging   ████  £22K│   ║
║  │ Advisory Fee  │ £32.1K │  64%  │      │ Commercial ███   £14K│   ║
║  │ (AdvFee)      │        │       │      │ Dev Finance██    £6K │   ║
║  │ Referral Fee  │ £10.1K │  71%  │      │ Refurb     █     £3K │   ║
║  └────────────────────────────────┘      └──────────────────────┘   ║
║                                                                       ║
║  OUTSTANDING COMMISSIONS (8)                                          ║
║  ┌──────────┬──────────────┬─────────────┬───────────┐              ║
║  │ CaseID   │ Name         │ Type        │ Expected  │              ║
║  │ BTL-0004 │ A. Davies    │ Adv Fee     │ £1,500    │              ║
║  │ BRG-0012 │ P. Wilson    │ Proc Fee    │ £3,200    │              ║
║  └──────────┴──────────────┴─────────────┴───────────┘              ║
╚══════════════════════════════════════════════════════════════════════╝
```

### Step 9 — Verify

- [ ] Commission Dashboard loads and displays data from the Commissions list
- [ ] Income by type shows correct totals for Proc Fee, Advisory Fee (AdvFee), and Referral Fee
- [ ] "Advisory Fee (AdvFee)" label displays correctly — never abbreviated differently
- [ ] Expected vs Received totals are accurate
- [ ] Received percentage calculates correctly
- [ ] Tax Year filter updates all metrics when changed
- [ ] Income by Product shows correct breakdown
- [ ] Outstanding commissions list shows only unreceived records
- [ ] All totals match the underlying Commissions list data

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Commission types missing from summary | Verify Commissions list has records for each type; check GroupBy field name matches the internal column name |
| Percentages show NaN or errors | Add a zero-check before dividing (see Step 3 formula) |
| Tax year filter not updating | Ensure the dropdown's `OnChange` property re-runs the `ClearCollect` statements |
| Advisory Fee label incorrect | The CommissionType choice value must be exactly "Advisory Fee (AdvFee)" — check list settings |
| Outstanding count doesn't match totals | Verify the `!IsReceived` filter is correct; check for null vs false values on IsReceived |
