# Task P3-12: Flow 12 — Email Docs to Lender

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a staff member, I want to email selected case documents to the lender so that I can submit applications directly from the app  
**Priority:** 🟡 High  
**Estimated effort:** 2–3 hours  
**Depends on:** Cases list (P1-01), Lenders list (P3-06), Document library (P1-05), Case Stage History list (P3-01), Messages list (P3-05)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Instant cloud flow**
2. Name: `Flow 12 — Email Docs to Lender`
3. Trigger: **Manually trigger a flow** (PowerApps button)
4. Add the following input parameters:

| Parameter | Type | Description |
|---|---|---|
| CaseID | Text | The case reference |
| LenderID | Number | SharePoint item ID of the selected lender |
| FileURLs | Text | JSON array of selected file URLs |
| Notes | Text | Optional cover note from the advisor |

### Step 2 — Get Case and Lender Data

1. Add **Get items** (SharePoint) — Get the Case:
   - List: `Specialist Lending Cases`
   - Filter Query: `CaseID eq '@{triggerBody()['text_CaseID']}'`
   - Top Count: 1

2. Add **Get item** (SharePoint) — Get the Lender:
   - List: `Lenders`
   - Id: `triggerBody()['number_LenderID']`

3. Add **Compose** — `SubmissionEmail`: `outputs('Get_Lender')?['body/SubmissionEmail']`

### Step 3 — Validate Submission Email

1. Add a **Condition**: `empty(outputs('SubmissionEmail'))` is equal to `true`
2. If **Yes**:
   - **Respond to a PowerApp or flow**: Return error message
     ```
     No submission email configured for this lender. Please update the Lenders list.
     ```
   - **Terminate** with status "Cancelled"

### Step 4 — Parse File URLs and Get File Content

1. Add **Parse JSON**:
   - Content: `triggerBody()['text_FileURLs']`
   - Schema:
     ```json
     {
         "type": "array",
         "items": { "type": "string" }
     }
     ```

2. Add **Apply to each** on the parsed array:
   - Inside: **Get file content** (SharePoint)
     - Site Address: `https://[tenant].sharepoint.com/sites/lending`
     - File Identifier: `items('Apply_to_each_File')`
   - **Compose** — append to an attachments array

### Step 5 — Send Email to Lender

1. Add **Send an email (V2)** (Office 365 Outlook):
   - **To:** `@{outputs('SubmissionEmail')}`
   - **Subject:** `Application Submission — @{triggerBody()['text_CaseID']} — @{first(outputs('Get_Case')?['body/value'])?['Applicant1FirstName']} @{first(outputs('Get_Case')?['body/value'])?['Applicant1LastName']}`
   - **Body (HTML):**

```html
<div style="font-family: Arial, sans-serif;">
    <p>Dear @{outputs('Get_Lender')?['body/ContactName']},</p>
    <p>Please find attached the application documents for the following case:</p>

    <table style="border-collapse: collapse; margin: 15px 0;">
        <tr>
            <td style="padding: 8px; border: 1px solid #ddd;"><strong>Case Reference</strong></td>
            <td style="padding: 8px; border: 1px solid #ddd;">@{triggerBody()['text_CaseID']}</td>
        </tr>
        <tr>
            <td style="padding: 8px; border: 1px solid #ddd;"><strong>Applicant</strong></td>
            <td style="padding: 8px; border: 1px solid #ddd;">
                @{first(outputs('Get_Case')?['body/value'])?['Applicant1FirstName']}
                @{first(outputs('Get_Case')?['body/value'])?['Applicant1LastName']}
            </td>
        </tr>
        <tr>
            <td style="padding: 8px; border: 1px solid #ddd;"><strong>Product</strong></td>
            <td style="padding: 8px; border: 1px solid #ddd;">
                @{first(outputs('Get_Case')?['body/value'])?['ProductType/Value']}
            </td>
        </tr>
        <tr>
            <td style="padding: 8px; border: 1px solid #ddd;"><strong>Loan Amount</strong></td>
            <td style="padding: 8px; border: 1px solid #ddd;">
                £@{first(outputs('Get_Case')?['body/value'])?['LoanAmountRequired']}
            </td>
        </tr>
    </table>

    @{if(empty(triggerBody()['text_Notes']), '',
        concat('<p><strong>Advisor Notes:</strong><br>', triggerBody()['text_Notes'], '</p>')
    )}

    <p>Kind regards,<br>Janco Finance</p>
</div>
```

   - **Attachments:** Use the collected file content from Step 4

### Step 6 — Log in Stage History

1. Add **Create item** (SharePoint):
   - List: `Case Stage History`
   - Title: `@{triggerBody()['text_CaseID']}-LenderEmail`
   - CaseID: `@{triggerBody()['text_CaseID']}`
   - CaseName: `@{first(outputs('Get_Case')?['body/value'])?['Title']}`
   - StageNumber: `@{first(outputs('Get_Case')?['body/value'])?['CurrentStageNumber']}`
   - StageName: `@{first(outputs('Get_Case')?['body/value'])?['CurrentStageName']}`
   - ChangedBy: (current user)
   - ChangedDate: `utcNow()`
   - Notes: `Documents emailed to @{outputs('Get_Lender')?['body/Title']} at @{outputs('SubmissionEmail')}`

### Step 7 — Create Message Record

1. Add **Create item** (SharePoint):
   - List: `Messages`
   - Title: `@{triggerBody()['text_CaseID']}-@{formatDateTime(utcNow(), 'yyyyMMddHHmmss')}`
   - CaseID: `@{triggerBody()['text_CaseID']}`
   - CaseName: `@{first(outputs('Get_Case')?['body/value'])?['Title']}`
   - MessageBody: `Application documents emailed to @{outputs('Get_Lender')?['body/Title']} (@{outputs('SubmissionEmail')}). Files: @{triggerBody()['text_FileURLs']}`
   - Direction: `Internal`
   - SentDate: `utcNow()`
   - IsSystemMessage: `Yes`

### Step 8 — Return Success to PowerApps

1. Add **Respond to a PowerApp or flow**:
   - Output: `Documents sent successfully to @{outputs('Get_Lender')?['body/Title']} at @{outputs('SubmissionEmail')}`

### Step 9 — Test

1. Open a test case in the Staff App Documents tab
2. Select one or more files
3. Click **[📧 Email Lender]**
4. Select a lender from the dropdown
5. Verify:
   - [ ] Email arrives at the lender's submission email with attachments
   - [ ] Email subject includes CaseID and applicant name
   - [ ] Email body includes case details and advisor notes
   - [ ] A Stage History record is created with lender email details
   - [ ] A Messages record is created as Internal/System message
   - [ ] Staff App shows success notification

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| "No submission email configured" error | Update the Lenders list with a valid SubmissionEmail for the selected lender |
| Attachments missing from email | Verify file URLs are valid SharePoint paths; check Get file content action results |
| Email not received by lender | Check the SubmissionEmail address; verify email is not blocked by spam filters |
| Large files fail | SharePoint/Outlook attachment limit is ~25MB — consider using a sharing link instead for large files |
| Flow takes too long | Large files may cause timeouts — add concurrency control and consider chunking |
