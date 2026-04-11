# Task P5-05: Flow 16 — Case Completion Wrap-Up

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a system, when a case status changes to "Completed" I need to send a completion email, close outstanding document requests, and remind the case owner to review commissions  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Cases list (P1-01), Document Requests list (P3-03), Commissions list (P5-01)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 16 — Case Completion Wrap-Up`
3. Trigger: **When an item is created or modified** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Specialist Lending Cases`

### Step 2 — Add Trigger Condition

Only fire when CaseStatus is set to "Completed":

1. Click the trigger → **Settings** → **Trigger Conditions**
2. Add condition:

```
@equals(triggerOutputs()?['body/CaseStatus/Value'], 'Completed')
```

### Step 3 — Initialize Variables

| Action | Variable Name | Type | Value |
|---|---|---|---|
| Initialize variable | varCaseID | String | `triggerOutputs()?['body/CaseID']` |
| Initialize variable | varCaseName | String | `concat(triggerOutputs()?['body/Applicant1FirstName'], ' ', triggerOutputs()?['body/Applicant1LastName'])` |
| Initialize variable | varCustomerEmail | String | `triggerOutputs()?['body/Applicant1Email']` |
| Initialize variable | varCaseOwnerEmail | String | `triggerOutputs()?['body/CaseOwner/EMail']` |
| Initialize variable | varCaseOwnerName | String | `triggerOutputs()?['body/CaseOwner/Title']` |

### Step 4 — Action 1: Send Completion Email to Customer

1. Add **Send an email (V2)** (Office 365 Outlook):
   - To: `varCustomerEmail`
   - Subject: `Congratulations — Your Case @{variables('varCaseID')} is Complete!`
   - Body (HTML):

```html
<html>
<body style="font-family: Arial, sans-serif; color: #333;">
  <h2>Congratulations, @{variables('varCaseName')}!</h2>
  <p>We are pleased to confirm that your lending case
     <strong>@{variables('varCaseID')}</strong> has been completed.</p>
  <p>Thank you for choosing our specialist lending services.
     If you have any questions about your mortgage or need further
     assistance, please don't hesitate to get in touch.</p>
  <hr />
  <p><strong>Your Advisor:</strong> @{variables('varCaseOwnerName')}</p>
  <p><strong>Contact:</strong> @{variables('varCaseOwnerEmail')}</p>
  <hr />
  <p style="font-size: 12px; color: #888;">
    This is an automated message from the Specialist Lending team.
  </p>
</body>
</html>
```

### Step 5 — Action 2: Close Outstanding Document Requests

Mark all outstanding Document Requests for this case as "Not Required":

1. Add **Get items** (SharePoint):
   - List: `Document Requests`
   - Filter Query: `CaseID eq '@{variables('varCaseID')}' and Status ne 'Received' and Status ne 'Not Required'`

2. Add **Apply to each** on the results:
   - Inside: Add **Update item** (SharePoint):
     - List: `Document Requests`
     - Id: `items('Apply_to_each')?['ID']`
     - Status: `Not Required`
     - Notes: `Auto-closed on case completion (@{utcNow()})`

### Step 6 — Action 3: Send Commission Review Reminder to Case Owner

1. Add **Post message in a chat or channel** (Microsoft Teams):
   - Post in: `Chat with Flow bot`
   - Recipient: `varCaseOwnerEmail`
   - Message:

```
💰 Commission Review Reminder

Case **@{variables('varCaseID')}** (@{variables('varCaseName')}) has been marked as **Completed**.

Please review the commission records for this case:
• Are all expected commissions recorded (Proc Fee, Advisory Fee (AdvFee), Referral Fees)?
• Have actual amounts been confirmed?
• Have received commissions been marked as received?

Open the Case Workspace → Commission tab to review.
```

### Step 7 — Update Case with Actual Completion Date

1. Add **Update item** (SharePoint):
   - List: `Specialist Lending Cases`
   - Id: `triggerOutputs()?['body/ID']`
   - ActualCompletionDate: `utcNow()`

### Step 8 — Error Handling

1. Wrap Steps 4–7 in a **Scope**
2. Add a parallel failure branch (**Configure run after** → "has failed"):
   - Send Teams notification to admin:
     - Message: `❌ Flow 16 — Case Completion Wrap-Up failed for case @{variables('varCaseID')}. Actions may be incomplete. Please check.`

### Step 9 — Test

1. Open an existing Case record with:
   - At least one Document Request with Status ≠ "Received"
   - At least one Commission record
2. Change `CaseStatus` to `Completed`
3. Wait up to 60 seconds
4. Verify:
   - [ ] Customer receives completion email (check inbox or flow run details)
   - [ ] Email contains correct case ID, customer name, and advisor details
   - [ ] All outstanding Document Requests now have Status = "Not Required"
   - [ ] Document Requests that were already "Received" are unchanged
   - [ ] Case owner receives Teams message with commission review reminder
   - [ ] ActualCompletionDate is set on the Case record

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Email not received | Check flow run history for errors; verify Applicant1Email is valid; check spam folder |
| Document Requests not updating | Verify filter query field names match the Document Requests list; check CaseID matches exactly |
| Teams message not sent | Ensure the Flow bot connector is authorized; verify case owner email is valid |
| Flow triggers multiple times | Add a condition to check if ActualCompletionDate is already set — if yes, skip (prevents re-run on subsequent edits) |
| HTML email renders incorrectly | Test in Outlook and Gmail; use inline styles only (no external CSS) |
