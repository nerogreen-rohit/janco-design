# Task P5-03: Flow 10 — Calculate Advisory Fee (AdvFee)

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a system, when a Commission record of type "Advisory Fee (AdvFee)" is created or modified I need to validate the amount and update the case total so that advisory fees are tracked accurately  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** Commissions list (P5-01), Cases list commission columns active (P5-02)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 10 — Calculate Advisory Fee (AdvFee)`
3. Trigger: **When an item is created or modified** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Commissions`

### Step 2 — Add Trigger Condition

Add a trigger condition to limit the flow to Advisory Fee records only:

1. Click the trigger → **Settings** → **Trigger Conditions**
2. Add condition:

```
@equals(triggerOutputs()?['body/CommissionType/Value'], 'Advisory Fee (AdvFee)')
```

> **Terminology:** Always use "Advisory Fee (AdvFee)" — never abbreviate or use alternatives.

### Step 3 — Initialize Variables

| Action | Variable Name | Type | Value |
|---|---|---|---|
| Initialize variable | varCaseID | String | `triggerOutputs()?['body/CaseID']` |
| Initialize variable | varExpectedAmount | Float | `triggerOutputs()?['body/ExpectedAmount']` |
| Initialize variable | varTotalCommissionExpected | Float | 0 |

### Step 4 — Validate the Advisory Fee Amount

1. Add a **Condition** action:
   - Left: `varExpectedAmount`
   - Operator: `is greater than`
   - Right: `0`

2. **If No** (amount is 0 or blank):
   - Add **Send an HTTP request to SharePoint** or **Send a message** (Teams) to the case owner:
     - Message: `⚠️ Advisory Fee (AdvFee) for case @{variables('varCaseID')} has no expected amount. Please update.`
   - Terminate the flow (optional — or continue to still update total)

3. **If Yes**: Continue to Step 5

### Step 5 — Calculate TotalCommissionExpected

Sum all Commission records for this CaseID to calculate the total:

1. Add **Get items** (SharePoint):
   - List: `Commissions`
   - Filter Query: `CaseID eq '@{variables('varCaseID')}'`

2. Add **Apply to each** on the results:
   - Inside: Add **Compose** or **Increment variable**:
     - Set `varTotalCommissionExpected` = `add(variables('varTotalCommissionExpected'), items('Apply_to_each')?['ExpectedAmount'])`

> **Formula:** `TotalCommissionExpected = SUM(ExpectedAmount) for all Commission records WHERE CaseID = varCaseID`

### Step 6 — Update the Case Record

1. First, get the Case item. Add **Get items** (SharePoint):
   - List: `Specialist Lending Cases`
   - Filter Query: `CaseID eq '@{variables('varCaseID')}'`
   - Top Count: 1

2. Add **Update item** (SharePoint):
   - List: `Specialist Lending Cases`
   - Id: `first(outputs('Get_items_-_Case')?['body/value'])?['ID']`
   - AdvFeeCharged: `varExpectedAmount`
   - TotalCommissionExpected: `varTotalCommissionExpected`

### Step 7 — Flag if Not Received

1. Add a **Condition**:
   - Left: `triggerOutputs()?['body/IsReceived']`
   - Operator: `is equal to`
   - Right: `false`

2. **If Yes** (not yet received):
   - (Optional) Send a Teams notification to the case owner:
     - Message: `💰 Advisory Fee (AdvFee) of £@{variables('varExpectedAmount')} recorded for case @{variables('varCaseID')}. Status: Pending receipt.`

### Step 8 — Error Handling

1. Wrap Steps 5–7 in a **Scope** action
2. Add a parallel branch after the Scope configured to run on failure (**Configure run after** → "has failed")
3. In the failure branch:
   - Send a Teams notification to admin:
     - Message: `❌ Flow 10 failed for Commission record {triggerOutputs()?['body/Title']}. Please check.`

### Step 9 — Test

1. Create a Commission record in the Commissions list:
   - CaseID: (use an existing case)
   - CommissionType: `Advisory Fee (AdvFee)`
   - ExpectedAmount: `£1,500`
2. Wait up to 30 seconds
3. Verify:
   - [ ] Flow 10 ran successfully (check flow run history)
   - [ ] Case record `AdvFeeCharged` = £1,500
   - [ ] Case record `TotalCommissionExpected` updated (sum of all commissions for this case)
4. Create a second Commission record (e.g. Proc Fee, £2,000) for the same case
5. Verify `TotalCommissionExpected` = £3,500 (sum of both)

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Flow doesn't trigger | Verify trigger condition uses exact string `Advisory Fee (AdvFee)` — check for typos or extra spaces |
| TotalCommissionExpected incorrect | Check the Get items filter — ensure it pulls ALL commission records for the CaseID, not just AdvFee |
| Case record not found | Verify the CaseID in the Commission record matches the CaseID in the Cases list exactly |
| Flow runs for non-AdvFee records | Confirm the trigger condition is correctly configured in trigger settings |
| Currency amounts show as 0 | Ensure ExpectedAmount is mapped as a number/float, not a string |
