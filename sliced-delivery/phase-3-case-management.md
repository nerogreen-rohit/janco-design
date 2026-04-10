# Phase 3 — Case Management & Lifecycle

**Goal:** Full 14-stage lifecycle, document requests, messaging, SLA alerts, and email-to-lender.  
**Estimated duration:** 2 weeks (Weeks 4–5)  
**Depends on:** Phase 1 (Cases list, Fact-Find, Counters, document library) and Phase 2 (Leads list, conversion flow)  
**Business value:** Staff have full case management — track every case from enquiry to completion with audit trail, SLA monitoring, document tracking, and direct lender email.

---

## What This Phase Delivers

At the end of Phase 3, the following is operational:

1. **Full 14-stage lifecycle** is active. Staff can advance a case through all stages via the Case Workspace Overview tab.
2. Every stage change is **automatically logged** to an immutable Stage History record (audit trail).
3. The customer receives an **automated email notification** when their case moves to a customer-visible stage.
4. Staff can **request documents** from the customer; automated emails are sent; document receipt is tracked.
5. A **Tasks list** enables staff to create internal tasks linked to cases.
6. A **Messages list** enables threaded case-level communication between staff and customers.
7. The **SLA Reminder flow** runs daily at 08:00 and sends Teams alerts for overdue or at-risk stages.
8. Staff can **email documents directly to the lender** from the Documents tab.
9. Staff can view the complete **stage timeline** in the Case Workspace Timeline tab.
10. The **Lenders list** is populated with the lender panel.

---

## SharePoint Lists Required (new in Phase 3)

### 5.5 Case Stage History List

Immutable audit log — one item per stage transition per case. Created by Flow 6 only; never edited by staff.

| Column | Type | Notes |
|---|---|---|
| Title | Single line | Set to `{CaseID}-Stage{N}` by flow |
| CaseID | Single line | FK to Cases list |
| CaseName | Single line | Denormalized |
| StageNumber | Number | Snapshot at time of transition |
| StageName | Single line | **Text snapshot — NOT a lookup** (preserves audit integrity) |
| PreviousStageNumber | Number | |
| PreviousStageName | Single line | Text snapshot |
| ChangedBy | Person | User who triggered stage change |
| ChangedDate | Date/Time | |
| TimeInPreviousStageHours | Number | Duration in previous stage (calculated by flow) |
| Notes | Multiple lines | Notes recorded at time of transition |
| IsSkipped | Yes/No | True if stage was skipped |

> **Design note:** Stage History records are permanent. The list does not expose an edit form in the Staff App. Staff read history in the Timeline tab only.  
> **Source:** `05-sharepoint-list-schemas.md` lines 319–338; `17-key-design-decisions.md` §17.9

---

### 5.6 Document Types List

Reference/configuration list defining document categories.

| Column | Type | Notes |
|---|---|---|
| Title | Single line | e.g. "Passport" |
| Category | Choice | Identity, Income, Property, Legal, Lender, Other |
| ApplicableTo | Choice (multi) | All Products, BTL, Bridging, Commercial, Dev Finance, Refurb, Auction |
| Description | Multiple lines | Shown to customer when requesting |
| IsRequired | Yes/No | Whether typically mandatory |
| SortOrder | Number | |
| IsActive | Yes/No | |

Initial document types to configure:

| Category | Document Types |
|---|---|
| Identity | Passport, Driving Licence, Utility Bill (3 months), Council Tax Bill |
| Income | Last 3 Months Payslips, Last 2 Years SA302 + Tax Overview, Last 2 Years Company Accounts, 6 Months Business Bank Statements, 6 Months Personal Bank Statements |
| Property | Mortgage Statement, Buildings Insurance, Tenancy Agreement, EPC Certificate, Title Register (OS1) |
| Legal | Memorandum & Articles, Certificate of Incorporation, Schedule of Works |
| Lender | ESIS/KFI, DIP Confirmation, Formal Loan Offer, Valuation Report |
| Other | Power of Attorney, Proof of Deposit, Auction Legal Pack, Exit Strategy Evidence |

> **Source:** `05-sharepoint-list-schemas.md` lines 342–365

---

### 5.7 Document Requests List

One item per requested document per case.

| Column | Type | Notes |
|---|---|---|
| Title | Single line | Set to `{CaseID}-{DocType}` by flow |
| CaseID | Single line | FK to Cases list |
| CaseName | Single line | Denormalized |
| DocTypeID | Lookup → Document Types | |
| DocTypeName | Single line | Denormalized |
| DocTypeCategory | Single line | Denormalized |
| ApplicantNumber | Choice | Applicant 1, Applicant 2, Both, N/A |
| RequestedBy | Person | |
| RequestedDate | Date | |
| DueDate | Date | |
| Status | Choice | Requested, Received, Approved, Rejected, Not Required |
| CustomerNotes | Multiple lines | Notes from customer on upload |
| StaffNotes | Multiple lines | Internal review notes |
| SharePointFileURL | Hyperlink | Link to uploaded file |
| RejectionReason | Multiple lines | If Status = Rejected |
| RetryCount | Number | Times re-requested |

> **Source:** `05-sharepoint-list-schemas.md` lines 369–391

---

### 5.8 Tasks List

| Column | Type | Notes |
|---|---|---|
| Title | Single line | Task description |
| CaseID | Single line | FK (optional — some tasks may be general) |
| CaseName | Single line | Denormalized |
| AssignedTo | Person | |
| CreatedBy | Person | |
| DueDate | Date | |
| Priority | Choice | High, Medium, Low |
| Status | Choice | Not Started, In Progress, Completed, Cancelled |
| Category | Choice | Chase Customer, Chase Lender, Chase Solicitor, Internal, Admin, Other |
| Notes | Multiple lines | |
| CompletedDate | Date | |
| IsOverdue | Yes/No | Set by SLA flow |

> **Source:** `05-sharepoint-list-schemas.md` lines 394–412

---

### 5.9 Messages List

Case-level threaded communication between staff and customers.

| Column | Type | Notes |
|---|---|---|
| Title | Single line | Set to `{CaseID}-{timestamp}` by flow |
| CaseID | Single line | FK to Cases list |
| CaseName | Single line | Denormalized |
| MessageBody | Multiple lines | Message content |
| SentBy | Person | Sender (staff member) |
| SentByName | Single line | Denormalized display name |
| Direction | Choice | **Advisor → Customer**, **Customer → Advisor**, Internal |
| IsRead | Yes/No | Customer has read |
| IsReadByStaff | Yes/No | Staff has read |
| SentDate | Date/Time | |
| AttachmentURL | Hyperlink | Optional document link |
| IsSystemMessage | Yes/No | Auto-generated status notification |

> **Design note:** Direction values use **Advisor** (not "Case Owner" or "Staff") in all customer-facing contexts.  
> **Source:** `05-sharepoint-list-schemas.md` lines 417–433; `17-key-design-decisions.md` §17.7

---

### 5.12 Lenders List

| Column | Type | Notes |
|---|---|---|
| Title | Single line | Lender name |
| LenderType | Choice | High Street, Specialist, Challenger, Private, Bridging, Development |
| ProductsOffered | Choice (multi) | BTL, Bridging, Commercial, Dev Finance, Refurb, Auction |
| ContactName | Single line | BDM name |
| ContactEmail | Single line | |
| ContactPhone | Single line | |
| SubmissionEmail | Single line | **Used by Flow 12 (Email Docs to Lender)** |
| MaxLTV | Number | |
| MinLoanAmount | Currency | |
| MaxLoanAmount | Currency | |
| TypicalProcFeePercent | Number | |
| Notes | Multiple lines | |
| IsActive | Yes/No | |
| WebsiteURL | Hyperlink | |

> **Source:** `05-sharepoint-list-schemas.md` lines 485–504

---

### Stage Configuration List — Full 14 rows (extending Phase 1)

Add the remaining 11 stages (stages 4–14) to the Stage Configuration list created in Phase 1:

| # | Title | ShortName | CustomerFriendlyName | StageNumber | VisibleToCustomer | DefaultSLADays | CanSkip |
|---|---|---|---|---|---|---|---|
| 4 | Product Research | PROD | Under Review | 4 | **No** | 3 | No |
| 5 | Present Options & Agree | OPT | Options Presented | 5 | Yes | 3 | Yes |
| 6 | Produce Illustrations | ILLUS | Illustrations Prepared | 6 | Yes | 3 | Yes |
| 7 | Decision in Principle | DIP | Initial Decision | 7 | Yes | 5 | Yes |
| 8 | Full Application | APP | Application Submitted | 8 | Yes | 7 | No |
| 9 | Valuation | VAL | Valuation Arranged | 9 | Yes | 10 | No |
| 10 | Underwriting | UW | Lender Review | 10 | Yes | 14 | No |
| 11 | Loan Offer | OFFER | Offer Issued | 11 | Yes | 5 | No |
| 12 | Legals | LEGAL | Legal Work | 12 | Yes | 21 | No |
| 13 | Completion | COMP | Funds Released | 13 | Yes | 1 | No |
| 14 | Post-Completion & Logging | POST | Completed | 14 | Yes | 3 | No |

> **Source:** `05-sharepoint-list-schemas.md` lines 131–148; `04-lifecycle-stages.md`

---

## Power Automate Flows Required

### Flow 6 — Log Stage Change

| Attribute | Value |
|---|---|
| Trigger | When `CurrentStageID` changes on a Case item |
| Action | 1. Read Stage Config item (via LookupID) to get StageName, CustomerFriendlyName, VisibleToCustomer; 2. Write Stage History record (text snapshot of stage name — NOT a lookup); 3. Calculate `TimeInPreviousStageHours` from `StageChangedDate`; 4. If `VisibleToCustomer = Yes`, trigger Flow 13 |

> **Source:** `10-automation-summary.md` lines 19 and 37–42

---

### Flow 7 — Send Document Request Email

| Attribute | Value |
|---|---|
| Trigger | When Document Request item created |
| Action | Send formatted email to customer with: document name, description, upload folder link, due date |

> **Source:** `10-automation-summary.md` line 20

---

### Flow 8 — Document Received Notification

| Attribute | Value |
|---|---|
| Trigger | File uploaded to Case Documents folders 01–04 |
| Action | Create or update Document Request item as "Received"; notify case owner |

> **Source:** `10-automation-summary.md` line 21

---

### Flow 9 — SLA Reminder (Scheduled)

| Attribute | Value |
|---|---|
| Trigger | Scheduled — daily at 08:00 |
| Action | Check all active cases for SLA breaches (compare `StageChangedDate + DefaultSLADays` vs today); send Teams notification to case owner for overdue or at-risk stages; update `IsOverdue` on Tasks |

> **Source:** `10-automation-summary.md` line 22

---

### Flow 12 — Email Docs to Lender

| Attribute | Value |
|---|---|
| Trigger | Button — Staff App Documents tab |
| Action | Compile selected files from case folder; send formatted email to `LenderID.SubmissionEmail` with attachments; log event in Stage History and add a Message record |

> **Source:** `10-automation-summary.md` line 25

---

### Flow 13 — Customer Status Update

| Attribute | Value |
|---|---|
| Trigger | When `CurrentStageID` changes (only when `VisibleToCustomer = Yes` on Stage Config) |
| Action | Send customer email with `CustomerFriendlyName` of new stage, portal link, and action required (if any) |

> **Source:** `10-automation-summary.md` line 26

---

### Flow 14 — Message Notification

| Attribute | Value |
|---|---|
| Trigger | When Message item created with `Direction = "Advisor → Customer"` |
| Action | Send email notification to customer that a new message is waiting in their portal |

> **Source:** `10-automation-summary.md` line 27

---

## Staff App Screens Required

### Case List Screen

Full filterable/searchable case list (Stage, Product, Owner, Status filters).

> **Source:** `07-screen-wireframes-internal.md` lines 76–99

---

### Case Workspace — Overview Tab

Displays: Case Header (CaseID, applicant, product, status, lender, loan amount, LTV, property), progress bar (stages 1–14), current stage name and notes, stage date and SLA due, [Advance to next stage] and [Mark as On Hold] buttons, product-specific fields panel.

> **Source:** `07-screen-wireframes-internal.md` lines 104–144

---

### Case Workspace — Documents Tab

Shows Document Requests list for the case (status per document), document library folders panel, [+ Request Document] and [📧 Email Lender] buttons.

> **Source:** `07-screen-wireframes-internal.md` lines 189–219

---

### Case Workspace — Messages Tab

Threaded message view for the case, with Direction filter (All / Advisor→Customer / Customer→Advisor / Internal), new message compose area, [Send Message] button.

> **Source:** `07-screen-wireframes-internal.md` lines 222–259

---

### Case Workspace — Timeline Tab

Read-only chronological stage history log. Each entry shows stage name, date entered, who triggered it, duration in previous stage, and any notes. Skip events are clearly labelled.

> **Source:** `07-screen-wireframes-internal.md` lines 264–299

---

### Admin Screen

Configuration screens for:
- Stage Configuration list (view/edit stages)
- Document Types list (add/edit document types)

> **Source:** `16-implementation-timeline.md` line 79

---

## Stage Applicability by Product

When advancing stages, optional/skippable stages are shown but can be bypassed. The `CanSkip` flag on Stage Configuration drives whether a stage can be skipped.

| Stage | BTL | Bridge | Commercial | Dev Finance | Refurb | Auction |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| 5. Present Options | ✅ | ⚪ | ✅ | ✅ | ✅ | ⚪ |
| 6. Illustrations | ✅ | ⚪ | ✅ | ⚪ | ⚪ | ⚪ |
| 7. DIP | ✅ | ⚪ | ✅ | ⚪ | ⚪ | ⚪ |

✅ = Required ⚪ = Optional / Skip allowed

> **Source:** `04-lifecycle-stages.md` lines 27–45

---

## Phase 3 Acceptance Criteria

| # | Criterion |
|---|---|
| 1 | All 14 stages are present in Stage Configuration list |
| 2 | Staff can advance a BTL case through all 14 stages |
| 3 | Each stage change creates an immutable Stage History record |
| 4 | Stage History cannot be edited by staff from the Staff App |
| 5 | Customer receives automated email when case moves to a VisibleToCustomer stage |
| 6 | Staff can create a document request; customer receives email with upload link |
| 7 | When customer uploads a file, Document Request status updates to "Received" and case owner is notified |
| 8 | Staff can send a message to the customer via the Messages tab |
| 9 | Customer receives email notification of new message |
| 10 | Daily SLA flow sends Teams alert for any case where the SLA deadline has passed |
| 11 | Staff can select documents and email them to the lender from the Documents tab |
| 12 | Optional stages (5, 6, 7 for Bridge/Auction) can be skipped with a reason recorded in Stage History |
| 13 | Timeline tab shows complete chronological stage history for each case |
