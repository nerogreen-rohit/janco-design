# Task P6-03: Production Configuration Checklist

**Phase:** 6 — UAT & Go Live  
**Task:** Complete all production configuration steps before go-live  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–4 hours  
**Depends on:** All Phases 1–5 complete; production SharePoint site provisioned

---

## Overview

This checklist ensures the production environment is fully configured before the platform goes live. Every item must be completed and verified before system owner sign-off.

---

## 1. Site & Tenant Configuration

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 1.1 | Production SharePoint site confirmed (not test site) | IT Admin | ☐ | |
| 1.2 | Site URL documented: `https://[tenant].sharepoint.com/sites/[production-site]` | IT Admin | ☐ | |
| 1.3 | Guest sharing enabled at tenant level (SharePoint Admin Centre → Sharing) | IT Admin | ☐ | |
| 1.4 | Guest link expiry configured (set to appropriate duration or "Never") | IT Admin | ☐ | |
| 1.5 | External sharing set to "Anyone" or "New and existing guests" at site level | IT Admin | ☐ | |
| 1.6 | Staff users added to Members group | IT Admin | ☐ | |
| 1.7 | System Owner added to Owners group | IT Admin | ☐ | |

---

## 2. SharePoint Lists — Reference Data

### 2.1 Stage Configuration List

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 2.1.1 | Stage Configuration list created in production | Developer | ☐ | |
| 2.1.2 | All 14 stages entered (×6 product types = 84 rows total) | System Owner | ☐ | |
| 2.1.3 | Verify SLA days are set for each stage | System Owner | ☐ | |
| 2.1.4 | Verify `IsOptional` flags are correct (stages 5, 6, 7 for Bridging) | System Owner | ☐ | |
| 2.1.5 | Verify `VisibleToCustomer` flags are correct | System Owner | ☐ | |
| 2.1.6 | Verify `CustomerFriendlyName` is populated for visible stages | System Owner | ☐ | |

### 2.2 Document Types List

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 2.2.1 | Document Types list created in production | Developer | ☐ | |
| 2.2.2 | All 25 document types entered with correct folder mappings | System Owner | ☐ | |
| 2.2.3 | Verify each document type has a `FolderNumber` assigned (01–13) | System Owner | ☐ | |
| 2.2.4 | Verify `IsRequired` flags match business requirements | System Owner | ☐ | |

### 2.3 Lenders List

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 2.3.1 | Lenders list created in production | Developer | ☐ | |
| 2.3.2 | Lender panel data entered (all active lenders) | System Owner | ☐ | |
| 2.3.3 | `SubmissionEmail` verified for each lender | System Owner | ☐ | |
| 2.3.4 | `ProcFeePercentage` set for each lender | System Owner | ☐ | |

### 2.4 Referrers List

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 2.4.1 | Referrers list created in production | Developer | ☐ | |
| 2.4.2 | Known referrers entered with contact details | System Owner | ☐ | |
| 2.4.3 | `DefaultSplitPercentage` set for each referrer | System Owner | ☐ | |

### 2.5 Counters List

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 2.5.1 | Counters list created in production | Developer | ☐ | |
| 2.5.2 | All 7 counter rows created (6 product types + LEAD) | Developer | ☐ | |
| 2.5.3 | Starting values confirmed for each counter | Developer | ☐ | |

**Counter starting values:**

| Counter Name | Prefix | Starting Value |
|---|---|---|
| BTL | BTL | 0 |
| Bridging | BRG | 0 |
| Commercial | COM | 0 |
| Dev Finance | DEV | 0 |
| Refurb | REF | 0 |
| Auction | AUC | 0 |
| LEAD | LEAD | 0 |

> **Note:** If migrating open cases, set starting values above the highest migrated CaseID number to avoid duplicates.

---

## 3. Power Automate Flows

All 16 flows must be verified in the production environment.

| # | Flow | Owner | Status | Date Completed |
|---|---|---|---|---|
| 3.1 | Flow 1 — Lead ID Generation | Developer | ☐ | |
| 3.2 | Flow 2 — Lead to Case Conversion | Developer | ☐ | |
| 3.3 | Flow 3 — CaseID Generation | Developer | ☐ | |
| 3.4 | Flow 4 — Create Fact-Find Item + Guest Link | Developer | ☐ | |
| 3.5 | Flow 5 — Create Folders + Share Folders 01–04 | Developer | ☐ | |
| 3.6 | Flow 6 — Welcome Email | Developer | ☐ | |
| 3.7 | Flow 7 — Stage Advancement | Developer | ☐ | |
| 3.8 | Flow 8 — Customer Stage Notification | Developer | ☐ | |
| 3.9 | Flow 9 — SLA Reminder (Daily 08:00) | Developer | ☐ | |
| 3.10 | Flow 10 — Document Request to Customer | Developer | ☐ | |
| 3.11 | Flow 11 — Commission Calculation | Developer | ☐ | |
| 3.12 | Flow 12 — Email to Lender | Developer | ☐ | |
| 3.13 | Flow 13 — Document Upload Notification | Developer | ☐ | |
| 3.14 | Flow 14 — Customer Message Notification | Developer | ☐ | |
| 3.15 | Flow 15 — Fact-Find Update Notification | Developer | ☐ | |
| 3.16 | Flow 16 — Completion (email + close Doc Requests) | Developer | ☐ | |

**Verification steps for each flow:**
1. Flow exists in the production environment (not test)
2. Flow is turned **On**
3. Flow connections use production service accounts (not personal test accounts)
4. Flow triggers point to production lists/libraries
5. Test-trigger the flow and verify it completes successfully

---

## 4. SLA Reminder Schedule

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 4.1 | Flow 9 recurrence trigger set to daily at 08:00 | Developer | ☐ | |
| 4.2 | Flow 9 points to production Cases list | Developer | ☐ | |
| 4.3 | Flow 9 Teams notification targets correct team/channel | Developer | ☐ | |
| 4.4 | Test run confirms notifications fire correctly | Developer | ☐ | |

---

## 5. Cleanup

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 5.1 | All test cases removed from production Cases list | Developer | ☐ | |
| 5.2 | All test Fact-Find items removed | Developer | ☐ | |
| 5.3 | All test document folders removed from Case Documents | Developer | ☐ | |
| 5.4 | All test messages removed | Developer | ☐ | |
| 5.5 | All test commission records removed | Developer | ☐ | |
| 5.6 | All test Stage History records removed | Developer | ☐ | |
| 5.7 | All test Document Requests removed | Developer | ☐ | |
| 5.8 | Counters reset to starting values (or adjusted for data migration) | Developer | ☐ | |
| 5.9 | Recycle bin emptied (Site Contents → Recycle bin) | Developer | ☐ | |

---

## 6. Final Verification

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 6.1 | Create one test case in production and verify full flow | Developer | ☐ | |
| 6.2 | Delete the verification test case and clean up | Developer | ☐ | |
| 6.3 | All 16 flows show as "On" in Power Automate | Developer | ☐ | |
| 6.4 | Production site URL bookmarked and shared with staff | System Owner | ☐ | |

---

## Sign-Off

| Role | Name | Date | Signature |
|---|---|---|---|
| Developer | | | |
| IT Admin | | | |
| System Owner | | | |

> **All items must be completed and verified before proceeding to data migration and go-live.**
