# Task P5-04: Flow 11 — Calculate Proc Fee

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a system, when a Lender is assigned to a case I need to auto-calculate the expected proc fee from `LoanAmountRequired × TypicalProcFeePercent` so that proc fee estimates are always current  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Commissions list (P5-01), Cases list commission columns (P5-02), Lenders list (P3-06)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 11 — Calculate Proc Fee`
3. Trigger: **When an item is created or modified** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Specialist Lending Cases`

### Step 2 — Add Trigger Condition

Only fire when LenderID has a value:

1. Click the trigger → **Settings** → **Trigger Conditions**
2. Add condition:

```
@not(empty(triggerOutputs()?['body/LenderID/Value']))
```

### Step 3 — Initialize Variables

| Action | Variable Name | Type | Value |
|---|---|---|---|
| Initialize variable | varCaseID | String | `triggerOutputs()?['body/CaseID']` |
| Initialize variable | varLoanAmount | Float | `triggerOutputs()?['body/LoanAmountRequired']` |
| Initialize variable | varLenderID | Integer | `triggerOutputs()?['body/LenderIDId']` |
| Initialize variable | varProcFeePercent | Float | 0 |
| Initialize variable | varProcFeeExpected | Float | 0 |
| Initialize variable | varTotalCommissionExpected | Float | 0 |

### Step 4 — Read Lender Proc Fee Percentage

1. Add **Get item** (SharePoint):
   - List: `Lenders`
   - Id: `varLenderID`

2. Add **Set variable**:
   - Name: `varProcFeePercent`
   - Value: `outputs('Get_item_-_Lender')?['body/TypicalProcFeePercent']`

### Step 5 — Calculate Expected Proc Fee

1. Add **Set variable**:
   - Name: `varProcFeeExpected`
   - Value: `mul(variables('varLoanAmount'), div(variables('varProcFeePercent'), 100))`

> **Formula:** `ProcFeeExpected = LoanAmountRequired × (TypicalProcFeePercent / 100)`  
> Example: £285,000 × 0.75% = £2,137.50

### Step 6 — Check for Existing Proc Fee Commission Record

1. Add **Get items** (SharePoint):
   - List: `Commissions`
   - Filter Query: `CaseID eq '@{variables('varCaseID')}' and CommissionType eq 'Proc Fee'`
   - Top Count: 1

2. Add **Condition**:
   - Left: `length(outputs('Get_items_-_Existing_ProcFee')?['body/value'])`
   - Operator: `is greater than`
   - Right: `0`

### Step 7a — Update Existing Commission Record (If Yes)

If a Proc Fee record already exists (handles lender change mid-case):

1. Add **Update item** (SharePoint):
   - List: `Commissions`
   - Id: `first(outputs('Get_items_-_Existing_ProcFee')?['body/value'])?['ID']`
   - ExpectedAmount: `varProcFeeExpected`
   - LenderName: `outputs('Get_item_-_Lender')?['body/Title']`
   - Title: `@{variables('varCaseID')}-Proc Fee`

### Step 7b — Create New Commission Record (If No)

1. Add **Create item** (SharePoint):
   - List: `Commissions`
   - Title: `@{variables('varCaseID')}-Proc Fee`
   - CaseID: `varCaseID`
   - CaseName: `concat(triggerOutputs()?['body/Applicant1FirstName'], ' ', triggerOutputs()?['body/Applicant1LastName'])`
   - LenderName: `outputs('Get_item_-_Lender')?['body/Title']`
   - ProductType: `triggerOutputs()?['body/ProductType/Value']`
   - CommissionType: `Proc Fee`
   - ExpectedAmount: `varProcFeeExpected`
   - IsReceived: `No`
   - TaxYear: (derive from current date — e.g. if month >= April then "YYYY/YY+1", else "YYYY-1/YY")

### Step 8 — Calculate TotalCommissionExpected

Sum all Commission records for this CaseID:

1. Add **Get items** (SharePoint):
   - List: `Commissions`
   - Filter Query: `CaseID eq '@{variables('varCaseID')}'`

2. Loop through results and sum `ExpectedAmount` into `varTotalCommissionExpected`

### Step 9 — Update the Case Record

1. Add **Update item** (SharePoint):
   - List: `Specialist Lending Cases`
   - Id: `triggerOutputs()?['body/ID']`
   - ProcFeeExpected: `varProcFeeExpected`
   - LenderName: `outputs('Get_item_-_Lender')?['body/Title']`
   - TotalCommissionExpected: `varTotalCommissionExpected`

### Step 10 — Error Handling

1. Wrap Steps 4–9 in a **Scope**
2. Add a parallel failure branch (**Configure run after** → "has failed"):
   - Send Teams notification to admin:
     - Message: `❌ Flow 11 — Proc Fee calculation failed for case @{variables('varCaseID')}. Please investigate.`

### Step 11 — Test

1. Open an existing Case record and set `LenderID` to a lender with `TypicalProcFeePercent = 0.75`
2. Set `LoanAmountRequired = 285000`
3. Wait up to 30 seconds
4. Verify:
   - [ ] A new Commission record exists with CommissionType = "Proc Fee" and ExpectedAmount = £2,137.50
   - [ ] Case record `ProcFeeExpected` = £2,137.50
   - [ ] Case record `TotalCommissionExpected` updated correctly
5. Change the Lender to a different lender (TypicalProcFeePercent = 1.0)
6. Verify:
   - [ ] The existing Proc Fee Commission record is updated (not duplicated)
   - [ ] ExpectedAmount = £2,850.00
   - [ ] Case record `ProcFeeExpected` updated to £2,850.00

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Flow doesn't trigger when LenderID changes | Ensure trigger condition checks for non-empty LenderID; verify the column internal name matches |
| Proc fee calculated as 0 | Check that TypicalProcFeePercent is stored as a number (e.g. 0.75) not a string; verify the formula divides by 100 only if stored as a percentage |
| Duplicate Proc Fee records created | Verify the filter query in Step 6 matches on both CaseID and CommissionType = "Proc Fee" |
| LenderName not populating | Ensure the Get item action on Lenders list returns the Title field |
| Flow runs in a loop | Add a condition to skip if ProcFeeExpected hasn't changed — compare old vs new values |
