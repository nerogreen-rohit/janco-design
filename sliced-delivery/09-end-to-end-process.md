# 09. End-to-End Process — Sliced Delivery View

**Version:** 1.0  
**Date:** 10 April 2026  
**Companion to:** `../09-end-to-end-process.md` (full platform view)

This document shows the end-to-end process **as it changes at each delivery phase**. It answers the question: *"What can staff and customers actually do the day each phase goes live?"*

The full-platform end-to-end flow is documented in the root design file `09-end-to-end-process.md`. This document focuses on the **incremental process capability** delivered phase by phase.

---

## How to Read This Document

Each section describes:
1. **What changes** when that phase goes live
2. The **process flow** staff and customers follow using the new capability
3. **What still uses the old method** (bridging steps until subsequent phases land)

---

## Phase 1 Go-Live — Fact-Find Process

**Week 2. What is live:** Case creation (manual), Fact-Find Responses list, document library (folders 01–04), welcome email, Staff App Fact-Find tab.  
**What is NOT yet live:** Lead pipeline, full stage lifecycle, messages, SLA alerts, customer progress portal.

### Staff Process — Phase 1

```
┌──────────────────────────────────────────────────────────────────────┐
│                    STAFF PROCESS — PHASE 1                          │
└──────────────────────────────────────────────────────────────────────┘

[Enquiry received — phone / email / website (unchanged)]
      │
      ▼
[Staff creates Case record in Staff App]   ← NEW
  • Enter applicant name, email, phone
  • Select ProductType
  • Enter property address and loan amount
  • Set CaseOwner (default: current user)
  • Click Save
      │
      ▼   [Power Automate triggers automatically]
      │
      ├──▶ Flow 3: CaseID generated (e.g. BTL-2026-0001)         ← NEW
      │
      ├──▶ Flow 4: Fact-Find item created; guest link generated   ← NEW
      │
      └──▶ Flow 5: 13 subfolders created; folders 01–04 guest     ← NEW
                   shared; welcome email sent to customer

[Staff notified of each customer fact-find update via Teams]   ← NEW
      │
      ▼
[Staff opens Case Workspace → Fact-Find tab]                   ← NEW
  • Reviews all sections (read-only)
  • Requests missing information via email (manual — Messages
    tab not yet live)
  • Clicks [Mark Reviewed] when satisfied

[Subsequent qualification and product research steps]
  → ⚠ Still handled by existing email/spreadsheet process
     until Phase 2 (lead pipeline) and Phase 3 (lifecycle) land
```

---

### Customer Process — Phase 1

```
┌──────────────────────────────────────────────────────────────────────┐
│                  CUSTOMER PROCESS — PHASE 1                         │
└──────────────────────────────────────────────────────────────────────┘

Day 1 — Welcome Email Received
──────────────────────────────
Customer receives automated email containing:
  • "Complete your information" → direct fact-find link (unique)
  • "Upload your documents" → folder links 01–04
  • Advisor contact email

No M365 account required. Links work in any browser.

Days 1–5 — Fact-Find Completion
─────────────────────────────────
Customer opens fact-find link → SharePoint list item edit form
  • Section 1: Personal details (Applicant 1 + 2)
  • Section 2: Employment and income
  • Section 3: Property details
  • Section 4: Loan requirements
  • Section 5: Company / SPV (if applicable)
  • Section 6: Bridging / development details (if applicable)
  • Declaration checkbox
  • Click Save (can save multiple times before final submission)

Customer uploads documents to shared folders:
  • 01-Identity (passport, driving licence, utility bill)
  • 02-Income (payslips, SA302, company accounts)
  • 03-Property (mortgage statement, tenancy agreement, EPC)
  • 04-Bank-Statements

Customer cannot yet:
  → ⚠ View a progress tracker (Phase 4)
  → ⚠ Send messages to their advisor (Phase 3 / 4)
  → ⚠ See which documents have been reviewed (Phase 3 / 4)
```

---

### Bridging Steps — Phase 1 Only

| Task | Interim Method | Replaced in |
|---|---|---|
| Lead qualification | Existing email / phone / spreadsheet | Phase 2 |
| Sending update messages to customer | Email directly from advisor | Phase 3 |
| Document request tracking | Mental / spreadsheet | Phase 3 |
| Customer progress view | Advisor calls / email | Phase 4 |
| Commission recording | Existing spreadsheet | Phase 5 |

---

## Phase 2 Go-Live — Lead Management Process

**Week 3. What is live (in addition to Phase 1):** Leads list, lead qualification pipeline, [Convert to Case] button, Referrers list.

### Staff Process — Phase 2

```
┌──────────────────────────────────────────────────────────────────────┐
│                    STAFF PROCESS — PHASE 2                          │
└──────────────────────────────────────────────────────────────────────┘

[Enquiry received]
      │
      ▼
[Staff creates Lead record in Staff App — Leads screen]        ← NEW
  • Name, email, phone, product interest, loan amount
  • Assign to staff member
  • LeadID auto-generated (e.g. LEAD-0021)
  • LeadStatus = "New"
      │
      ▼
[Staff qualifies lead — updates LeadStatus]                    ← NEW
  Contacted → Qualified (or Lost / Dormant)
      │ (if Qualified)
      ▼
[Staff clicks [Convert to Case] in Lead detail view]           ← NEW
      │
      ▼   [Flow 2 triggers]
      │
      ├──▶ Case record created pre-populated from Lead data
      │
      └──▶ Flows 3, 4, 5 trigger: CaseID, Fact-Find item,
           folders, welcome email — exactly as in Phase 1
      │
      ▼
[Lead updated: Status = "Converted", ConvertedToCaseID stamped] ← NEW

[Fact-find and review process — same as Phase 1]
```

---

### What Changes for the Customer — Phase 2

Nothing changes from the customer's perspective. The welcome email and fact-find flow are identical. The improvement is entirely on the staff side (structured lead tracking instead of ad-hoc notes/spreadsheet).

---

## Phase 3 Go-Live — Case Lifecycle Process

**Weeks 4–5. What is live (in addition to Phases 1–2):** Full 14-stage lifecycle, Stage History, Document Requests list, Messages list, Tasks list, SLA reminder flow, Email Docs to Lender, Lenders list, Stage Config rows 4–14.

### Staff Process — Phase 3

```
┌──────────────────────────────────────────────────────────────────────┐
│                    STAFF PROCESS — PHASE 3                          │
└──────────────────────────────────────────────────────────────────────┘

[Case created via Lead conversion (Phase 2) or direct creation (Phase 1)]
      │
      ▼
[Stage 1: Customer Enquiry]
  Staff confirms receipt. Advances to Stage 2.
      │
      ▼
[Stage 2: Fact-Find] — SLA: 5 days
  Staff reviews fact-find (Phase 1 Fact-Find tab)
  Staff uses [+ Request Document] in Documents tab            ← NEW
    → Flow 7: Customer receives document request email
    → Customer uploads → Flow 8: "Received" status, notification
  Staff marks fact-find Reviewed
  Advances to Stage 3
      │
      ▼   [Flow 6: Stage History record written — immutable]    ← NEW
      │   [Flow 13: Customer email if VisibleToCustomer = Yes]  ← NEW
      ▼
[Stage 3: Terms of Business & GDPR] — SLA: 2 days
  Staff issues TOB; uploads signed copy to 06-Terms-of-Business
  Staff advances stage
      │
      ▼
[Stage 4: Product Research] — Internal only (not shown on portal)
  Staff researches lender panel
  Staff records findings in Stage Notes
  Staff advances stage
      │
      ▼
[Stage 5: Present Options & Agree] — SLA: 3 days
  Staff uses Messages tab to communicate options to customer    ← NEW
  Customer confirmed via message / email
  Staff advances stage
      │
      ▼
[Stage 6: Produce Illustrations] — SLA: 3 days
  Staff uploads ESIS/KFI to 07-Illustrations folder
  Staff sends illustration via Messages tab                    ← NEW
      │
      ▼
[Stage 7: Decision in Principle] — SLA: 5 days
  Staff submits DIP to lender
  Staff uploads DIP confirmation to 08-DIP folder
  Staff advances stage
      │
      ▼
[Stage 8: Full Application] — SLA: 7 days
  Staff compiles application pack
  Staff uses [📧 Email Lender] in Documents tab                ← NEW
    → Flow 12: Files sent to LenderID.SubmissionEmail
    → Event logged in Stage History and Messages
  Staff advances stage
      │
      ▼
[Stages 9–13: Valuation, Underwriting, Offer, Legals, Completion]
  Each stage:
    • Case owner manages progress
    • SLA reminder fires if overdue (Flow 9 — daily 08:00)     ← NEW
    • Staff advances stage → Stage History written (immutable)
    • Customer notified automatically on VisibleToCustomer stages
      │
      ▼
[Stage 13: Completion]
  Funds released; staff confirms completion
  Customer receives automated completion email
  Commission records updated (manual until Phase 5)
      │
      ▼
[Stage 14: Post-Completion & Logging]
  Final admin; case status → "Completed"
```

---

### Stage Skip Process — Bridging / Auction Cases

For product types where stages 5, 6, and 7 are not applicable:

```
[Stage 4: Product Research]
      │
      ▼  Staff selects [Skip Stage] for Stage 5
      │  Enters reason: "Bridging — no illustrations required"
      │
      ▼  [Flow 6: Stage History written with IsSkipped = Yes]
      │
      ▼  Stage advances directly to Stage 8 (Full Application)
```

The Timeline tab shows all skipped stages clearly labelled with skip reason.

---

### What Changes for the Customer — Phase 3

Customers now receive **automated stage notification emails** when their case advances to a `VisibleToCustomer` stage. They also receive **document request emails** when staff request specific documents. The advisor can now send messages via the Messages list and customers receive email notifications of new messages.

However, customers still cannot view a structured progress page — that comes in Phase 4.

---

## Phase 4 Go-Live — Customer Portal Process

**Week 6. What is live (in addition to Phases 1–3):** Full SharePoint customer portal page with progress tracker, document status, messages view, fact-find link, upload links.

### Customer Process — Phase 4 (Full)

```
┌──────────────────────────────────────────────────────────────────────┐
│                CUSTOMER PROCESS — PHASE 4 (COMPLETE)                │
└──────────────────────────────────────────────────────────────────────┘

[Welcome email received — Day 1]
  Includes portal page link (now fully useful — Phase 4)       ← IMPROVED

[Customer opens portal page]
  ✅ Application Received
  ✅ Information Gathering
  ▶  Terms Agreed  ← YOU ARE HERE
  ○  Options Presented
  ○  ...
  (CustomerFriendlyName values from Stage Config — no internal
   stage names visible; Stage 4 Product Research hidden)

[Customer sees "What you need to do" panel]
  • Outstanding document requests listed with due dates
  • [📤 Upload Documents] links to their shared folders

[Customer accesses fact-find from portal]
  • [📋 Update Your Information] → opens fact-find guest link
  • Can return and update multiple times before submission

[Customer views and sends messages]
  • [📨 View Messages] → filtered Messages list view
  • Customer can read advisor messages and reply
  • Internal Direction messages NOT shown to customer

[Customer receives milestone emails]
  • Sent by Flow 13 on each stage advance (VisibleToCustomer = Yes)
  • Email contains CustomerFriendlyName of new stage and
    a link back to the portal
```

---

### Staff Process — Phase 4

No changes to staff process. Phase 4 is entirely customer-facing. Staff continue to use the same Staff App screens from Phases 1–3. The portal page reflects the data staff are already managing.

---

## Phase 5 Go-Live — Commissions & Dashboard Process

**Week 7. What is live (in addition to Phases 1–4):** Commissions list, commission flows (10/11/16), Commission tab in Case Workspace, Commissions Overview screen, all 4 KPI dashboards.

### Commission Recording Process — Phase 5

```
┌──────────────────────────────────────────────────────────────────────┐
│                  COMMISSION PROCESS — PHASE 5                       │
└──────────────────────────────────────────────────────────────────────┘

[Lender assigned to Case record]
      │
      ▼
[Flow 11: Expected Proc Fee calculated automatically]            ← NEW
  LoanAmountRequired × LenderProcFeePercent
  Commission record created: Type = "Proc Fee"

[Staff adds Advisory Fee record from Commission tab]             ← NEW
  • CommissionType = "Advisory Fee (AdvFee)"
  • Amount = agreed fee (e.g. £1,500)
  • TotalCommissionExpected on Case updates automatically

[Staff adds Referral Fee record if applicable]                   ← NEW
  • CommissionType = "Referral Fee"
  • SplitPercentage = referrer's share
  • ReferrerID = linked Referrer record

[Case advances to Stage 13 (Completion)]
      │
      ▼
[Flow 16: Case Completion Wrap-Up fires]                         ← NEW
  • Customer receives completion email
  • Outstanding Document Requests set to "Not Required"
  • Case owner receives commission review reminder

[Staff marks Commission records as received]
  • IsReceived = Yes; ReceivedDate stamped
  • Commission Dashboard updates in real time
```

---

### Dashboard Process — Phase 5

```
[Staff opens Dashboard screen in Staff App]                      ← NEW

PIPELINE DASHBOARD — viewed daily / weekly
  • Shows active cases by stage and by product type
  • Total pipeline value (sum of LoanAmountRequired)
  • Used in team meetings to review workload

PERFORMANCE DASHBOARD — viewed weekly
  • Lead → Case conversion rate
  • Case → Completion conversion rate
  • Average days per stage (from Stage History TimeInPreviousStageHours)
  • SLA performance (on track / at risk / overdue counts)

COMMISSION DASHBOARD — viewed monthly
  • Proc Fees: expected vs received vs %
  • Advisory Fees (AdvFee): expected vs received vs %
  • Referral Fees: expected vs received vs %
  • Income by product type (bar chart)
  • Outstanding commissions list
  • Filter by TaxYear for year-end reporting

COMPLIANCE DASHBOARD — reviewed regularly
  • TOB signed: % of active cases with signed TOB in folder 06
  • Fact-Find completion: Submitted / In Progress / Not Started
  • Overdue document requests list
  • Adverse credit flagged cases
```

---

### What Changes for the Customer — Phase 5

Nothing changes from the customer's perspective. Phase 5 improvements are internal to staff.

---

## Full End-to-End Process — After Phase 5 (All Phases Live)

This is the complete operational process once Phases 1–5 are all live. Phase 6 (UAT & Go Live) validates this before production handover.

```
┌──────────────────────────────────────────────────────────────────────┐
│            FULL END-TO-END PROCESS (POST-PHASE 5)                   │
└──────────────────────────────────────────────────────────────────────┘

ENQUIRY INTAKE
──────────────
[Enquiry received — phone / email / website]
      │
      ▼
[Staff creates Lead record]
  → LeadID generated automatically
  → Assign to staff member; LeadStatus = "New"
      │
      ▼
[Staff qualifies lead]
  → LeadStatus updated: Contacted → Qualified (or Lost)
      │ (if Qualified)
      ▼
[Staff clicks [Convert to Case]]
  → Case created; CaseID generated
  → Fact-Find item created; guest link generated
  → 13 document subfolders created; folders 01–04 guest-shared
  → Welcome email sent to customer with:
      • Portal link
      • Fact-find link
      • Document upload folder links
  → Lead updated: Status = "Converted"

FACT-FIND & ONBOARDING (Stages 1–3)
─────────────────────────────────────
[Customer completes fact-find via portal link]
  → Fills in SharePoint list item form (personal, income, property, loan)
  → Uploads identity and income docs to folders 01–04
  → Case owner notified via Teams on each update

[Staff reviews fact-find in Fact-Find tab]
  → Requests any missing documents via Document Requests
  → Customer receives document request email; uploads to folder
  → Staff marks fact-find "Reviewed"

[Stage 2 → 3 advance]
  → Stage History written (immutable)
  → Customer notified: "Terms Agreed" stage email

[Staff issues TOB; uploads signed copy to folder 06]
  → Stage 3 → 4 advance; Stage History written

PRODUCT RESEARCH & PRESENTATION (Stages 4–6)
──────────────────────────────────────────────
[Stage 4: Product Research — internal only]
  → Staff researches lender panel; records findings in Stage Notes
  → Stage advances; Stage History written; customer NOT notified

[Stage 5: Present Options]
  → Staff sends message via Messages tab
  → Customer receives email notification; replies via portal
  → Stage advances; customer notified: "Options Presented"

[Stage 6: Produce Illustrations]
  → Staff uploads ESIS/KFI to folder 07
  → Staff sends via Messages tab
  → Stage advances; customer notified: "Illustrations Prepared"

APPLICATION (Stages 7–8)
─────────────────────────
[Stage 7: Decision in Principle]
  → Staff submits DIP to lender
  → DIP result uploaded to folder 08
  → Stage advances; customer notified: "Initial Decision"

[Stage 8: Full Application]
  → Staff compiles application pack
  → Staff uses [Email Lender] → Flow 12 sends to lender
  → Event logged in Stage History and Messages
  → Stage advances; customer notified: "Application Submitted"

LENDER PROCESS (Stages 9–11)
──────────────────────────────
[Stage 9: Valuation]
  → Lender instructs valuer; staff uploads report to folder 10
  → SLA reminder fires if overdue (daily 08:00 flow)
  → Stage advances; customer notified: "Valuation Arranged"

[Stage 10: Underwriting]
  → Lender reviews; staff manages queries
  → Stage advances; customer notified: "Lender Review"

[Stage 11: Loan Offer]
  → Offer uploaded to folder 11
  → Stage advances; customer notified: "Offer Issued"

LEGALS & COMPLETION (Stages 12–14)
────────────────────────────────────
[Stage 12: Legals]
  → Solicitors instructed; legal docs in folder 12
  → Stage advances; customer notified: "Legal Work"

[Stage 13: Completion]
  → Funds released; staff confirms
  → Flow 16: Completion email to customer; Doc Requests closed;
    commission review reminder to case owner
  → Customer portal: Application Completed ✅

[Stage 14: Post-Completion]
  → Commission records confirmed (IsReceived = Yes)
  → Case status = "Completed"
  → Commission Dashboard updated

DASHBOARDS (ongoing, all phases live)
───────────────────────────────────────
Pipeline   → Daily/weekly: workload and pipeline value review
Performance→ Weekly: conversion rates and SLA performance
Commissions→ Monthly: income tracking and year-end reporting
Compliance → Regular: TOB, fact-find, document overdue checks
```

---

## Customer Journey — Full Platform (Phases 1–5 Live)

```
┌──────────────────────────────────────────────────────────────────────┐
│                     CUSTOMER JOURNEY — FULL PLATFORM                │
└──────────────────────────────────────────────────────────────────────┘

Day 1 — Welcome
  Automated email with portal link, fact-find link, upload folder links

Days 1–5 — Fact-Find Stage
  • Opens portal → sees progress tracker (Information Gathering ▶)
  • Completes fact-find online (no M365 account needed)
  • Uploads identity, income, property docs
  • Sends message to advisor if needed

Days 5–10 — Research & Options
  • Portal updates: Terms Agreed ✅ → Under Review ▶ → Options Presented ▶
  • Receives illustration documents from advisor via Messages

Days 10–20 — Application
  • Portal: Initial Decision ▶ → Application Submitted ▶
  • Receives advisor updates via Messages and automated emails

Days 20–60 — Lender Process
  • Portal: Valuation Arranged ▶ → Lender Review ▶ → Offer Issued ▶
  • Automated email: "Offer Issued — your advisor will be in touch"

Days 60–90 — Legals & Completion
  • Portal: Legal Work ▶ → Funds Released ✅
  • Automated completion email
  • Portal: Application Completed ✅

CUSTOMER TOUCHPOINTS SUMMARY

| Touchpoint | Method | Phase Introduced |
|---|---|---|
| Welcome email | Automated (Flow 5) | Phase 1 |
| Fact-find link | SharePoint guest (direct item link) | Phase 1 |
| Document upload folders | SharePoint guest (folder links 01–04) | Phase 1 |
| Staff fact-find update notification | Teams (Flow 15) | Phase 1 |
| Document request emails | Automated (Flow 7) | Phase 3 |
| Stage notification emails | Automated (Flow 13) | Phase 3 |
| Advisor messages | SharePoint Messages list (email notification) | Phase 3 |
| Portal progress page | SharePoint page | Phase 4 |
| Portal action panel | SharePoint page | Phase 4 |
| Completion email | Automated (Flow 16) | Phase 5 |
```

---

## What is Never Shown to Customers

The following data is internal to staff at all times across all phases:

| Internal item | Why hidden |
|---|---|
| Stage 4: Product Research | `VisibleToCustomer = No` in Stage Config |
| Internal stage names (e.g. "Fact-Find", "UW") | `CustomerFriendlyName` used on portal and in emails |
| Stage History records | No customer access to Stage History list |
| Staff notes on cases | No customer access to Case record columns |
| Commission records | No sharing applied to Commissions list |
| Messages with `Direction = "Internal"` | Filtered out of customer Messages view |
| Document folders 05–13 | Guest sharing only on folders 01–04 |
| Other customers' data | Isolated by direct item/folder sharing — no list-level access |

> **Source:** `17-key-design-decisions.md` §17.4; `08-external-customer-portal.md` lines 193–207

---

## Document Index (Sliced Delivery Folder)

| File | Contents |
|---|---|
| `00-overview.md` | Phasing rationale, phase summary, design constraints |
| `phase-1-fact-find.md` | Phase 1 detail — lists, flows, screens, ACs |
| `phase-2-lead-management.md` | Phase 2 detail — Leads, Referrers, conversion flow |
| `phase-3-case-management.md` | Phase 3 detail — full lifecycle, documents, messages, SLA |
| `phase-4-customer-portal.md` | Phase 4 detail — customer portal, permissions |
| `phase-5-commissions-dashboards.md` | Phase 5 detail — commissions, 4 dashboards |
| `phase-6-uat-golive.md` | Phase 6 — 10 test scenarios, go-live checklist, roadmap |
| `09-end-to-end-process.md` | **This file** — end-to-end process through the phased delivery lens |
