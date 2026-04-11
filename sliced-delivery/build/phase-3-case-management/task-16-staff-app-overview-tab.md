# Task P3-16: Staff App — Case Workspace Overview Tab

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a staff member, I want a Case Workspace Overview tab with a 14-stage progress bar so that I can see case status and advance stages  
**Priority:** 🔴 Critical  
**Estimated effort:** 3–4 hours  
**Depends on:** Cases list (P1-01), Stage Configuration list with all 14 stages (P3-07), Flow 6 — Log Stage Change (P3-08), Case List screen (P3-15)

---

## Step-by-Step Guide

### Step 1 — Create the Case Workspace Screen

1. In the Staff App, add a new screen: **Case Workspace Screen**
2. This screen receives `varCurrentCase` set by the Case List (P3-15)
3. Add tab navigation at the top:

```
╔══════════════════════════════════════════════════════════════════════════╗
║  ← Back    BTL-2026-0001 — John Doe — Buy-to-Let                      ║
╠══════════════════════════════════════════════════════════════════════════╣
║  [Overview]  [Fact-Find]  [Documents]  [Messages]  [Timeline]          ║
╚══════════════════════════════════════════════════════════════════════════╝
```

### Step 2 — Case Header

Display key case information at the top:

```
╔══════════════════════════════════════════════════════════════════════════╗
║  BTL-2026-0001                          Status: ● Active               ║
║  John Doe & Jane Doe                    Owner: Alice Brown             ║
║  Buy-to-Let                             Lender: The Mortgage Lender    ║
║  123 High Street, London SW1A 1AA       Loan: £250,000  LTV: 75%      ║
╚══════════════════════════════════════════════════════════════════════════╝
```

| Element | Source |
|---|---|
| CaseID | `varCurrentCase.CaseID` |
| Applicant(s) | `varCurrentCase.Applicant1FirstName & " " & varCurrentCase.Applicant1LastName` |
| Product | `varCurrentCase.ProductType.Value` |
| Status | `varCurrentCase.CaseStatus.Value` |
| Owner | `varCurrentCase.CaseOwner.DisplayName` |
| Lender | `varCurrentCase.LenderName` |
| Loan Amount | `varCurrentCase.LoanAmountRequired` |
| LTV | `varCurrentCase.LTV` |
| Property | `varCurrentCase.PropertyAddress` |

### Step 3 — 14-Stage Progress Bar

Create a horizontal gallery showing all 14 stages:

```
╔══════════════════════════════════════════════════════════════════════════╗
║  1    2    3    4    5    6    7    8    9   10   11   12   13   14     ║
║  ✅───✅───✅───🔵───⬜───⬜───⬜───⬜───⬜───⬜───⬜───⬜───⬜───⬜     ║
║  ENQ  FACT TOB  PROD OPT  ILLUS DIP  APP  VAL  UW  OFFER LEGAL COMP POST║
╚══════════════════════════════════════════════════════════════════════════╝

✅ = Complete (green)    🔵 = Current (blue)    ⬜ = Future (gray)
```

1. Add a **Horizontal Gallery** control
2. Set **Items**: `Sort('Stage Configuration', StageNumber, SortOrder.Ascending)`
3. Inside each gallery item, add a circle icon and label:

**Circle Fill Color:**
```
If(
    ThisItem.StageNumber < varCurrentCase.CurrentStageNumber,
    RGBA(40, 167, 69, 1),      // Green — complete
    ThisItem.StageNumber = varCurrentCase.CurrentStageNumber,
    RGBA(0, 120, 212, 1),      // Blue — current
    RGBA(200, 200, 200, 1)     // Gray — future
)
```

**Stage Label:**
```
ThisItem.ShortName
```

**Connector Line between circles:**
Use a Rectangle control with dynamic fill matching the left circle's color.

### Step 4 — Current Stage Panel

Below the progress bar, show details of the current stage:

```
╔══════════════════════════════════════════════════════════════════════════╗
║  CURRENT STAGE: Product Research                                        ║
║  Entered: 15 Apr 2026                    SLA Due: 18 Apr 2026          ║
║  Days in stage: 2                        SLA Status: 🟢 On Track       ║
║                                                                         ║
║  Stage Notes: [_________________________________________________]      ║
║                                                                         ║
║  [Advance to Next Stage →]              [Mark On Hold]                  ║
╚══════════════════════════════════════════════════════════════════════════╝
```

| Element | Source / Formula |
|---|---|
| Stage name | `varCurrentCase.CurrentStageName` |
| Entered date | `varCurrentCase.StageChangedDate` |
| SLA Due | `DateAdd(varCurrentCase.StageChangedDate, LookUp('Stage Configuration', StageNumber = varCurrentCase.CurrentStageNumber, DefaultSLADays), TimeUnit.Days)` |
| Days in stage | `DateDiff(varCurrentCase.StageChangedDate, Today(), TimeUnit.Days)` |
| SLA Status | Same logic as SLA Badge (P3-15 Step 6) |

### Step 5 — Advance Stage Button

**[Advance to Next Stage →]** button `OnSelect`:

```
// Get next stage
Set(
    varNextStage,
    LookUp(
        'Stage Configuration',
        StageNumber = varCurrentCase.CurrentStageNumber + 1
    )
);

// Confirm with user
If(
    IsBlank(varNextStage),
    Notify("This is the final stage.", NotificationType.Warning),

    // Check if next stage can be skipped (show skip option)
    UpdateContext({showAdvanceDialog: true})
)
```

### Step 6 — Advance Stage Dialog

When `showAdvanceDialog` is true, show a dialog:

```
╔════════════════════════════════════════╗
║  Advance to: Present Options & Agree   ║
║                                        ║
║  Notes: [__________________________]   ║
║                                        ║
║  ☐ Skip this stage                     ║
║    (Only for stages 5, 6, 7)           ║
║    Reason: [________________________]  ║
║                                        ║
║  [Cancel]              [Confirm →]     ║
╚════════════════════════════════════════╝
```

**Skip checkbox visibility:**
```
varNextStage.CanSkip
```

**[Confirm →] OnSelect:**
```
// Update the Case record with new stage
Patch(
    'Specialist Lending Cases',
    varCurrentCase,
    {
        CurrentStageID: varNextStage,
        StageNotes: If(
            chkSkipStage.Value,
            "SKIPPED: " & txtSkipReason.Text,
            txtAdvanceNotes.Text
        )
    }
);

// Refresh case data
Set(
    varCurrentCase,
    LookUp('Specialist Lending Cases', ID = varCurrentCase.ID)
);

UpdateContext({showAdvanceDialog: false});
Notify(
    "Stage advanced to " & varNextStage.Title,
    NotificationType.Success
)
```

> **Note:** Flow 6 triggers automatically when the stage changes, creating the Stage History record and sending customer notifications.

### Step 7 — Mark On Hold Button

**[Mark On Hold]** button `OnSelect`:

```
Patch(
    'Specialist Lending Cases',
    varCurrentCase,
    {
        CaseStatus: {Value: "On Hold"}
    }
);
Set(
    varCurrentCase,
    LookUp('Specialist Lending Cases', ID = varCurrentCase.ID)
);
Notify("Case marked as On Hold.", NotificationType.Warning)
```

**Visibility:** Only show when `varCurrentCase.CaseStatus.Value = "Active"`

### Step 8 — Product-Specific Fields Panel

Below the stage panel, show fields relevant to the product type:

```
// Visible when ProductType = "BTL"
╔══════════════════════════════════════╗
║  BTL DETAILS                         ║
║  Monthly Rent:    £1,200             ║
║  ICR Multiplier:  1.25              ║
║  Max Loan:        £215,000          ║
║  EPC Rating:      C                 ║
╚══════════════════════════════════════╝

// Visible when ProductType = "Bridging" or "Auction"
╔══════════════════════════════════════╗
║  BRIDGING / AUCTION DETAILS          ║
║  Auction Date:    28 Apr 2026       ║
║  Completion Deadline: 26 May 2026   ║
║  Exit Strategy:   Refinance         ║
║  Works Required:  Yes               ║
║  Works Budget:    £45,000           ║
╚══════════════════════════════════════╝

// Visible when ProductType = "Dev Finance" or "Refurb"
╔══════════════════════════════════════╗
║  DEVELOPMENT / REFURB DETAILS        ║
║  Build Cost:      £180,000          ║
║  Estimated GDV:   £450,000          ║
║  Profit on GDV:   28%               ║
║  Works Required:  Yes               ║
║  Works Budget:    £180,000          ║
╚══════════════════════════════════════╝
```

**Panel Visibility formulas:**
```
// BTL panel
varCurrentCase.ProductType.Value = "BTL"

// Bridging/Auction panel
varCurrentCase.ProductType.Value in ["Bridging", "Auction"]

// Dev Finance/Refurb panel
varCurrentCase.ProductType.Value in ["Dev Finance", "Refurb"]
```

### Step 9 — Test

1. Open a test case from the Case List
2. Verify the Overview tab:
   - [ ] Case header shows correct case details
   - [ ] Progress bar displays all 14 stages with correct colors
   - [ ] Current stage is highlighted in blue
   - [ ] Completed stages show green
   - [ ] SLA information is correct
3. Test stage advancement:
   - [ ] [Advance to Next Stage →] opens the confirmation dialog
   - [ ] Notes can be entered
   - [ ] After confirming, the stage updates on screen
   - [ ] Progress bar updates to reflect the new current stage
   - [ ] Flow 6 creates a Stage History record
4. Test stage skipping:
   - [ ] Skip checkbox only appears for stages with CanSkip = Yes (5, 6, 7)
   - [ ] Skipping requires a reason
   - [ ] Skipped stage is logged in Stage History with IsSkipped = Yes
5. Test On Hold:
   - [ ] [Mark On Hold] changes CaseStatus to "On Hold"
   - [ ] Button hides after case is on hold
6. Test product-specific panels:
   - [ ] BTL case shows rent/ICR fields
   - [ ] Bridging case shows auction date/exit strategy
   - [ ] Dev Finance case shows build cost/GDV

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Progress bar not showing all 14 stages | Verify Stage Configuration list has all 14 items (P3-07); check gallery Items property |
| Stage not advancing | Check the Patch formula targets CurrentStageID with the correct lookup value |
| Skip checkbox showing for wrong stages | Verify CanSkip values in Stage Configuration — only stages 5, 6, 7 should be Yes |
| Product-specific fields blank | Ensure the Case record has these columns populated; they may be empty for new cases |
| Flow 6 not triggered after stage change | Verify the CurrentStageID column is actually changed by the Patch; check flow trigger conditions |
