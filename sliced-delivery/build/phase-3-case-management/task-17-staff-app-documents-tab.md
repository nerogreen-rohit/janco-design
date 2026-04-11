# Task P3-17: Staff App — Documents Tab

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a staff member, I want a Documents tab showing document requests and statuses so that I can track and manage case documents  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Document Requests list (P3-03), Document Types list (P3-02), Document library (P1-05), Lenders list (P3-06), Flow 7 (P3-09), Flow 12 (P3-12), Case Workspace screen (P3-16)

---

## Step-by-Step Guide

### Step 1 — Add the Documents Tab

1. In the Case Workspace screen, add the Documents tab content area
2. This tab is visible when the user selects "Documents" from the tab navigation

### Step 2 — Screen Layout

```
╔══════════════════════════════════════════════════════════════════════════╗
║  DOCUMENTS — BTL-2026-0001                                              ║
╠══════════════════════════════════════════════════════════════════════════╣
║                                                                         ║
║  ── DOCUMENT REQUESTS ────────────────────────────────────────────────  ║
║                                                                         ║
║  [+ Request Document]                          [📧 Email Lender]        ║
║                                                                         ║
║  Document             │ Category │ Applicant │ Status    │ Due Date     ║
║  ─────────────────────┼──────────┼───────────┼───────────┼────────────  ║
║  Passport             │ Identity │ App 1     │ ✅ Received│ 20 Apr      ║
║  Driving Licence      │ Identity │ App 1     │ ✅ Approved│ 20 Apr      ║
║  Last 3 Months Payslips│ Income  │ App 1     │ 📨 Requested│ 22 Apr    ║
║  Utility Bill (3 mon) │ Identity │ Both      │ 📨 Requested│ 22 Apr    ║
║  Mortgage Statement   │ Property │ N/A       │ ❌ Rejected│ 18 Apr     ║
║                                                                         ║
║  ── CASE FOLDERS ─────────────────────────────────────────────────────  ║
║                                                                         ║
║  📁 01-identification  (3 files)                                        ║
║  📁 02-property        (0 files)                                        ║
║  📁 03-legal-and-lender (1 file)                                        ║
║  📁 04-other           (0 files)                                        ║
║  📁 05-fact-find       (1 file)     [staff only]                        ║
║  ...                                                                    ║
╚══════════════════════════════════════════════════════════════════════════╝
```

### Step 3 — Document Requests Gallery

1. Add a **Gallery** control (vertical, blank template)
2. Set **Items**:

```
Sort(
    Filter(
        'Document Requests',
        CaseID = varCurrentCase.CaseID
    ),
    DocTypeCategory,
    SortOrder.Ascending
)
```

3. Inside the gallery row template, add labels:

| Label | Text Property |
|---|---|
| Document Name | `ThisItem.DocTypeName` |
| Category | `ThisItem.DocTypeCategory` |
| Applicant | `ThisItem.ApplicantNumber.Value` |
| Status | `ThisItem.Status.Value` |
| Due Date | `Text(ThisItem.DueDate, "dd MMM")` |

### Step 4 — Status Badge Styling

Set the status label's color dynamically:

```
Switch(
    ThisItem.Status.Value,
    "Requested", RGBA(0, 120, 212, 1),     // Blue
    "Received",  RGBA(40, 167, 69, 1),     // Green
    "Approved",  RGBA(40, 167, 69, 1),     // Green
    "Rejected",  RGBA(220, 53, 69, 1),     // Red
    "Not Required", RGBA(128, 128, 128, 1) // Gray
)
```

**Status icons:**
```
Switch(
    ThisItem.Status.Value,
    "Requested",    "📨",
    "Received",     "✅",
    "Approved",     "✅",
    "Rejected",     "❌",
    "Not Required", "➖"
)
```

### Step 5 — Request Document Button

**[+ Request Document]** button opens a form:

```
╔════════════════════════════════════════════╗
║  REQUEST DOCUMENT                          ║
║                                            ║
║  Document Type: [Passport ▼]               ║
║  Applicant:     [Applicant 1 ▼]            ║
║  Due Date:      [________]                 ║
║  Notes:         [________________________] ║
║                                            ║
║  [Cancel]                   [Request →]    ║
╚════════════════════════════════════════════╝
```

**Document Type dropdown Items:**
```
Filter(
    'Document Types',
    IsActive = true
)
```

**[Request →] OnSelect:**
```
Patch(
    'Document Requests',
    Defaults('Document Requests'),
    {
        Title: varCurrentCase.CaseID & "-" & ddDocType.Selected.Title,
        CaseID: varCurrentCase.CaseID,
        CaseName: varCurrentCase.Title,
        DocTypeID: ddDocType.Selected,
        DocTypeName: ddDocType.Selected.Title,
        DocTypeCategory: ddDocType.Selected.Category.Value,
        ApplicantNumber: {Value: ddApplicant.Selected.Value},
        RequestedBy: User(),
        RequestedDate: Today(),
        DueDate: dpDueDate.SelectedDate,
        Status: {Value: "Requested"},
        CustomerNotes: "",
        StaffNotes: txtRequestNotes.Text,
        RetryCount: 0
    }
);

// Flow 7 triggers automatically to email the customer
Notify("Document requested. Customer will be emailed.", NotificationType.Success);
UpdateContext({showRequestForm: false})
```

### Step 6 — Email Lender Button

**[📧 Email Lender]** button opens a dialog:

```
╔══════════════════════════════════════════════╗
║  EMAIL DOCUMENTS TO LENDER                    ║
║                                               ║
║  Lender: [Select Lender ▼]                    ║
║                                               ║
║  Select files to attach:                      ║
║  ☑ passport-scan.pdf          (01-ident)      ║
║  ☑ payslips-march.pdf         (01-ident)      ║
║  ☐ tenancy-agreement.pdf      (02-property)   ║
║  ☑ valuation-report.pdf       (03-legal)      ║
║                                               ║
║  Cover Note: [____________________________]   ║
║                                               ║
║  [Cancel]                   [Send to Lender →]║
╚══════════════════════════════════════════════╝
```

**Lender dropdown Items:**
```
Filter(Lenders, IsActive = true)
```

**[Send to Lender →] OnSelect:**
```
// Collect selected file URLs into a JSON array
Set(
    varSelectedFiles,
    JSON(
        ForAll(
            Filter(galFiles.AllItems, chkSelected.Value = true),
            ThisRecord.FileURL
        )
    )
);

// Trigger Flow 12
Flow12_EmailDocsToLender.Run(
    varCurrentCase.CaseID,
    ddLender.Selected.ID,
    varSelectedFiles,
    txtCoverNote.Text
);

Notify("Documents sent to " & ddLender.Selected.Title, NotificationType.Success);
UpdateContext({showEmailLenderDialog: false})
```

### Step 7 — Case Folders Panel

Display the document library folders for this case:

1. Use a **Gallery** bound to a collection of folder names
2. For each folder, show the name and file count
3. Clicking a folder opens the SharePoint folder in a new browser tab:

```
// OnSelect for folder item
Launch(
    "https://[tenant].sharepoint.com/sites/lending/CaseDocuments/" &
    varCurrentCase.CaseID & "/" & ThisItem.FolderName
)
```

### Step 8 — Test

1. Open a test case → Documents tab
2. Test document requests:
   - [ ] [+ Request Document] shows the form with document type dropdown
   - [ ] Creating a request adds an item to the Document Requests gallery
   - [ ] Flow 7 sends email to customer (check inbox)
   - [ ] Status badges show correct colors and icons
3. Test Email Lender:
   - [ ] [📧 Email Lender] shows the lender selection dialog
   - [ ] Files from the case folder are listed with checkboxes
   - [ ] Selecting files and clicking Send triggers Flow 12
   - [ ] Email arrives at lender's submission address
4. Test folder panel:
   - [ ] All case folders are displayed
   - [ ] Clicking a folder opens it in SharePoint

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Document Requests gallery empty | Verify CaseID filter matches the current case; check list name in Items |
| Document Type dropdown empty | Verify Document Types list has items with IsActive = Yes |
| Email Lender shows "no submission email" | Update the selected lender's SubmissionEmail in the Lenders list |
| File list not loading | Ensure the document library connector is added to the app |
| Flow 7 not triggered | Verify the Document Request item is created in the correct list |
