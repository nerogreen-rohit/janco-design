# Task P3-03: Create the Document Requests List

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a developer, I need to create the Document Requests list so that staff can track requested and received documents per case  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** Cases list (P1-01), Document Types list (P3-02)

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Open your SharePoint site: `https://[tenant].sharepoint.com/sites/lending`
2. Click **⚙️ Settings** → **Site contents**
3. Click **+ New** → **List** → **Blank list**
4. Name: `Document Requests`
5. Description: `Tracks individual document requests per case — one item per requested document`
6. Click **Create**

### Step 2 — Add Columns

Add each column in order. Use **+ Add column** or **List Settings → Create column**.

#### Core Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CaseID | Single line of text | Yes | FK to Cases list |
| CaseName | Single line of text | No | Denormalized |
| DocTypeID | Lookup | Yes | Lookup list: `Document Types`, Lookup column: `Title` |
| DocTypeName | Single line of text | No | Denormalized from Document Types |
| DocTypeCategory | Single line of text | No | Denormalized from Document Types |
| ApplicantNumber | Choice | No | Choices: `Applicant 1`, `Applicant 2`, `Both`, `N/A` |

#### Tracking Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| RequestedBy | Person or Group | Yes | Allow selection of people only |
| RequestedDate | Date and Time | Yes | Date only |
| DueDate | Date and Time | No | Date only |
| Status | Choice | Yes | Choices: `Requested`, `Received`, `Approved`, `Rejected`, `Not Required`. Default: `Requested` |

#### Notes & File Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CustomerNotes | Multiple lines of text | No | Plain text — notes from customer on upload |
| StaffNotes | Multiple lines of text | No | Plain text — internal review notes |
| SharePointFileURL | Hyperlink | No | Link to the uploaded file in document library |
| RejectionReason | Multiple lines of text | No | Required when Status = Rejected |
| RetryCount | Number | No | 0 decimal places, Default: 0 |

### Step 3 — Rename the Title Column

1. Go to **List Settings** → click **Title** column
2. Set **Require that this column contains information** = No
3. Title will be auto-set to `{CaseID}-{DocType}` by the flow or Staff App

### Step 4 — Create Views

#### Active Requests View

1. Go to **List Settings** → **Views** → **Create view**
2. Name: `Active Requests`
3. Filter: `Status is equal to Requested OR Status is equal to Rejected`
4. Columns: `Title`, `CaseID`, `DocTypeName`, `DocTypeCategory`, `ApplicantNumber`, `Status`, `DueDate`, `RequestedBy`
5. Sort by: `DueDate` (ascending)

#### By Case View

1. Create another view named `By Case`
2. Columns: `Title`, `DocTypeName`, `Status`, `ApplicantNumber`, `DueDate`, `SharePointFileURL`
3. Group by: `CaseID`
4. Sort by: `DocTypeCategory` (ascending)

### Step 5 — Verify

- [ ] List appears in Site Contents as "Document Requests"
- [ ] All columns are present with correct types
- [ ] DocTypeID lookup points to the Document Types list
- [ ] Status choice has 5 options with default "Requested"
- [ ] ApplicantNumber choice has 4 options
- [ ] RetryCount defaults to 0
- [ ] Both views are created and configured correctly

---

## Importable Content

See [`imports/document-requests-columns.csv`](imports/document-requests-columns.csv) for the complete column definition.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| DocTypeID lookup not available | Create the Document Types list first (P3-02), then return to add this column |
| Status default not working | Edit the column in List Settings and set the default value explicitly |
| SharePointFileURL not clickable | Ensure column type is Hyperlink (not Single line of text) |
