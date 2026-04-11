# Task P3-15: Staff App — Case List Screen

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a staff member, I want a filterable case list screen so that I can quickly find and open any case  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Cases list (P1-01), Stage Configuration list with all 14 stages (P3-07), Staff App created (P1-11)

---

## Step-by-Step Guide

### Step 1 — Create the Case List Screen

1. Open the Staff App in **Power Apps** → Edit mode
2. Add a new screen: **Case List Screen**
3. Set as the app's default start screen

### Step 2 — Add Filter Controls

Add filter dropdowns across the top of the screen:

| Control | Type | Items Source | Default |
|---|---|---|---|
| ddFilterStage | Dropdown | `Distinct('Stage Configuration', Title)` | `"All"` |
| ddFilterProduct | Dropdown | `["All", "BTL", "Bridging", "Commercial", "Dev Finance", "Refurb", "Auction"]` | `"All"` |
| ddFilterOwner | Combo Box | `Office365Users.SearchUser({searchTerm: ""})` | — |
| ddFilterStatus | Dropdown | `["All", "Active", "On Hold", "Withdrawn", "Declined", "Completed"]` | `"All"` |
| txtSearch | Text Input | — | Placeholder: `"Search by CaseID or applicant name..."` |

### Step 3 — Screen Layout

```
╔══════════════════════════════════════════════════════════════════════════╗
║  CASE LIST                                           [+ New Case]       ║
╠══════════════════════════════════════════════════════════════════════════╣
║  Stage: [All ▼]  Product: [All ▼]  Owner: [All ▼]  Status: [All ▼]    ║
║  Search: [______________________________________]                       ║
╠══════════════════════════════════════════════════════════════════════════╣
║  CaseID       │ Applicant        │ Product  │ Stage        │ SLA  │ Owner║
║  ─────────────┼──────────────────┼──────────┼──────────────┼──────┼──── ║
║  BTL-2026-001 │ John Doe         │ BTL      │ Fact-Find    │ 🟢   │ AB  ║
║  BRI-2026-001 │ Jane Smith       │ Bridging │ Valuation    │ 🔴   │ CD  ║
║  COM-2026-001 │ Acme Ltd         │ Commercial│ DIP         │ 🟡   │ AB  ║
║  DEV-2026-001 │ Build Co         │ Dev Fin  │ Legals       │ 🟢   │ EF  ║
║  BTL-2026-002 │ Bob Wilson       │ BTL      │ Loan Offer   │ 🟢   │ CD  ║
║               │                  │          │              │      │     ║
╚══════════════════════════════════════════════════════════════════════════╝
```

### Step 4 — Configure the Gallery

1. Add a **Gallery** control (vertical, blank template)
2. Set the **Items** property with filtering:

```
Sort(
    Filter(
        'Specialist Lending Cases',
        // Stage filter
        (ddFilterStage.Selected.Value = "All" ||
         CurrentStageName = ddFilterStage.Selected.Value) &&
        // Product filter
        (ddFilterProduct.Selected.Value = "All" ||
         ProductType.Value = ddFilterProduct.Selected.Value) &&
        // Status filter
        (ddFilterStatus.Selected.Value = "All" ||
         CaseStatus.Value = ddFilterStatus.Selected.Value) &&
        // Search filter
        (IsBlank(txtSearch.Text) ||
         StartsWith(CaseID, txtSearch.Text) ||
         StartsWith(Applicant1LastName, txtSearch.Text) ||
         StartsWith(Applicant1FirstName, txtSearch.Text))
    ),
    EnquiryDate,
    SortOrder.Descending
)
```

> **Note:** If the owner filter is used, add: `(IsBlank(ddFilterOwner.Selected) || CaseOwner.Email = ddFilterOwner.Selected.Mail)`

### Step 5 — Gallery Row Template

Add labels inside the gallery for each column:

| Label | Text Property | Width |
|---|---|---|
| CaseID | `ThisItem.CaseID` | 120px |
| Applicant | `ThisItem.Applicant1FirstName & " " & ThisItem.Applicant1LastName` | 180px |
| Product | `ThisItem.ProductType.Value` | 100px |
| Stage | `ThisItem.CurrentStageName` | 140px |
| SLA Badge | (see Step 6) | 50px |
| Owner | `ThisItem.CaseOwner.DisplayName` | 100px |

### Step 6 — SLA Badge Logic

Add a circle or icon for the SLA status:

```
// SLA Badge Color
If(
    IsBlank(ThisItem.StageChangedDate),
    Color.Gray,
    // Get SLA days for current stage
    With(
        {
            slaDays: LookUp(
                'Stage Configuration',
                StageNumber = ThisItem.CurrentStageNumber,
                DefaultSLADays
            ),
            daysSinceChange: DateDiff(
                ThisItem.StageChangedDate,
                Today(),
                TimeUnit.Days
            )
        },
        If(
            daysSinceChange > slaDays,
            Color.Red,          // 🔴 Overdue
            daysSinceChange >= slaDays - 1,
            Color.Orange,       // 🟡 At risk
            Color.Green         // 🟢 On track
        )
    )
)
```

### Step 7 — Row Navigation

On the gallery's **OnSelect** property:

```
Set(varCurrentCase, ThisItem);
Navigate(CaseWorkspaceScreen)
```

### Step 8 — New Case Button

Add a **[+ New Case]** button in the header:

```
// OnSelect
Navigate(CaseCreationScreen)
```

### Step 9 — Test

1. Open the Staff App
2. Verify the case list loads with all active cases
3. Test each filter:
   - [ ] Stage filter narrows results to selected stage
   - [ ] Product filter narrows results to selected product type
   - [ ] Status filter works (Active, On Hold, etc.)
   - [ ] Search by CaseID returns matching cases
   - [ ] Search by applicant last name works
4. Verify SLA badges:
   - [ ] 🟢 Green for cases within SLA
   - [ ] 🟡 Orange for at-risk cases (within 1 day of SLA)
   - [ ] 🔴 Red for overdue cases
5. Click a case row:
   - [ ] Navigates to Case Workspace with correct case loaded

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Gallery shows no results | Check filter logic — ensure "All" default values don't exclude records |
| SLA badge always gray | Verify StageChangedDate is populated on case records (set by Flow 6) |
| Search is slow | Use `StartsWith` instead of `in` or `Search` for delegation-safe filtering |
| Owner filter not matching | Ensure the Person field comparison uses `.Email` property for matching |
| Too many cases loading | Add pagination or set gallery to load 50 items at a time |
