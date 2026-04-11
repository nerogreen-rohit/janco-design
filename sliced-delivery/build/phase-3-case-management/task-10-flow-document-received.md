# Task P3-10: Flow 8 — Document Received Notification

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a system, when a customer uploads a file I need to update the Document Request status and notify the case owner so that document receipt is tracked  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Document library with case folders (P1-05), Document Requests list (P3-03), Cases list (P1-01)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 8 — Document Received Notification`
3. Trigger: **When a file is created (properties only)** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. Library Name: `Case Documents`
6. Folder: `/` (root — monitors all subfolders)

### Step 2 — Extract Case ID and Folder Category from File Path

Parse the file path to identify the case and document category.

> **Important:** The library name is `Case Documents` (with space). SharePoint encodes this as `Case%20Documents` in URLs but uses `Case Documents` in server-relative paths. Folder names from Phase 1 Flow 5 are: `01-Identity`, `02-Income`, `03-Property`, `04-Bank-Statements`.

```
// File path format: /sites/lending/Case Documents/{CaseID}/{FolderName}/{FileName}
// Example: /sites/lending/Case Documents/BTL-2026-0001/01-Identity/passport-scan.pdf

// Extract CaseID and FolderName from the path
```

1. Add **Compose** — `FilePath`: `triggerOutputs()?['body/{Path}']`
2. Add **Compose** — `PathSegments`: `split(outputs('FilePath'), '/')`
3. Add **Compose** — `CaseID`: `outputs('PathSegments')[4]` ← adjust index: segments are `['', 'sites', 'lending', 'Case Documents', '{CaseID}', '{FolderName}', '{FileName}']`
4. Add **Compose** — `FolderName`: `outputs('PathSegments')[5]`

> **Tip:** The exact path segment index depends on how your trigger returns the path. Test with a sample file upload and inspect `triggerOutputs()?['body/{Path}']` in the flow run history to confirm the correct indices before going live.

### Step 3 — Map Folder to Document Category

| Folder Name | Document Category |
|---|---|
| `01-Identity` | Identity |
| `02-Income` | Income |
| `03-Property` | Property |
| `04-Bank-Statements` | Bank Statements, Other |

> **Note:** Folders 05–13 are internal staff folders — uploads to these do not trigger document request matching.

Add a **Condition**: Check if FolderName starts with "01", "02", "03", or "04"
- If **No** → Terminate (internal folder upload — no action needed)

### Step 4 — Match to Document Request

1. Add **Get items** (SharePoint):
   - List: `Document Requests`
   - Filter Query: `CaseID eq '@{outputs('CaseID')}' and Status eq 'Requested'`
   - Order By: `RequestedDate asc`
   - Top Count: 100

2. Add **Filter array** to match by category:

```
@or(
    and(equals(outputs('FolderName'), '01-Identity'),
        equals(item()?['DocTypeCategory'], 'Identity')),
    and(equals(outputs('FolderName'), '02-Income'),
        equals(item()?['DocTypeCategory'], 'Income')),
    and(equals(outputs('FolderName'), '03-Property'),
        equals(item()?['DocTypeCategory'], 'Property')),
    and(equals(outputs('FolderName'), '04-Bank-Statements'),
        or(equals(item()?['DocTypeCategory'], 'Other'),
           equals(item()?['DocTypeCategory'], 'Lender'),
           equals(item()?['DocTypeCategory'], 'Legal')))
)
```

### Step 5 — Update Matching Document Request

If matching requests are found, update the first match:

1. Add a **Condition**: `length(body('Filter_array'))` is greater than 0
2. If **Yes**:
   - **Update item** (SharePoint):
     - List: `Document Requests`
     - Id: `first(body('Filter_array'))?['ID']`
     - Status: `Received`
     - SharePointFileURL: `triggerOutputs()?['body/{Link}']`

### Step 6 — Get the Case Owner

1. Add **Get items** (SharePoint):
   - List: `Specialist Lending Cases`
   - Filter Query: `CaseID eq '@{outputs('CaseID')}'`
   - Top Count: `1`

   > **Note:** Use **Get items** (plural) with Top Count = 1, not **Get item** (singular). The "Get item" action requires a list item ID and does not support OData filter queries. Reference the result as `first(outputs('Get_items_-_Case')?['body/value'])`.

### Step 7 — Notify Case Owner via Teams

1. Add **Post message in a chat or channel** (Microsoft Teams):
   - Post as: Flow bot
   - Post in: Chat with Flow bot
   - Recipient: Case owner email from Step 6

   **Message:**
   ```
   📄 Document Received — @{outputs('CaseID')}

   File: @{triggerOutputs()?['body/{FilenameWithExtension}']}
   Folder: @{outputs('FolderName')}
   Uploaded: @{formatDateTime(utcNow(), 'dd MMM yyyy HH:mm')}

   @{if(greater(length(body('Filter_array')), 0),
       concat('✅ Matched to request: ', first(body('Filter_array'))?['DocTypeName']),
       '⚠️ No matching document request found — manual review needed'
   )}
   ```

### Step 8 — Error Handling

1. Wrap Steps 2–7 in a **Scope**
2. Add a parallel failure branch with Teams notification to admin

### Step 9 — Test

1. Create a Document Request for a test case (e.g. CaseID = BTL-2026-0001, DocType = Passport)
2. Upload a file named `passport-scan.pdf` to the case's `01-Identity` folder
3. Wait 30–60 seconds
4. Verify:
   - [ ] Document Request status changes from "Requested" to "Received"
   - [ ] SharePointFileURL is populated with the uploaded file link
   - [ ] Case owner receives Teams notification
   - [ ] Notification includes the matched document request name
5. Upload a file to a folder with no matching request
6. Verify:
   - [ ] Case owner receives notification with "No matching document request" warning
   - [ ] No Document Request item is erroneously updated

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Flow doesn't trigger on upload | Verify the trigger monitors the correct library and folder path |
| CaseID extraction fails | Check the path segment index — use a test file path to verify the split logic |
| No document request match found | Verify DocTypeCategory on the Document Request matches the folder-to-category mapping |
| Multiple requests matched | The flow updates only the first (oldest) matching request — review if multiple requests exist for the same category |
| Internal folder uploads trigger flow | Ensure the condition in Step 3 filters out folders 05–13 |
