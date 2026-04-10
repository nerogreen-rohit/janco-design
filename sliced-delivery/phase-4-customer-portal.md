# Phase 4 — Customer Portal

**Goal:** Full customer-facing portal with application progress tracker, document upload, messaging, and fact-find access — all via SharePoint.  
**Estimated duration:** 1 week (Week 6)  
**Depends on:** Phase 1 (Fact-Find, guest sharing), Phase 3 (Messages list, stage tracking, Document Requests)  
**Business value:** Customer self-service — customers can track their application, upload documents, communicate with their advisor, and complete their fact-find without calling the office.

---

## What This Phase Delivers

At the end of Phase 4, the following is operational:

1. A **SharePoint portal page** is live for each customer, showing their application progress (customer-friendly stage names only, no internal data visible).
2. The progress tracker shows completed, current, and upcoming stages — dynamically driven by the Stage Configuration list.
3. A **"What you need to do"** action panel highlights any outstanding document requests or actions.
4. The customer can access their **Fact-Find** form directly from the portal (Phase 1 link, surfaced in the portal UI).
5. The customer can **view and reply to messages** from their advisor via the Messages list.
6. The customer can view their **uploaded documents** and upload new files.
7. Customer permissions are fully locked down — no access to other cases, internal data, or staff-only folders.

---

## Technical Implementation

The customer portal requires no Power Apps license for customers. It is built entirely on SharePoint Online features included in M365 Business Standard.

> **Source:** `17-key-design-decisions.md` §17.4; `13-licensing.md` §13.2

### Components

| Component | Technology | Notes |
|---|---|---|
| Progress page | SharePoint Page (web part) | Embedded filtered list view — filtered by CaseID |
| Fact-find access | Guest-shared direct link to Fact-Find item | Generated in Phase 1 by Flow 4 |
| Document upload | Guest-shared folder links (01–04) | Generated in Phase 1 by Flow 5 |
| Messages | Guest-shared filtered Messages list view | Filtered to CaseID, Direction ≠ Internal |
| Progress stage data | Cases list (CurrentStageName, CurrentStageNumber) | Read-only via embedded view |

---

## Portal Page Structure

```
╔══════════════════════════════════════════════════════════════════════╗
║                                                                      ║
║         🏦  YOUR LENDING APPLICATION                                 ║
║         Specialist Lending Hub — Customer Portal                     ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  Welcome, [Applicant Name]                                           ║
║  Case Reference: [CaseID]                                            ║
║                                                                      ║
║  APPLICATION PROGRESS                                                ║
║  ─────────────────────────────────────────────────────────────────  ║
║   ✅ Application Received                                            ║
║   ✅ Information Gathering                                           ║
║   ✅ Terms Agreed                                                    ║
║   ✅ Options Presented                                               ║
║   ✅ Illustrations Prepared                                          ║
║   ✅ Initial Decision                                                ║
║   ▶  Application Submitted  ← YOU ARE HERE                          ║
║   ○  Valuation Arranged                                              ║
║   ○  Lender Review                                                   ║
║   ○  Offer Issued                                                    ║
║   ○  Legal Work                                                      ║
║   ○  Funds Released                                                  ║
║                                                                      ║
║  WHAT YOU NEED TO DO                                                 ║
║  ─────────────────────────────────────────────────────────────────  ║
║  ⚠  Please upload your bank statements (last 6 months)              ║
║  ⚠  Your EPC certificate was rejected — please re-upload            ║
║  [📤 Upload Documents]                                               ║
║                                                                      ║
║  MESSAGES                                                            ║
║  ─────────────────────────────────────────────────────────────────  ║
║  ✉  You have 1 unread message from your advisor                      ║
║  [📨 View Messages]                                                  ║
║                                                                      ║
║  LINKS                                                               ║
║  ─────────────────────────────────────────────────────────────────  ║
║  [📋 Update Your Information (Fact-Find)]                            ║
║  [📁 View Your Documents]                                            ║
║                                                                      ║
║  Questions? Contact your advisor: sarah@[firmname].co.uk             ║
╚══════════════════════════════════════════════════════════════════════╝
```

> **Key UX note:** Customers see `CustomerFriendlyName` values from Stage Configuration (e.g. "Information Gathering" not "Fact-Find"). Only stages where `VisibleToCustomer = Yes` are shown. Stage 4 (Product Research) is internal and hidden from the portal.  
> **Source:** `08-external-customer-portal.md` lines 8–68; note at line 70

---

## Customer Fact-Find Experience

The customer fact-find form is the SharePoint list item edit form accessed via guest-shared direct link (built in Phase 1). In Phase 4 this link is surfaced as a prominent button on the portal page.

The form supports:
- Multiple saves before final submission
- Pre-populated applicant name and case reference (set by Flow 4 at case creation)
- Accordion sections for Applicant 2, Property, Loan, Bridge/Dev specific fields
- Declaration checkbox before submission

> **Source:** `08-external-customer-portal.md` lines 74–118

---

## Customer Access Technical Flow

```
STEP 1 — Case creation (Phase 1)
  Flow 4: Creates Fact-Find item, generates guest share link
  Flow 5: Creates folders, generates folder share links (01–04)
  Welcome email sent with all links

STEP 2 — Customer access (Phase 4 portal)
  Customer clicks portal page link
  Sees filtered progress view (their case only — by CaseID URL parameter)
  Clicks [Update Your Information] → opens Fact-Find guest link (Phase 1)
  Clicks [Upload Documents] → opens folder 01–04 guest links (Phase 1)
  Clicks [View Messages] → opens Messages filtered view

STEP 3 — Customer updates fact-find
  Customer edits Fact-Find list item
  Flow 15: Updates LastUpdatedByCustomer timestamp, notifies case owner (Phase 1)

STEP 4 — Customer uploads documents
  Customer uploads files to shared folders
  Flow 8: Creates/updates Document Request as "Received", notifies case owner (Phase 3)

STEP 5 — Stage advances
  Staff advances case stage in Staff App (Phase 3)
  Flow 13: Sends customer email with CustomerFriendlyName of new stage (Phase 3)
  Portal progress page reflects new stage automatically
```

> **Source:** `08-external-customer-portal.md` lines 123–191

---

## Permissions Configuration

| Customer can ACCESS | How |
|---|---|
| Their Fact-Find item (edit) | Guest-shared direct item link (generated by Flow 4) |
| Document upload folders 01–04 (contribute) | Guest-shared folder links (generated by Flow 5) |
| Messages list filtered to their CaseID (read/contribute) | Guest-shared filtered view |
| Progress page (read only) | SharePoint page — public or guest-accessible |

| Customer CANNOT access | Enforced by |
|---|---|
| Other customers' Fact-Find items | Direct item link (not list-level access) |
| Internal staff notes or stage names | CustomerFriendlyName used; internal stages hidden |
| Commission records | No sharing applied to Commissions list |
| Folders 05–13 | Guest sharing applied to 01–04 only |
| Other cases | Filtered views + permission isolation |

> **Source:** `08-external-customer-portal.md` lines 193–207

---

## IT Admin Prerequisites

The following must be confirmed before Phase 4 go-live:

| Requirement | Owner | Risk |
|---|---|---|
| SharePoint guest sharing enabled at M365 tenant level | IT Admin | Medium — requires tenant admin access. Raise at start of project. |
| "New and existing guests" sharing level at site level | IT Admin | Low once tenant setting is enabled |
| Guest link expiry set to "Never" (or long duration) for active cases | IT Admin | Low — must be set before customer links are generated |
| Automated email deliverability tested | Developer | Medium — automated emails may hit spam filters |

> **Source:** `16-implementation-timeline.md` §16.3; `17-key-design-decisions.md` §17.4

---

## Phase 4 Acceptance Criteria

| # | Criterion |
|---|---|
| 1 | Customer portal page is accessible via a SharePoint guest link |
| 2 | Portal shows CustomerFriendlyName stage values (not internal stage names) |
| 3 | Stage 4 (Product Research) is NOT shown on the portal |
| 4 | Completed stages show ✅, current stage shows ▶, future stages show ○ |
| 5 | "What you need to do" panel shows any outstanding document requests |
| 6 | [Update Your Information] button opens the customer's fact-find edit form |
| 7 | Customer can complete and submit the fact-find form from the portal |
| 8 | [Upload Documents] link opens the correct upload folders for the customer's case |
| 9 | Customer can view messages from their advisor and send a reply |
| 10 | Customer cannot navigate to any other customer's data |
| 11 | Customer cannot see folders 05–13, Commission records, or internal stage notes |
| 12 | When case advances to a VisibleToCustomer stage, portal updates without manual intervention |

---

## What Phase 4 Does NOT Include

| Capability | Delivered in Phase / Version |
|---|---|
| Commission tracking | Phase 5 |
| KPI Dashboards | Phase 5 |
| E-Sign for Terms of Business | Future v1.1 |
| PDF Fact-Find export | Future v1.1 |
| Power Apps-based customer portal (richer UX) | Future consideration |
| Referrer self-service portal | Future v2.1 |
