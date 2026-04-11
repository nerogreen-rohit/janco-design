# Task P3-08: Flow 6 — Log Stage Change

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a system, when a case stage changes I need to log it to Stage History with text snapshots so that an immutable audit trail exists  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Cases list (P1-01), Stage Configuration list with all 14 stages (P3-07), Case Stage History list (P3-01)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 6 — Log Stage Change`
3. Trigger: **When an item is created or modified** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Specialist Lending Cases`

### Step 2 — Configure Trigger Condition

Add a trigger condition so the flow only runs when the stage actually changes:

**Settings → Trigger Conditions:**

```
@not(equals(triggerOutputs()?['body/CurrentStageID'], triggerOutputs()?['body/{PreviousCurrentStageID}']))
```

> **Alternative approach:** If trigger conditions on lookup changes are unreliable, use a Compose action to compare old vs. new CurrentStageNumber and terminate early if unchanged.

### Step 3 — Initialize Variables

| Action | Variable Name | Type | Value |
|---|---|---|---|
| Initialize variable | varCaseID | String | `triggerOutputs()?['body/CaseID']` |
| Initialize variable | varCaseName | String | `triggerOutputs()?['body/Title']` |
| Initialize variable | varNewStageID | Integer | `triggerOutputs()?['body/CurrentStageID']` |
| Initialize variable | varPrevStageNumber | Integer | `0` |
| Initialize variable | varPrevStageName | String | `''` |
| Initialize variable | varIsSkipped | Boolean | `false` |

### Step 4 — Read the New Stage Configuration

1. Add **Get item** (SharePoint):
   - List: `Stage Configuration`
   - Id: `@{variables('varNewStageID')}`
2. Add **Compose** — `NewStageName`: `outputs('Get_item')?['body/Title']`
3. Add **Compose** — `NewStageNumber`: `outputs('Get_item')?['body/StageNumber']`
4. Add **Compose** — `CustomerFriendlyName`: `outputs('Get_item')?['body/CustomerFriendlyName']`
5. Add **Compose** — `VisibleToCustomer`: `outputs('Get_item')?['body/VisibleToCustomer']`

### Step 5 — Get Previous Stage History Record

1. Add **Get items** (SharePoint):
   - List: `Case Stage History`
   - Filter Query: `CaseID eq '@{variables('varCaseID')}'`
   - Order By: `ChangedDate desc`
   - Top Count: 1
2. Add a **Condition**: `length(outputs('Get_items_-_Previous_History')?['body/value'])` is greater than 0
3. If **Yes**:
   - Set `varPrevStageNumber` = `first(outputs('Get_items_-_Previous_History')?['body/value'])?['StageNumber']`
   - Set `varPrevStageName` = `first(outputs('Get_items_-_Previous_History')?['body/value'])?['StageName']`

### Step 6 — Calculate Time in Previous Stage

```
// TimeInPreviousStageHours — difference between now and StageChangedDate on the Case
div(
    sub(
        ticks(utcNow()),
        ticks(triggerOutputs()?['body/StageChangedDate'])
    ),
    36000000000
)
```

> **Note:** `36000000000` = ticks per hour. This gives hours as a decimal. If StageChangedDate is null (first stage), set to 0.

### Step 7 — Detect Stage Skipping

```
// If the new stage number is more than 1 greater than the previous, a stage was skipped
// The Staff App sets a flag when the user explicitly skips a stage
```

Add a **Condition**: `triggerOutputs()?['body/StageNotes']` contains "SKIPPED"

If **Yes**: Set `varIsSkipped` to `true`

### Step 8 — Create the Stage History Record

1. Add **Create item** (SharePoint):
   - List: `Case Stage History`
   - Title: `@{variables('varCaseID')}-Stage@{outputs('NewStageNumber')}`
   - CaseID: `@{variables('varCaseID')}`
   - CaseName: `@{variables('varCaseName')}`
   - StageNumber: `@{outputs('NewStageNumber')}`
   - StageName: `@{outputs('NewStageName')}` ← **Text snapshot, NOT a lookup**
   - PreviousStageNumber: `@{variables('varPrevStageNumber')}`
   - PreviousStageName: `@{variables('varPrevStageName')}` ← **Text snapshot**
   - ChangedBy: `triggerOutputs()?['body/Editor/Claims/emailaddress']`
   - ChangedDate: `utcNow()`
   - TimeInPreviousStageHours: `@{outputs('TimeInPreviousStageHours')}`
   - Notes: `triggerOutputs()?['body/StageNotes']`
   - IsSkipped: `@{variables('varIsSkipped')}`

### Step 9 — Update Case Denormalized Fields

1. Add **Update item** (SharePoint):
   - List: `Specialist Lending Cases`
   - Id: `triggerOutputs()?['body/ID']`
   - CurrentStageNumber: `@{outputs('NewStageNumber')}`
   - CurrentStageName: `@{outputs('NewStageName')}`
   - StageChangedDate: `utcNow()`

### Step 10 — Trigger Customer Notification (Conditional)

1. Add a **Condition**: `outputs('VisibleToCustomer')` is equal to `true`
2. If **Yes**: Call **Flow 13 — Customer Status Update** (child flow) or use an HTTP action to trigger it, passing:
   - CaseID
   - CustomerFriendlyName
   - Applicant1Email (from trigger)

### Step 11 — Error Handling

1. Wrap Steps 4–10 in a **Scope**
2. Add a parallel branch with **Configure run after** → "has failed"
3. In the failure branch:
   - Send a Teams notification to the admin channel:
     ```
     ⚠️ Flow 6 Failed — Stage Change Logging
     Case: @{variables('varCaseID')}
     Error: @{result('Scope')?[0]?['error']?['message']}
     ```

### Step 12 — Test

1. Open a test case in the Cases list
2. Change the CurrentStageID to the next stage (e.g. Stage 2 → Stage 3)
3. Wait 30–60 seconds
4. Verify:
   - [ ] A new item appears in Case Stage History with correct StageName (text, not lookup)
   - [ ] PreviousStageNumber and PreviousStageName are correct
   - [ ] TimeInPreviousStageHours is calculated (non-zero if time elapsed)
   - [ ] Case record has updated CurrentStageNumber and CurrentStageName
   - [ ] StageChangedDate is updated to now
   - [ ] If VisibleToCustomer = Yes, Flow 13 was triggered (check run history)

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Flow runs on every case edit, not just stage changes | Add a trigger condition to compare CurrentStageID before/after, or use a Compose + Terminate pattern |
| StageName is blank in history | Verify the Get item action reads from Stage Configuration correctly |
| TimeInPreviousStageHours is 0 | Check that StageChangedDate on the Case was previously set; first transition may be 0 |
| Flow 13 not triggered | Verify the condition checks VisibleToCustomer correctly; check child flow connection |
| Duplicate history records | Ensure trigger conditions prevent multiple runs for the same change |
