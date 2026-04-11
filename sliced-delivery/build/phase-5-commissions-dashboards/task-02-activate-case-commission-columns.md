# Task P5-02: Activate Case Commission Summary Columns

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a developer, I need to activate the commission summary columns on the Cases list (created in Phase 1, now populated) so that case-level commission totals are visible  
**Priority:** 🔴 Critical  
**Estimated effort:** 30 minutes – 1 hour  
**Depends on:** Cases list (P1-01), Lenders list (P3-03)

---

## Step-by-Step Guide

### Step 1 — Verify Existing Commission Columns on Cases List

These columns were created in Phase 1 (Task P1-01) as "future columns." Confirm they exist:

1. Open the Cases list: **⚙️ Settings** → **List Settings**
2. Scroll to the Columns section
3. Verify the following columns are present:

| Column Name | Type | Expected Status |
|---|---|---|
| ProcFeeExpected | Currency | Created in P1-01, currently empty |
| AdvFeeCharged | Currency | Created in P1-01, currently empty |
| TotalCommissionExpected | Currency | Created in P1-01, currently empty |
| CommissionReceived | Yes/No | Created in P1-01, currently empty |
| CommissionReceivedDate | Date and Time | Created in P1-01, currently empty |

> If any column is missing, create it now using the specifications from Task P1-01.

### Step 2 — Verify LenderID Lookup Column

The `LenderID` column should have been created in Phase 1 as a placeholder. Now that the Lenders list exists (Phase 3), verify the lookup is correctly configured:

1. In the Cases list **List Settings**, click on the `LenderID` column
2. Confirm:
   - Type: **Lookup**
   - Lookup list: **Lenders**
   - Lookup column: **Title**
3. If the lookup was not connected in Phase 1 (because Lenders didn't exist yet), update it now:
   - Delete the placeholder column and re-create it as a Lookup
   - Name: `LenderID`
   - Lookup list: `Lenders`
   - Lookup column: `Title`

### Step 3 — Verify LenderName Column

1. Confirm `LenderName` (Single line of text) exists on the Cases list
2. This column stores the denormalized lender name and will be set by Flow 11 when a lender is assigned

### Step 4 — Add Commission Columns to Active Cases View

1. Go to the **Active Cases** view (created in P1-01)
2. Edit the view → add these columns:
   - `ProcFeeExpected`
   - `TotalCommissionExpected`
   - `CommissionReceived`
3. Save the view

### Step 5 — Create a Commission Summary View

1. Go to **List Settings** → **Views** → **Create view**
2. Name: `Commission Summary`
3. Filter: `CaseStatus is equal to Active` OR `CaseStatus is equal to Completed`
4. Columns to show: `CaseID`, `ProductType`, `Applicant1FirstName`, `Applicant1LastName`, `LenderName`, `ProcFeeExpected`, `AdvFeeCharged`, `TotalCommissionExpected`, `CommissionReceived`, `CommissionReceivedDate`
5. Sort by: `CommissionReceived` (ascending — unreceived first), then `EnquiryDate` (descending)

### Step 6 — Verify

- [ ] All 5 commission summary columns exist on the Cases list: `ProcFeeExpected`, `AdvFeeCharged`, `TotalCommissionExpected`, `CommissionReceived`, `CommissionReceivedDate`
- [ ] `LenderID` lookup correctly points to the Lenders list
- [ ] `LenderName` column exists (Single line of text)
- [ ] Active Cases view includes commission columns
- [ ] Commission Summary view is created and displays the correct columns
- [ ] Currency columns display in GBP (£)

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| LenderID lookup shows no items | Verify the Lenders list (Phase 3) exists and has at least one row |
| Commission columns missing from Cases list | Recreate them per the P1-01 specification — they may have been skipped as "future" columns |
| Currency format incorrect | Edit column settings and set the locale/currency to GBP |
| CommissionReceived defaults to blank instead of No | Edit the column and set Default value to "No" |
