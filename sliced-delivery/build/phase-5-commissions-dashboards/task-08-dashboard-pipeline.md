# Task P5-08: Dashboard — Pipeline Dashboard

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a manager, I want a Pipeline Dashboard showing live case counts by stage and product type so that I can see the current pipeline at a glance  
**Priority:** 🟡 High  
**Estimated effort:** 2–3 hours  
**Depends on:** Cases list (P1-01), Stage Configuration list (P1-02), Dashboard Screen Layout (P5-12)

---

## Step-by-Step Guide

### Step 1 — Create the Pipeline Dashboard Container

1. Open the **Specialist Lending Staff App** in Power Apps Studio
2. Navigate to the **Dashboard** screen (created in P5-12) or create a new screen
3. Add a container or group visible when `varDashboardTab = "Pipeline"`
4. Name: `conPipelineDashboard`

### Step 2 — Load Active Cases Data

Create a collection on screen load (in the Dashboard screen's `OnVisible`):

```
ClearCollect(
    colActiveCases,
    Filter(
        'Specialist Lending Cases',
        CaseStatus.Value = "Active"
    )
)
```

### Step 3 — Metric Card: Total Pipeline Value

1. Add a card-style container:
   - Header label: `"Total Pipeline Value"`
   - Value label:

```
Text(
    Sum(colActiveCases, LoanAmountRequired),
    "£#,##0"
)
```

### Step 4 — Metric Card: Active Case Count

1. Add a card-style container:
   - Header label: `"Active Cases"`
   - Value label: `CountRows(colActiveCases)`

### Step 5 — Metric Card: Average Case Size

1. Add a card-style container:
   - Header label: `"Average Case Size"`
   - Value label:

```
Text(
    Average(colActiveCases, LoanAmountRequired),
    "£#,##0"
)
```

### Step 6 — Bar Chart: Cases by Stage

Create a gallery-based horizontal bar chart showing case count per stage:

1. Create a collection for stage counts:

```
ClearCollect(
    colPipelineByStage,
    AddColumns(
        GroupBy(colActiveCases, "CurrentStageName", "StageGroup"),
        "CaseCount", CountRows(StageGroup)
    )
)
```

2. Add a **Gallery** control:
   - Name: `galPipelineByStage`
   - Items: `Sort(colPipelineByStage, CaseCount, SortOrder.Descending)`
   - Template:
     - Label (left): `ThisItem.CurrentStageName`
     - Rectangle (bar): Width = `ThisItem.CaseCount / Max(colPipelineByStage, CaseCount) * MaxBarWidth`
     - Label (right): `ThisItem.CaseCount`

> **Alternative:** If using Power Apps charts component, add a **BarChart** control with Items = `colPipelineByStage`, Labels = `CurrentStageName`, Values = `CaseCount`.

### Step 7 — Count by Product Type

1. Create a collection:

```
ClearCollect(
    colPipelineByProduct,
    AddColumns(
        GroupBy(colActiveCases, "ProductType", "ProductGroup"),
        "CaseCount", CountRows(ProductGroup),
        "TotalValue", Sum(ProductGroup, LoanAmountRequired)
    )
)
```

2. Add a **Gallery** control:
   - Name: `galPipelineByProduct`
   - Items: `colPipelineByProduct`
   - Template:
     - Label: `ThisItem.ProductType.Value`
     - Count label: `ThisItem.CaseCount`
     - Value label: `Text(ThisItem.TotalValue, "£#,##0")`

### Step 8 — Wireframe Reference

```
╔══════════════════════════════════════════════════════════════════════╗
║  DASHBOARDS                                                          ║
║  [Pipeline] [Performance] [Commission] [Compliance]                   ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                       ║
║  ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐     ║
║  │ PIPELINE VALUE   │ │ ACTIVE CASES     │ │ AVG CASE SIZE    │     ║
║  │ £12,450,000      │ │ 47               │ │ £264,893         │     ║
║  └──────────────────┘ └──────────────────┘ └──────────────────┘     ║
║                                                                       ║
║  CASES BY STAGE                                                       ║
║  ┌────────────────────────────────────────────────────────────┐      ║
║  │ 1. Initial Enquiry    ████████████████████████  12         │      ║
║  │ 2. Fact-Find          ██████████████████  9                │      ║
║  │ 3. TOB / Compliance   ████████████  6                     │      ║
║  │ 4. DIP Submission     ██████████████████  9                │      ║
║  │ 5. Full Application   ████████  4                         │      ║
║  │ 6. Offer              ██████  3                           │      ║
║  │ 7. Legal              ████████  4                         │      ║
║  └────────────────────────────────────────────────────────────┘      ║
║                                                                       ║
║  BY PRODUCT TYPE                                                      ║
║  BTL: 22 (£5.8M)  |  Bridging: 10 (£3.2M)  |  Commercial: 8 (£2.1M)║
║  Dev Finance: 4 (£0.9M)  |  Refurb: 2 (£0.3M)  |  Auction: 1 (£0.2M)║
╚══════════════════════════════════════════════════════════════════════╝
```

### Step 9 — Verify

- [ ] Pipeline Dashboard shows data only for Active cases (CaseStatus = "Active")
- [ ] Total Pipeline Value displays correct sum of LoanAmountRequired
- [ ] Active Cases count matches the Cases list filtered to Active
- [ ] Average Case Size = Total Pipeline Value ÷ Active Cases
- [ ] Bar chart shows correct case counts per stage
- [ ] Product type breakdown shows correct counts and values
- [ ] Data refreshes when navigating to the Dashboard tab

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Metrics show 0 or blank | Verify `colActiveCases` is being populated on `OnVisible`; check CaseStatus filter value matches exactly |
| Stage names missing | Ensure `CurrentStageName` is populated on Case records (set by stage advancement flows in Phase 3) |
| Bar chart not rendering | Check gallery Items property references the correct collection; verify MaxBarWidth is set |
| Delegation warnings | Filtering by CaseStatus Choice column is delegable in SharePoint; keep within 2000-item limit or index the column |
| Slow performance | Reduce data loaded — use `ShowColumns` to fetch only needed columns; limit to last 12 months if necessary |
