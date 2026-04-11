# Task P5-09: Dashboard — Performance Dashboard

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a manager, I want a Performance Dashboard showing conversion rates and SLA performance so that I can track team efficiency  
**Priority:** 🟡 High  
**Estimated effort:** 3–4 hours  
**Depends on:** Cases list (P1-01), Leads list (P2-03), Case Stage History (P3-02), Dashboard Screen Layout (P5-12)

---

## Step-by-Step Guide

### Step 1 — Create the Performance Dashboard Container

1. Open the **Specialist Lending Staff App** in Power Apps Studio
2. Navigate to the **Dashboard** screen (created in P5-12)
3. Add a container visible when `varDashboardTab = "Performance"`
4. Name: `conPerformanceDashboard`

### Step 2 — Load Data Collections

Add to the Dashboard screen's `OnVisible` (or use a refresh button):

```
// Active + completed cases
ClearCollect(
    colAllCases,
    'Specialist Lending Cases'
);

// Leads
ClearCollect(
    colAllLeads,
    Leads
);

// Stage History
ClearCollect(
    colStageHistory,
    'Case Stage History'
)
```

### Step 3 — Metric Card: Cases by Case Owner

1. Create a collection grouped by owner:

```
ClearCollect(
    colCasesByOwner,
    AddColumns(
        GroupBy(
            Filter(colAllCases, CaseStatus.Value = "Active"),
            "CaseOwner",
            "OwnerCases"
        ),
        "ActiveCount", CountRows(OwnerCases)
    )
)
```

2. Add a **Gallery** control:
   - Name: `galCasesByOwner`
   - Items: `Sort(colCasesByOwner, ActiveCount, SortOrder.Descending)`
   - Template:
     - Label: `ThisItem.CaseOwner.DisplayName`
     - Count: `ThisItem.ActiveCount`

### Step 4 — Metric Card: Conversion Rates

#### Lead → Case Conversion Rate

```
Set(
    varLeadToCaseRate,
    If(
        CountRows(colAllLeads) > 0,
        RoundUp(
            CountRows(Filter(colAllLeads, !IsBlank(ConvertedCaseID))) /
            CountRows(colAllLeads) * 100,
            1
        ),
        0
    )
)
```

Display: `varLeadToCaseRate & "%"`

#### Case → Completion Conversion Rate

```
Set(
    varCaseCompletionRate,
    If(
        CountRows(colAllCases) > 0,
        RoundUp(
            CountRows(Filter(colAllCases, CaseStatus.Value = "Completed")) /
            CountRows(colAllCases) * 100,
            1
        ),
        0
    )
)
```

Display: `varCaseCompletionRate & "%"`

### Step 5 — Metric: Average Stage Durations

Calculate average time spent at each stage from `TimeInPreviousStageHours`:

```
ClearCollect(
    colAvgStageDuration,
    AddColumns(
        GroupBy(colStageHistory, "StageName", "StageEntries"),
        "AvgHours", Average(StageEntries, TimeInPreviousStageHours),
        "AvgDays", RoundUp(Average(StageEntries, TimeInPreviousStageHours) / 24, 1)
    )
)
```

Add a **Gallery**:
- Items: `Sort(colAvgStageDuration, AvgHours, SortOrder.Ascending)`
- Template:
  - Label: `ThisItem.StageName`
  - Duration: `ThisItem.AvgDays & " days"`

### Step 6 — SLA Performance: On Track / At Risk / Overdue

Determine SLA status based on `TimeInPreviousStageHours` vs expected durations:

```
Set(varOnTrack,   CountRows(Filter(colAllCases, CaseStatus.Value = "Active" && SLAStatus.Value = "On Track")));
Set(varAtRisk,    CountRows(Filter(colAllCases, CaseStatus.Value = "Active" && SLAStatus.Value = "At Risk")));
Set(varOverdue,   CountRows(Filter(colAllCases, CaseStatus.Value = "Active" && SLAStatus.Value = "Overdue")))
```

> **Note:** If `SLAStatus` is not a column on Cases, calculate it in the collection:
> Compare `DateDiff(StageChangedDate, Today(), TimeUnit.Hours)` against Stage Configuration `ExpectedDurationHours`.

Display as three metric cards:

| Card | Colour | Value |
|---|---|---|
| ✅ On Track | Green | `varOnTrack` |
| ⚠️ At Risk | Amber | `varAtRisk` |
| 🔴 Overdue | Red | `varOverdue` |

### Step 7 — Wireframe Reference

```
╔══════════════════════════════════════════════════════════════════════╗
║  DASHBOARDS                                                          ║
║  [Pipeline] [Performance] [Commission] [Compliance]                   ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                       ║
║  ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐     ║
║  │ LEAD → CASE      │ │ CASE → COMPLETE  │ │ AVG COMPLETION   │     ║
║  │ 62%              │ │ 78%              │ │ 42 days          │     ║
║  └──────────────────┘ └──────────────────┘ └──────────────────┘     ║
║                                                                       ║
║  TEAM PERFORMANCE                          SLA STATUS                ║
║  ┌──────────────────────────────┐         ┌──────────────────┐      ║
║  │ Sarah Johnson    ▎ 12 cases  │         │ ✅ On Track  32  │      ║
║  │ Mark Williams    ▎ 9 cases   │         │ ⚠️ At Risk    8  │      ║
║  │ Emma Thompson    ▎ 7 cases   │         │ 🔴 Overdue    3  │      ║
║  └──────────────────────────────┘         └──────────────────┘      ║
║                                                                       ║
║  AVERAGE STAGE DURATION                                               ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │ 1. Initial Enquiry     1.2 days                              │   ║
║  │ 2. Fact-Find           3.5 days                              │   ║
║  │ 3. TOB / Compliance    2.1 days                              │   ║
║  │ 4. DIP Submission      5.8 days                              │   ║
║  │ 5. Full Application    7.2 days                              │   ║
║  │ 6. Offer               3.4 days                              │   ║
║  │ 7. Legal              14.6 days                              │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

### Step 8 — Verify

- [ ] Performance Dashboard shows conversion rate for Lead → Case
- [ ] Performance Dashboard shows conversion rate for Case → Completion
- [ ] Cases by CaseOwner displays correct counts for active cases
- [ ] Average stage durations calculate correctly from `TimeInPreviousStageHours`
- [ ] SLA status cards show On Track / At Risk / Overdue counts
- [ ] All metrics update on screen refresh
- [ ] Data matches underlying list counts when spot-checked

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Conversion rates show 0% | Verify Leads list has records; check `ConvertedCaseID` is populated when a lead becomes a case |
| Stage durations missing | Ensure Case Stage History list has records with `TimeInPreviousStageHours` populated (Phase 3 flow) |
| SLA status all "On Track" | Check the SLA calculation logic; verify `StageChangedDate` is being set on stage transitions |
| Team list shows duplicates | Use `GroupBy` on CaseOwner — ensure person field is resolved consistently |
| Slow loading | Reduce collections to Active cases only where possible; use `ShowColumns` to fetch fewer fields |
