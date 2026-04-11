# Task P1-09: Flow 15 — Fact-Find Updated Notification

**Phase:** 1 — Fact-Find  
**Story:** As a system, when a customer updates their fact-find I need to notify the case owner via Teams so that they can review changes promptly  
**Priority:** 🟡 High  
**Estimated effort:** 30–45 minutes  
**Depends on:** Fact-Find Responses list (P1-03), Cases list (P1-01)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 15 — Fact-Find Updated Notification`
3. Trigger: **When an item is created or modified** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Fact-Find Responses`

### Step 2 — Get the Linked Case

1. Add **Get items** (SharePoint):
   - List: `Specialist Lending Cases`
   - Filter Query: `CaseID eq '@{triggerOutputs()?['body/CaseID']}'`
   - Top Count: 1

2. Add **Compose** — `CaseOwnerEmail`:
   - `first(outputs('Get_items')?['body/value'])?['CaseOwner']?['Email']`

3. Add **Compose** — `CaseOwnerName`:
   - `first(outputs('Get_items')?['body/value'])?['CaseOwner']?['DisplayName']`

### Step 3 — Update LastUpdatedByCustomer Timestamp

1. Add **Update item** (SharePoint):
   - List: `Fact-Find Responses`
   - Id: `triggerOutputs()?['body/ID']`
   - LastUpdatedByCustomer: `utcNow()`

### Step 4 — Send Teams Notification to Case Owner

1. Add **Post message in a chat or channel** (Microsoft Teams):
   - Post as: Flow bot
   - Post in: Chat with Flow bot
   - Recipient: `@{outputs('CaseOwnerEmail')}`
   - Message:
   ```
   📋 Fact-Find Updated

   Customer has updated their fact-find for case @{triggerOutputs()?['body/CaseID']}.

   Last updated: @{utcNow()}

   Open the case in the Staff App to review the latest information.
   ```

### Step 5 — Optional: Update Fact-Find Status

If the fact-find status is "Not Started", update it to "In Progress":

1. Add **Condition**:
   - `triggerOutputs()?['body/Status/Value']` is equal to `Not Started`
2. If yes:
   - **Update item** (SharePoint): set Status = `In Progress`

### Step 6 — Prevent Loop (Important!)

Since this flow triggers on modification AND also modifies the item (Step 3), add a trigger condition to prevent infinite loops:

1. Click the trigger → **Settings** → **Trigger Conditions**
2. Add condition:
   ```
   @not(equals(triggerOutputs()?['body/Editor']?['Email'], 'flowaccount@yourtenant.com'))
   ```
   Replace with the email of the flow connection account.

   **Alternative:** Check the `Modified By` field — only fire if the modifier is NOT the flow's service account.

### Step 7 — Test

1. Open a Fact-Find item via the guest share link (simulating a customer)
2. Update a field (e.g. A1FirstName) and save
3. Verify:
   - [ ] LastUpdatedByCustomer is set to current timestamp
   - [ ] Case owner receives a Teams notification
   - [ ] Notification includes the CaseID
   - [ ] If Status was "Not Started", it is now "In Progress"
   - [ ] Flow does NOT trigger again from its own update (no infinite loop)

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Infinite loop — flow keeps triggering | Add trigger condition to exclude flow's own modifications (Step 6) |
| Teams notification not received | Verify case owner has a Teams account; check the recipient email |
| Wrong case linked | Verify CaseID on the Fact-Find item matches a Cases list item |
