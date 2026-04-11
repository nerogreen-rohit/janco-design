# Task P5-06: Case Workspace — Commission Tab

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a staff member, I want a Commission tab on the Case Workspace showing all commission records for the current case so that I can view and add commissions in context  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Commissions list (P5-01), Staff App Case Workspace (P1-12)

---

## Step-by-Step Guide

### Step 1 — Add the Commission Tab to the Case Workspace

1. Open the **Specialist Lending Staff App** in Power Apps Studio
2. Navigate to the **Case Workspace** screen
3. Locate the tab bar (Overview | Fact-Find | Documents | Messages | Timeline)
4. Add a new tab button:
   - Text: `Commission`
   - OnSelect: `Set(varActiveTab, "Commission")`
5. Update the tab underline/highlight logic to include "Commission"

### Step 2 — Create the Commission Gallery

1. Add a **Gallery** control below the tab bar, visible when `varActiveTab = "Commission"`
2. Name: `galCaseCommissions`
3. Data source: `Commissions`
4. Items formula:

```
Filter(
    Commissions,
    CaseID = varCurrentCaseID
)
```

5. Configure gallery template with 4 columns:

| Column | Control | Value |
|---|---|---|
| Type | Label | `ThisItem.CommissionType.Value` |
| Expected | Label | `Text(ThisItem.ExpectedAmount, "£#,##0.00")` |
| Actual/Received | Label | `If(ThisItem.IsReceived, Text(ThisItem.ActualAmount, "£#,##0.00"), "—")` |
| Status | Label | `If(ThisItem.IsReceived, "✅ Received", "⏳ Pending")` |

### Step 3 — Add Column Headers

Add a header row above the gallery:

| Header Label | Width | Text |
|---|---|---|
| lblCommType | 200 | `"Type"` |
| lblCommExpected | 150 | `"Expected"` |
| lblCommReceived | 150 | `"Received"` |
| lblCommStatus | 150 | `"Status"` |

### Step 4 — Add the [+ Add Commission Record] Button

1. Add a **Button** control above the gallery (right-aligned):
   - Text: `"+ Add Commission Record"`
   - OnSelect: `Set(varShowCommissionForm, true)`

### Step 5 — Create the Commission Entry Form

1. Add a **Form** control (or custom card layout), visible when `varShowCommissionForm = true`
2. Name: `frmAddCommission`
3. Data source: `Commissions`
4. DefaultMode: `FormMode.New`
5. Configure form fields:

| Field Label | Control Type | Data Source Column | Notes |
|---|---|---|---|
| Commission Type | Dropdown | CommissionType | Choices: Proc Fee, Advisory Fee (AdvFee), Introducer Fee, Referral Fee |
| Expected Amount | Text Input (number) | ExpectedAmount | Format as currency |
| Actual Amount | Text Input (number) | ActualAmount | Optional — may be added later |
| Received? | Toggle | IsReceived | Default: Off |
| Received Date | Date Picker | ReceivedDate | Visible only when IsReceived = true |
| Invoice Number | Text Input | InvoiceNumber | Optional |
| Payment Method | Dropdown | PaymentMethod | BACS, Cheque, Deducted at Completion, Other |
| Notes | Text Input (multiline) | Notes | Optional |

### Step 6 — Form Submit Action

On the **Save** button's `OnSelect`:

```
Patch(
    Commissions,
    Defaults(Commissions),
    {
        Title: varCurrentCaseID & "-" & ddCommissionType.Selected.Value,
        CaseID: varCurrentCaseID,
        CaseName: varCurrentCaseName,
        LenderName: varCurrentLenderName,
        ProductType: varCurrentProductType,
        CommissionType: ddCommissionType.Selected,
        ExpectedAmount: Value(txtExpectedAmount.Text),
        ActualAmount: Value(txtActualAmount.Text),
        IsReceived: tglIsReceived.Value,
        ReceivedDate: If(tglIsReceived.Value, dpReceivedDate.SelectedDate, Blank()),
        InvoiceNumber: txtInvoiceNumber.Text,
        PaymentMethod: ddPaymentMethod.Selected,
        Notes: txtCommNotes.Text
    }
);
Set(varShowCommissionForm, false);
Reset(frmAddCommission);
Notify("Commission record saved.", NotificationType.Success)
```

### Step 7 — Add a Summary Row

Below the gallery, add a summary section:

```
Total Expected:  Text(Sum(Filter(Commissions, CaseID = varCurrentCaseID), ExpectedAmount), "£#,##0.00")
Total Received:  Text(Sum(Filter(Commissions, CaseID = varCurrentCaseID && IsReceived), ActualAmount), "£#,##0.00")
```

### Step 8 — Wireframe Reference

```
╔══════════════════════════════════════════════════════════════════════╗
║  BTL-2026-0001 — John & Jane Doe                                     ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Overview] [Fact-Find] [Documents] [Messages] [Timeline] [Commission]║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                       ║
║  COMMISSIONS FOR BTL-2026-0001         [+ Add Commission Record]      ║
║                                                                       ║
║  ┌──────────────────┬──────────────┬──────────────┬──────────────┐   ║
║  │  Type            │  Expected    │  Received    │  Status      │   ║
║  ├──────────────────┼──────────────┼──────────────┼──────────────┤   ║
║  │  Proc Fee        │  £2,137      │  £2,137      │  ✅ Received  │   ║
║  │  Advisory Fee    │  £1,500      │  —           │  ⏳ Pending   │   ║
║  │  (AdvFee)        │              │              │              │   ║
║  └──────────────────┴──────────────┴──────────────┴──────────────┘   ║
║                                                                       ║
║  Total Expected: £3,637          Total Received: £2,137               ║
╚══════════════════════════════════════════════════════════════════════╝
```

### Step 9 — Verify

- [ ] Commission tab appears on Case Workspace tab bar
- [ ] Gallery shows only commission records for the current case
- [ ] Commission Type displays "Advisory Fee (AdvFee)" correctly — never abbreviated
- [ ] [+ Add Commission Record] button opens the entry form
- [ ] Form defaults CaseID, CaseName, LenderName, and ProductType from the current case
- [ ] Saving a record updates the gallery immediately
- [ ] Summary row displays correct totals for Expected and Received
- [ ] Status column shows ✅ Received or ⏳ Pending correctly

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Gallery shows all commissions, not just current case | Check the Filter formula uses `varCurrentCaseID` and it's set when navigating to the Case Workspace |
| Commission Type dropdown shows wrong values | Verify the Commissions list CommissionType choices match exactly: Proc Fee, Advisory Fee (AdvFee), Introducer Fee, Referral Fee |
| Currency formatting missing £ symbol | Use `Text(value, "£#,##0.00")` format string |
| Form doesn't reset after save | Ensure `Reset(frmAddCommission)` is called after Patch |
| Summary row shows incorrect totals | Verify the Filter inside Sum matches the gallery filter |
