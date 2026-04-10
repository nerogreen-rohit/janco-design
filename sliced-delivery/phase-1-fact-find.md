# Phase 1 — Fact-Find (First Delivery)

**Goal:** Customer can complete their fact-find online. Staff can create a case and review the fact-find in the app.  
**Estimated duration:** 2 weeks  
**Business value:** Immediate replacement of the Word-document-by-email fact-find process. Unblocks lender submissions without waiting for the full platform.

---

## What This Phase Delivers

At the end of Phase 1, the following is operational:

1. Staff can create a **new Case record** (manually, without a lead) from the minimal Staff App screen.
2. On case creation, Power Automate automatically creates a linked **Fact-Find Responses item**, generates a guest-shared direct link, and sends a welcome email to the customer with:
   - A direct link to their fact-find item's edit form
   - Links to their document upload folders (01–04)
3. The **customer** can open their unique link (no M365 account required), fill in their personal, employment, property, and loan details, and click Submit.
4. Each time the customer updates their fact-find, the **case owner is notified** via Teams.
5. Staff can open the case in the **Staff App Fact-Find tab** and review all sections in a read-only view, then mark it as Reviewed.
6. Customer can upload documents (identity, income, property, bank statements) to their shared folders.

---

## SharePoint Lists Required

### 5.1 Cases List (Minimal — Phase 1 columns only)

All columns from `05-sharepoint-list-schemas.md` §5.2. In Phase 1, the following column groups are required; all others are present in the list schema but may be left empty until Phase 3:

| Column group | Phase 1 required? |
|---|---|
| CaseID, Title | ✅ Required |
| ProductType, CaseStatus, CaseOwner | ✅ Required |
| LeadID | ⬜ Optional in Phase 1 (no leads yet) |
| Applicant details (names, email, phone) | ✅ Required (to send welcome email) |
| Property address, postcode | ✅ Required |
| Loan amount required | ✅ Required |
| CurrentStageID (Lookup → Stage Config) | ✅ Required (stages 1–3 only for Phase 1) |
| CurrentStageNumber, CurrentStageName | ✅ Required (denormalized — set by flow) |
| StageChangedDate | ✅ Required |
| BTL-specific fields | ⬜ Present, not required until Phase 3 |
| Bridge/Auction/Dev/Refurb fields | ⬜ Present, not required until Phase 3 |
| Commission summary fields | ⬜ Not required until Phase 5 |
| LenderID, ReferrerID | ⬜ Not required until Phase 3 |
| FactFindItemID | ✅ Required — FK to Fact-Find Responses |
| EnquiryDate, TargetCompletionDate | ✅ Required |

> **Source:** `05-sharepoint-list-schemas.md` lines 36–106

---

### 5.3 Stage Configuration List (Phase 1 rows only)

Create the list with all columns (`05-sharepoint-list-schemas.md` lines 116–129). For Phase 1, enter only stages 1–3:

| # | Title | ShortName | CustomerFriendlyName | StageNumber | VisibleToCustomer | DefaultSLADays | CanSkip |
|---|---|---|---|---|---|---|---|
| 1 | Customer Enquiry | ENQ | Application Received | 1 | Yes | 1 | No |
| 2 | Fact-Find | FACT | Information Gathering | 2 | Yes | 5 | No |
| 3 | Terms of Business & GDPR | TOB | Terms Agreed | 3 | Yes | 2 | No |

> The remaining 11 stages are added in Phase 3 when the full lifecycle is built. Adding them now does no harm but is not required.  
> **Source:** `05-sharepoint-list-schemas.md` lines 131–148

---

### 5.4 Fact-Find Responses List (Full schema — all sections)

Create with the **complete schema** now. All sections are required for Phase 1. The customer fills in whatever is applicable to their case type.

| Section | Key columns | Source |
|---|---|---|
| Identity (Title, CaseID, CaseRef) | Title set to CaseID by flow | §5.4 lines 162–163 |
| Applicant 1 Personal | A1Title, A1FirstName, A1LastName, A1DateOfBirth, A1NiNumber, A1MaritalStatus, A1ResidentialStatus, A1CurrentAddress (lines 1, 2, town, postcode), A1TimeAtAddress (years, months), A1PreviousAddress (if < 3 years), A1MobilePhone, A1Email | §5.4 lines 165–183 |
| Applicant 2 Personal | Same fields prefixed A2 | §5.4 lines 184–196 |
| Income & Employment | A1EmploymentStatus, A1EmployerName, A1JobTitle, A1TimeEmployedYears, A1GrossAnnualSalary, A1OtherIncome/Source, A1SelfEmp fields (TradingAs, YearsTrading, NetProfit1/2/3), A2 equivalents | §5.4 lines 200–215 |
| Property Details | PropertyAddress, Postcode, PropertyType, Tenure, LeaseYearsRemaining, YearBuilt, Bedrooms, Bathrooms, Listed/NonStandard flags, CurrentUse, IntendedUse, PurchaseOrRefinance, EstimatedValue, PurchasePrice, OutstandingMortgageBalance, ExistingLender, MonthlyRentalIncome, IsHMO, NumberOfUnits, EpcRating | §5.4 lines 221–244 |
| Portfolio Details | IsPortfolioLandlord, TotalPortfolioProperties/Value/MortgageBalance/RentalIncome, PortfolioDetails | §5.4 lines 249–255 |
| Company / SPV | IsCompanyApplication, CompanyName, CompanyNumber, CompanyType, IncorporationDate, CompanyAddress, NatureOfBusiness, Turnover, NetProfit, DirectorsNames | §5.4 lines 259–270 |
| Loan Requirements | LoanAmountRequired, LoanPurpose, RepaymentType, PreferredTermMonths, MaxMonthlyPayment, HasExistingFinance, ExistingFinanceDetails, AdverseCredit, AdverseCreditDetails | §5.4 lines 274–284 |
| Bridging / Development | ExitStrategy, ExitStrategyDetails, AuctionDate, CompletionDeadlineDate, WorksRequired, WorksDescription, WorksBudget, BuildCost, EstimatedGDV, ProfitOnCosts, DrawdownSchedule, PlanningPermissionStatus | §5.4 lines 288–301 |
| Additional Info | Solicitor, SolicitorPhone, SolicitorEmail, PowerOfAttorney, SpecialCircumstances, CustomerDeclarationAgreed, CustomerDeclarationDate, LastUpdatedByCustomer, **Status** (Not Started / In Progress / Submitted / Reviewed) | §5.4 lines 305–315 |

> **Source:** `05-sharepoint-list-schemas.md` lines 152–315

---

### 5.13 Counters List

Required for sequential CaseID generation.

| Title | CurrentValue | Prefix | PaddingLength |
|---|---|---|---|
| BTL | 0 | BTL-2026- | 4 |
| BRIDGE | 0 | BRIDGE-2026- | 4 |
| COMMERCIAL | 0 | COMM-2026- | 4 |
| DEV | 0 | DEV-2026- | 4 |
| REFURB | 0 | REFURB-2026- | 4 |
| AUCTION | 0 | AUCT-2026- | 4 |

> **Source:** `05-sharepoint-list-schemas.md` lines 510–530

---

## Document Library

### Case Documents Library (Phase 1 — folders 01–04 per case)

The library is created now with the full structure. Power Automate creates all 13 subfolders on case creation, but in Phase 1 only folders 01–04 are shared with the customer.

```
Case Documents/
└── {CaseID}/
    ├── 01-Identity/          ← Customer upload (guest link, Phase 1)
    ├── 02-Income/            ← Customer upload (guest link, Phase 1)
    ├── 03-Property/          ← Customer upload (guest link, Phase 1)
    ├── 04-Bank-Statements/   ← Customer upload (guest link, Phase 1)
    ├── 05-Fact-Find/         ← Auto PDF (future v1.1)
    ├── 06-Terms-of-Business/ ← Staff only (Phase 3)
    ├── 07-Illustrations/     ← Staff only (Phase 3)
    ├── 08-DIP/               ← Staff only (Phase 3)
    ├── 09-Application/       ← Staff only (Phase 3)
    ├── 10-Valuation/         ← Staff only (Phase 3)
    ├── 11-Offer/             ← Staff only (Phase 3)
    ├── 12-Legal/             ← Staff only (Phase 3)
    └── 13-Completion/        ← Staff only (Phase 3)
```

> **Source:** `06-document-library.md` lines 14–35 and §6.3

---

## Power Automate Flows Required

### Flow 3 — Generate Case ID

| Attribute | Value |
|---|---|
| Trigger | When Case item created |
| Action | Read Counters list by ProductType; increment counter; update Case Title and CaseID with formatted ID (e.g. `BTL-2026-0001`) |
| Dependency | Counters list |
| Notes | Uses SharePoint Patch with optimistic locking to prevent duplicate IDs |

> **Source:** `10-automation-summary.md` line 16; `17-key-design-decisions.md` not explicitly but concurrency note at `05-sharepoint-list-schemas.md` line 532

---

### Flow 4 — Create Fact-Find Item

| Attribute | Value |
|---|---|
| Trigger | When Case item created |
| Action | 1. Create Fact-Find Responses item (set CaseID, CaseRef, Title = CaseID); 2. Generate guest share link for the item; 3. Update Case record with FactFindItemID and the guest link URL |
| Dependency | Flow 3 (CaseID must exist before Fact-Find item is titled) — use trigger condition or delay until CaseID is set |
| Notes | Guest share link gives customer edit access to their specific item only |

> **Source:** `10-automation-summary.md` line 17; `08-external-customer-portal.md` lines 138–145

---

### Flow 5 — Create Case Folder Structure

| Attribute | Value |
|---|---|
| Trigger | When Case item created |
| Action | 1. Create top-level CaseID folder in Case Documents library; 2. Create all 13 subfolders; 3. Apply guest sharing (Contribute) to folders 01–04 and store share links on Case record; 4. Update Case record with library folder path URL |
| Dependency | Flow 3 (CaseID needed for folder name) |

> **Source:** `10-automation-summary.md` line 18; `06-document-library.md` §6.3; `08-external-customer-portal.md` lines 138–147

---

### Flow 15 — Fact-Find Updated Notification

| Attribute | Value |
|---|---|
| Trigger | When Fact-Find Responses item modified |
| Action | 1. Update `LastUpdatedByCustomer` timestamp on the Fact-Find item; 2. Send Teams notification to Case Owner: "[Customer name] has updated their fact-find for [CaseID]" |
| Dependency | None |

> **Source:** `10-automation-summary.md` line 28

---

### Welcome Email (part of Flow 2 / Flow 5)

The welcome email is sent from Flow 5 (or as a dedicated sub-flow) once the folder links are generated. It includes:
- Portal access link (SharePoint progress page — even if minimal in Phase 1)
- **Direct link to Fact-Find item edit form** (from Flow 4)
- **Document upload folder links** (01–04, from Flow 5)
- "Contact your advisor: [advisor email]"

> **Source:** `09-end-to-end-process.md` lines 32–39; `08-external-customer-portal.md` lines 148–162

---

## Staff App Screens Required

### Minimal Case Creation Screen

A simple form to create a new Case record. Required fields:
- Applicant 1 first/last name, email, phone
- Applicant 2 (optional)
- ProductType (Choice)
- PropertyAddress, PropertyPostcode
- LoanAmountRequired
- CaseOwner (Person — default to current user)
- EnquiryDate (default today)

On save, Flow 3, 4, and 5 trigger automatically.

---

### Case Workspace — Fact-Find Tab

Read-only view of the linked Fact-Find Responses item.

```
╔══════════════════════════════════════════════════════════════════╗
║  FACT-FIND                              Status: In Progress      ║
║  Last updated by customer: 09 Apr 2026 14:32                     ║
║                                                                  ║
║  Section: [Personal ▼]           [📋 View All]  [📤 Export]     ║
║  ┌──────────────────────────────────────────────────────────┐    ║
║  │  APPLICANT 1               APPLICANT 2                   │    ║
║  │  Name:  John Doe           Name:  Jane Doe               │    ║
║  │  DOB:   12/03/1978         DOB:   04/09/1981             │    ║
║  │  NI:    AB123456C          NI:    CD789012E              │    ║
║  │  ...                                                     │    ║
║  └──────────────────────────────────────────────────────────┘    ║
║  [Send Reminder to Customer]  [Copy Portal Link]  [Mark Reviewed]║
╚══════════════════════════════════════════════════════════════════╝
```

Buttons:
- **[Send Reminder to Customer]** — triggers a reminder email with the fact-find link
- **[Copy Portal Link]** — copies the guest-shared fact-find link to clipboard
- **[Mark Reviewed]** — sets `Status = "Reviewed"` on the Fact-Find item

> **Source:** `07-screen-wireframes-internal.md` lines 151–184

---

## Customer Experience in Phase 1

```
Day 1
┌────────────────────────────────────────────────────────────────┐
│  Staff creates Case record in Staff App                        │
│  ↓                                                             │
│  Flows 3, 4, 5 trigger automatically                          │
│  ↓                                                             │
│  Customer receives welcome email with:                         │
│    • Direct fact-find link (unique to their case)              │
│    • Document upload folder links (01–04)                      │
│    • Advisor contact email                                     │
└────────────────────────────────────────────────────────────────┘
       ↓
Days 1–5 (Fact-Find Stage — SLA: 5 days)
┌────────────────────────────────────────────────────────────────┐
│  Customer opens link (no M365 account needed)                  │
│  Completes SharePoint list item form:                          │
│    • Personal details (both applicants)                        │
│    • Employment and income                                     │
│    • Property details                                          │
│    • Loan requirements                                         │
│    • Declaration checkbox                                      │
│  Customer uploads identity and income documents to folders     │
│                                                                │
│  Case owner receives Teams notification on each update         │
│  Case owner reviews fact-find in Staff App (read-only)         │
│  Case owner clicks [Mark Reviewed] when satisfied              │
└────────────────────────────────────────────────────────────────┘
```

> **Source:** `09-end-to-end-process.md` lines 42–53; `08-external-customer-portal.md` §8.2

---

## Permissions Summary (Phase 1)

| Actor | Access |
|---|---|
| Staff | Full control of Cases list, Fact-Find Responses list, Case Documents library |
| Customer | Edit access to their own Fact-Find Responses item (guest link — direct item only) |
| Customer | Contribute access to folders 01–04 of their case (guest link) |
| Customer | Cannot access other customers' data, internal notes, or folders 05–13 |

> **Source:** `08-external-customer-portal.md` lines 193–207

---

## Phase 1 Acceptance Criteria

| # | Criterion |
|---|---|
| 1 | Staff can create a new case record from the Staff App |
| 2 | CaseID is auto-generated (e.g. BTL-2026-0001) within 30 seconds of case creation |
| 3 | A Fact-Find Responses item is created automatically and linked to the case |
| 4 | Customer receives welcome email with working fact-find link and upload folder links |
| 5 | Customer can open fact-find link without an M365 account |
| 6 | Customer can fill in and save the fact-find form (multiple saves allowed) |
| 7 | Case owner receives Teams notification when customer updates fact-find |
| 8 | Staff can view all fact-find sections in the Fact-Find tab (read-only) |
| 9 | Staff can mark fact-find as Reviewed |
| 10 | Customer can upload files to folders 01–04 |
| 11 | Customer cannot access any other case's data or folders 05–13 |

---

## What Phase 1 Does NOT Include

| Capability | Delivered in Phase |
|---|---|
| Lead capture and qualification | Phase 2 |
| Convert Lead to Case button | Phase 2 |
| Full 14-stage lifecycle progression | Phase 3 |
| Stage History (audit log) | Phase 3 |
| Document Request tracking list | Phase 3 |
| Messages list (staff ↔ customer) | Phase 3 |
| SLA reminders | Phase 3 |
| Email Docs to Lender | Phase 3 |
| Full customer portal progress page | Phase 4 |
| Commission tracking | Phase 5 |
| KPI Dashboards | Phase 5 |
