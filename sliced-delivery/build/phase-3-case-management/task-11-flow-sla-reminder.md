# Task P3-11: Flow 9 — SLA Reminder (Scheduled)

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a system, I need to check all active cases daily for SLA breaches and send Teams alerts so that overdue stages are flagged  
**Priority:** 🟡 High  
**Estimated effort:** 2–3 hours  
**Depends on:** Cases list (P1-01), Stage Configuration list with all 14 stages (P3-07), Tasks list (P3-04)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Scheduled cloud flow**
2. Name: `Flow 9 — SLA Reminder`
3. Repeat every: **1 Day**
4. Start time: Set to **08:00 AM** (local time)
5. Time zone: `GMT Standard Time`

   > **Important:** Use `GMT Standard Time` (the Windows time zone ID for UK time), **not** `(UTC+00:00) London`. The `GMT Standard Time` zone is DST-aware and automatically adjusts for British Summer Time (BST), ensuring the flow always runs at 08:00 local UK time year-round. Using a raw UTC offset would cause the flow to run at 09:00 during BST.

### Step 2 — Get All Active Cases

1. Add **Get items** (SharePoint):
   - List: `Specialist Lending Cases`
   - Filter Query: `CaseStatus eq 'Active'`
   - Top Count: 500

### Step 3 — Apply to Each Case

1. Add **Apply to each** on the results from Step 2

Inside the loop:

### Step 4 — Get Stage Configuration for Current Stage

1. Add **Get item** (SharePoint):
   - List: `Stage Configuration`
   - Id: `items('Apply_to_each')?['CurrentStageID']`
2. Add **Compose** — `DefaultSLADays`: `outputs('Get_Stage_Config')?['body/DefaultSLADays']`

### Step 5 — Calculate SLA Deadline

```
// SLA deadline = StageChangedDate + DefaultSLADays
addDays(
    items('Apply_to_each')?['StageChangedDate'],
    outputs('DefaultSLADays')
)
```

1. Add **Compose** — `SLADeadline`: (expression above)
2. Add **Compose** — `AtRiskDate`:

```
// At-risk = 1 day before deadline
addDays(outputs('SLADeadline'), -1)
```

### Step 6 — Check SLA Status

1. Add a **Condition**: Is the case **overdue**?
   - `formatDateTime(utcNow(), 'yyyy-MM-dd')` is greater than `formatDateTime(outputs('SLADeadline'), 'yyyy-MM-dd')`

2. If **Yes** (Overdue):
   - **Post message in a chat or channel** (Microsoft Teams):
     - Recipient: Case owner
     - Message:
       ```
       🔴 SLA BREACHED — @{items('Apply_to_each')?['CaseID']}

       Stage: @{items('Apply_to_each')?['CurrentStageName']}
       Stage entered: @{formatDateTime(items('Apply_to_each')?['StageChangedDate'], 'dd MMM yyyy')}
       SLA deadline: @{formatDateTime(outputs('SLADeadline'), 'dd MMM yyyy')}
       Days overdue: @{div(sub(ticks(utcNow()), ticks(outputs('SLADeadline'))), 864000000000)}

       Applicant: @{items('Apply_to_each')?['Applicant1FirstName']} @{items('Apply_to_each')?['Applicant1LastName']}
       Product: @{items('Apply_to_each')?['ProductType/Value']}
       ```

3. If **No**: Add another **Condition** — Is the case **at risk**?
   - `formatDateTime(utcNow(), 'yyyy-MM-dd')` is greater than or equal to `formatDateTime(outputs('AtRiskDate'), 'yyyy-MM-dd')`
   - AND `formatDateTime(utcNow(), 'yyyy-MM-dd')` is less than or equal to `formatDateTime(outputs('SLADeadline'), 'yyyy-MM-dd')`

4. If **Yes** (At Risk):
   - **Post message in a chat or channel** (Microsoft Teams):
     - Recipient: Case owner
     - Message:
       ```
       🟡 SLA AT RISK — @{items('Apply_to_each')?['CaseID']}

       Stage: @{items('Apply_to_each')?['CurrentStageName']}
       SLA deadline: @{formatDateTime(outputs('SLADeadline'), 'dd MMM yyyy')} (TOMORROW)

       Action needed to keep this case on track.
       ```

### Step 7 — Update Overdue Tasks

After the case loop, add a separate section for Tasks:

1. Add **Get items** (SharePoint):
   - List: `Tasks`
   - Filter Query: `Status ne 'Completed' and Status ne 'Cancelled'`
   - Top Count: 500

2. Add **Apply to each** on the results

3. Inside the loop, add a **Condition**:
   - `formatDateTime(utcNow(), 'yyyy-MM-dd')` is greater than `formatDateTime(items('Apply_to_each_Task')?['DueDate'], 'yyyy-MM-dd')`
   - AND `items('Apply_to_each_Task')?['IsOverdue']` is equal to `false`

4. If **Yes**:
   - **Update item** (SharePoint):
     - List: `Tasks`
     - Id: `items('Apply_to_each_Task')?['ID']`
     - IsOverdue: `Yes`

### Step 8 — Flow Summary Notification

At the end of the flow (after both loops), optionally send a daily summary:

```
📊 Daily SLA Check Complete — @{formatDateTime(utcNow(), 'dd MMM yyyy HH:mm')}

Cases checked: @{length(outputs('Get_Active_Cases')?['body/value'])}
Tasks updated to overdue: @{variables('varOverdueTaskCount')}
```

### Step 9 — Test

1. Create a test case with `StageChangedDate` set to 30 days ago and a stage with `DefaultSLADays = 5`
2. Manually trigger the flow (or wait for the scheduled run)
3. Verify:
   - [ ] Overdue case triggers a 🔴 Teams notification to the case owner
   - [ ] At-risk case (within 1 day of SLA) triggers a 🟡 Teams notification
   - [ ] Cases within SLA do not generate notifications
   - [ ] Overdue Tasks are flagged with IsOverdue = Yes
   - [ ] Tasks already marked as overdue are not re-updated

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Flow doesn't run at 08:00 | Check the recurrence trigger timezone setting; verify flow is turned on |
| All cases show as overdue | Verify StageChangedDate is being updated when stages advance (Flow 6) |
| No Teams notifications received | Verify the case owner's email resolves correctly for Teams posting |
| Too many API calls (throttling) | Add concurrency control on Apply to each (set to 1–5); consider batching |
| StageChangedDate is null | Add a condition to skip cases where StageChangedDate is blank (new cases not yet staged) |
