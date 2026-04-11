# Task P1-07: Flow 4 — Create Fact-Find Item & Guest Link

**Phase:** 1 — Fact-Find  
**Story:** As a system, when a new case is created I need to create a Fact-Find Responses item and generate a guest share link so that the customer can fill in their fact-find  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** Cases list (P1-01), Fact-Find Responses list (P1-03), Flow 3 running (P1-06)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 4 — Create Fact-Find Item`
3. Trigger: **When an item is created** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Specialist Lending Cases`

### Step 2 — Wait for CaseID

Flow 3 generates the CaseID asynchronously. Flow 4 must wait for it.

1. Add **Do until** loop:
   - Condition: `CaseID is not equal to null` (or `length(CaseID) > 0`)
   - Limit: Count = 10, Timeout = PT5M
   - Inside the loop:
     - **Delay** — 5 seconds
     - **Get item** (SharePoint) — re-read the Case item by ID
     - Set `varCaseID` = retrieved CaseID value

### Step 3 — Create Fact-Find Responses Item

1. Add **Create item** (SharePoint):
   - List: `Fact-Find Responses`
   - Title: `@{variables('varCaseID')}`
   - CaseID: `@{variables('varCaseID')}`
   - CaseRef: `@{variables('varCaseID')}`
   - Status: `Not Started`

### Step 4 — Generate Guest Share Link

1. Add **Create sharing link for a file or folder** (SharePoint):
   - Site Address: `https://[tenant].sharepoint.com/sites/lending`
   - Library Name / List Name: `Fact-Find Responses`
   - Item Id: `@{outputs('Create_item')?['body/ID']}`
   - Link type / Permission: `Edit` (or the least privilege required)
   - Link scope: `Specific people`
   - People: the customer email field from the Case item (e.g. `triggerOutputs()?['body/Applicant1Email']`)

   > Use **Specific people** for external customers so the recipient can access the item via their email address without requiring an internal tenant account. Only use **Anyone with the link** if your tenant sharing policy explicitly allows anonymous links.

2. Add **Compose** — extract the share link URL from the sharing action output:
   - Use the dynamic content for the generated sharing link URL from **Create sharing link for a file or folder**

### Step 5 — Update Case Record with Fact-Find Link

1. Add **Update item** (SharePoint):
   - List: `Specialist Lending Cases`
   - Id: `triggerOutputs()?['body/ID']`
   - FactFindItemID: `outputs('Create_item')?['body/ID']`

### Step 6 — Store the Guest Link

You can store the fact-find guest link in a custom column on the Cases list (e.g. `FactFindGuestLink`) or pass it directly to the Welcome Email flow.

**Recommended:** Add a column `FactFindGuestLink` (Hyperlink) to the Cases list:
1. In the Cases list, add column: `FactFindGuestLink` → Hyperlink
2. In this flow, update the Case record: `FactFindGuestLink = [share link URL]`

### Step 7 — Test

1. Create a new Case item
2. Wait for Flow 3 (CaseID) and Flow 4 to complete (up to 60 seconds)
3. Verify:
   - [ ] A Fact-Find Responses item exists with Title = CaseID
   - [ ] FactFindItemID is set on the Case record
   - [ ] The guest share link works in an incognito browser (no M365 login required)
   - [ ] The link opens the specific Fact-Find item's edit form
   - [ ] Customer can edit and save the form

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Flow 4 can't find CaseID | Increase the Do Until timeout or delay; ensure Flow 3 is working |
| Guest link requires M365 login | Check SharePoint tenant guest sharing settings — must be "New and existing guests" or "Anyone" |
| HTTP request fails with 403 | Flow identity needs sufficient permissions — use site collection admin or grant explicit access |
| Fact-Find item created but link not stored | Check the Compose action parsing the share link response |
