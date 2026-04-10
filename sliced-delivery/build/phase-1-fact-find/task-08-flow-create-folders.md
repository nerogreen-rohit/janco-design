# Task P1-08: Flow 5 — Create Case Folder Structure & Share Links

**Phase:** 1 — Fact-Find  
**Story:** As a system, when a new case is created I need to create the 13 document subfolders and share folders 01–04 with the customer  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Cases list (P1-01), Case Documents library (P1-05), Flow 3 running (P1-06)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 5 — Create Case Folders`
3. Trigger: **When an item is created** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Specialist Lending Cases`

### Step 2 — Wait for CaseID

Same pattern as Flow 4:

1. Add **Do until** loop waiting for CaseID to be set (Flow 3 dependency)
2. Timeout: PT5M, Count: 10
3. Inside loop: Delay 5 seconds → Get item → check CaseID

### Step 3 — Initialize Folder Names Array

1. Add **Initialize variable**:
   - Name: `varFolderNames`
   - Type: Array
   - Value:
   ```json
   [
     "01-Identity",
     "02-Income",
     "03-Property",
     "04-Bank-Statements",
     "05-Fact-Find",
     "06-Terms-of-Business",
     "07-Illustrations",
     "08-DIP",
     "09-Application",
     "10-Valuation",
     "11-Offer",
     "12-Legal",
     "13-Completion"
   ]
   ```

2. Add **Initialize variable**:
   - Name: `varShareLinks`
   - Type: Object
   - Value: `{}`

### Step 4 — Create the Top-Level Case Folder

1. Add **Send an HTTP request to SharePoint**:
   - Method: POST
   - Uri: `_api/web/folders`
   - Headers: `{ "Accept": "application/json;odata=nometadata", "Content-Type": "application/json" }`
   - Body:
   ```json
   {
     "ServerRelativeUrl": "/sites/lending/Case Documents/@{variables('varCaseID')}"
   }
   ```

   > **Alternative:** Use the **Create new folder** action with path: `Case Documents/@{variables('varCaseID')}`

### Step 5 — Create All 13 Subfolders

1. Add **Apply to each** on `varFolderNames`
2. Inside the loop, add **Create new folder**:
   - Site: `https://[tenant].sharepoint.com/sites/lending`
   - List or Library: `Case Documents`
   - Folder Path: `@{variables('varCaseID')}/@{items('Apply_to_each')}`

### Step 6 — Generate Guest Share Links for Folders 01–04

After the Apply to each completes, create share links for the first 4 folders only:

1. Add **Apply to each** on `createArray('01-Identity', '02-Income', '03-Property', '04-Bank-Statements')`
2. Inside the loop:
   - **Send an HTTP request to SharePoint**:
     - Method: POST
     - Uri: `_api/web/GetFolderByServerRelativeUrl('/sites/lending/Case Documents/@{variables('varCaseID')}/@{items('Apply_to_each_2')}')/ShareLink`
     - Body:
     ```json
     {
       "request": {
         "createLink": true,
         "settings": {
           "linkKind": 5,
           "role": 2,
           "allowAnonymousAccess": false
         }
       }
     }
     ```
   - **Compose** — extract the share URL from the response

   > `linkKind: 5` = Organization link, `role: 2` = Edit/Contribute. Adjust `linkKind` for guest access (use `2` for anonymous or specific people sharing).

### Step 7 — Update Case Record with Folder Links

1. Add **Update item** (SharePoint):
   - List: `Specialist Lending Cases`
   - Id: `triggerOutputs()?['body/ID']`
   - Add link URLs to the case record (consider adding Hyperlink columns for each folder share link, or store as a combined text field)

**Recommended additional columns on Cases list:**

| Column Name | Type | Purpose |
|---|---|---|
| FolderLink01Identity | Hyperlink | Guest link to 01-Identity folder |
| FolderLink02Income | Hyperlink | Guest link to 02-Income folder |
| FolderLink03Property | Hyperlink | Guest link to 03-Property folder |
| FolderLink04BankStatements | Hyperlink | Guest link to 04-Bank-Statements folder |
| CaseFolderPath | Single line of text | Full path: `/sites/lending/Case Documents/{CaseID}` |

### Step 8 — Send Welcome Email

At the end of this flow (after folder links are generated), send the welcome email. See [task-10-flow-welcome-email.md](task-10-flow-welcome-email.md) for email template.

1. Add **Send an email (V2)** (Office 365 Outlook):
   - To: Customer email from Case record (`Applicant1Email`)
   - Subject: `Your Lending Application — @{variables('varCaseID')}`
   - Body: HTML template (see Task P1-10)

### Step 9 — Test

1. Create a new Case item
2. Wait for all flows to complete (up to 90 seconds)
3. Verify:
   - [ ] Case folder exists: `Case Documents/{CaseID}/`
   - [ ] All 13 subfolders are present
   - [ ] Guest share links work for folders 01–04 (test in incognito)
   - [ ] Customer can upload files to shared folders
   - [ ] Customer cannot access folders 05–13
   - [ ] Welcome email received with correct links

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Folder creation fails with 404 | Ensure the Case Documents library exists and the server-relative URL is correct |
| Share links require M365 login | Check tenant-level and site-level guest sharing settings |
| Apply to each runs slowly | SharePoint throttling — add Concurrency Control = 1 on the Apply to each |
| Welcome email not received | Check spam folder; verify Applicant1Email is populated on the Case record |
