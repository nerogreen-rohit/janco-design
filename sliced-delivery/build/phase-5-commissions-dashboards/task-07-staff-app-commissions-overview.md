# Task P5-07: Commissions Overview Screen

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a staff member, I want a Commissions Overview screen showing all commission records across all cases so that I can filter, search, and export commission data  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Commissions list (P5-01), Staff App (P1-11)

---

## Step-by-Step Guide

### Step 1 — Create the Commissions Overview Screen

1. Open the **Specialist Lending Staff App** in Power Apps Studio
2. Add a new screen: **+ New screen** → **Blank**
3. Name: `scrCommissionsOverview`
4. Add a header label:
   - Text: `"Commissions Overview"`
   - Font size: 24, Bold

### Step 2 — Add Filter Controls

Add a row of filter controls at the top of the screen:

| Control | Name | Type | Items / Options |
|---|---|---|---|
| Commission Type | ddFilterCommType | Dropdown | `["All", "Proc Fee", "Advisory Fee (AdvFee)", "Introducer Fee", "Referral Fee"]` |
| Product Type | ddFilterProductType | Dropdown | `["All", "BTL", "Bridging", "Commercial", "Dev Finance", "Refurb", "Auction"]` |
| Tax Year | ddFilterTaxYear | Dropdown | `["All", "2024/25", "2025/26", "2026/27"]` |
| Received Status | ddFilterReceived | Dropdown | `["All", "Received", "Pending"]` |
| Case Owner | cmbFilterOwner | Combo Box | Distinct case owners or Office365Users |

### Step 3 — Build the Filtered Gallery

1. Add a **Gallery** control
2. Name: `galCommissionsOverview`
3. Items formula:

```
Sort(
    Filter(
        Commissions,
        (ddFilterCommType.Selected.Value = "All" || CommissionType.Value = ddFilterCommType.Selected.Value) &&
        (ddFilterProductType.Selected.Value = "All" || ProductType = ddFilterProductType.Selected.Value) &&
        (ddFilterTaxYear.Selected.Value = "All" || TaxYear = ddFilterTaxYear.Selected.Value) &&
        (ddFilterReceived.Selected.Value = "All" ||
            (ddFilterReceived.Selected.Value = "Received" && IsReceived = true) ||
            (ddFilterReceived.Selected.Value = "Pending" && IsReceived = false)
        )
    ),
    Created,
    SortOrder.Descending
)
```

4. Gallery template columns:

| Column | Control | Value |
|---|---|---|
| Case ID | Label | `ThisItem.CaseID` |
| Case Name | Label | `ThisItem.CaseName` |
| Commission Type | Label | `ThisItem.CommissionType.Value` |
| Product | Label | `ThisItem.ProductType` |
| Expected | Label | `Text(ThisItem.ExpectedAmount, "£#,##0.00")` |
| Actual | Label | `Text(ThisItem.ActualAmount, "£#,##0.00")` |
| Status | Label | `If(ThisItem.IsReceived, "✅ Received", "⏳ Pending")` |
| Tax Year | Label | `ThisItem.TaxYear` |

### Step 4 — Add Summary Row

Below the gallery, add summary labels:

```
Total Records:    CountRows(galCommissionsOverview.AllItems)
Total Expected:   Text(Sum(galCommissionsOverview.AllItems, ExpectedAmount), "£#,##0.00")
Total Received:   Text(Sum(Filter(galCommissionsOverview.AllItems, IsReceived), ActualAmount), "£#,##0.00")
Outstanding:      Text(Sum(Filter(galCommissionsOverview.AllItems, !IsReceived), ExpectedAmount), "£#,##0.00")
```

### Step 5 — Add Export Capability

1. Add an **Export** button:
   - Text: `"📥 Export to Excel"`
   - OnSelect:

```
Collect(
    colExportCommissions,
    galCommissionsOverview.AllItems
);
// Use a Power Automate flow to export, OR
// Navigate to the SharePoint list view for native Excel export
Launch(
    "https://[tenant].sharepoint.com/sites/lending/Lists/Commissions/AllItems.aspx"
)
```

> **Alternative:** Use Power Automate to create an Excel file in OneDrive and send the link to the user.

### Step 6 — Add Navigation to Case

1. On each gallery row, add an **icon** or make the row tappable:
   - OnSelect: `Set(varCurrentCaseID, ThisItem.CaseID); Navigate(scrCaseWorkspace)`
   - This navigates to the Case Workspace for the selected commission's case

### Step 7 — Add to App Navigation

1. Add "Commissions" to the app's left navigation menu or top nav
2. OnSelect: `Navigate(scrCommissionsOverview)`

### Step 8 — Wireframe Reference

```
╔══════════════════════════════════════════════════════════════════════════╗
║  COMMISSIONS OVERVIEW                                    [📥 Export]    ║
╠══════════════════════════════════════════════════════════════════════════╣
║                                                                          ║
║  Type: [All ▼]  Product: [All ▼]  Tax Year: [All ▼]  Status: [All ▼]   ║
║                                                                          ║
║  ┌────────┬──────────────┬─────────────┬─────────┬──────────┬────────┐  ║
║  │ CaseID │ Name         │ Type        │ Expected│ Actual   │ Status │  ║
║  ├────────┼──────────────┼─────────────┼─────────┼──────────┼────────┤  ║
║  │ BTL-01 │ John Doe     │ Proc Fee    │ £2,137  │ £2,137   │ ✅     │  ║
║  │ BTL-01 │ John Doe     │ Adv Fee     │ £1,500  │ —        │ ⏳     │  ║
║  │ BRG-03 │ Jane Smith   │ Proc Fee    │ £3,200  │ £3,200   │ ✅     │  ║
║  │ BRG-03 │ Jane Smith   │ Referral Fee│ £500    │ —        │ ⏳     │  ║
║  └────────┴──────────────┴─────────────┴─────────┴──────────┴────────┘  ║
║                                                                          ║
║  Records: 4   Expected: £7,337   Received: £5,337   Outstanding: £2,000 ║
╚══════════════════════════════════════════════════════════════════════════╝
```

### Step 9 — Verify

- [ ] Commissions Overview screen is accessible from the app navigation
- [ ] All commission records display across all cases
- [ ] Commission Type filter works correctly (including "Advisory Fee (AdvFee)")
- [ ] Product Type filter works correctly
- [ ] Tax Year filter works correctly
- [ ] Received Status filter (All/Received/Pending) works correctly
- [ ] Summary row shows accurate totals matching the filtered data
- [ ] Tapping a row navigates to the correct Case Workspace
- [ ] Export button opens the SharePoint list or triggers export flow

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Gallery shows no records | Verify the data source connection to the Commissions list; check all filter defaults are "All" |
| Filters not working | Ensure dropdown values match the exact choice values in the list (e.g. "Advisory Fee (AdvFee)" not "Advisory Fee") |
| Summary totals incorrect | Use `galCommissionsOverview.AllItems` for the filtered subset, not the raw Commissions data source |
| Delegation warning on Filter | SharePoint delegation supports `=` comparisons on Choice columns; keep data under 2000 items or use indexed columns |
| Export not working | Ensure the SharePoint URL is correct; alternatively use `Collect` + Power Automate for Excel export |
