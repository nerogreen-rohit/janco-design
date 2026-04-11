# Task P6-01: Run All 10 Core Test Scenarios

**Phase:** 6 — UAT & Go Live  
**Task:** Run all 10 core test scenarios to validate end-to-end platform functionality  
**Priority:** 🔴 Critical  
**Estimated effort:** 4–6 hours  
**Depends on:** All Phases 1–5 complete; production site provisioned

---

## Overview

These 10 test scenarios cover every major feature of the Specialist Lending Platform. Each scenario must pass before the system owner can sign off and the platform can go live.

**Who runs the tests:** Developer and System Owner together  
**Environment:** Production SharePoint site (or final staging if production is not yet available)  
**Record keeping:** Mark each test step as Pass / Fail in the tracker tables below

---

## Test 1 — End-to-End BTL Case

**Scenario:** Full lifecycle from lead to completion for a Buy to Let case.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 1.1 | Create lead (BTL, £285,000) | LeadID generated (e.g. LEAD-0001) | ☐ |
| 1.2 | Convert lead to case | CaseID generated (e.g. BTL-2026-0001); Fact-Find item created; 13 folders created; welcome email sent | ☐ |
| 1.3 | Customer completes fact-find via guest link | Flow 15 notifies case owner via Teams; Fact-Find Status updates to "In Progress" | ☐ |
| 1.4 | Advance to Stage 3 (Terms of Business) | Customer receives automated email; Stage History record created with timestamp | ☐ |
| 1.5 | Advance through Stages 4–8 | Each stage logs a Stage History record; customer notified on VisibleToCustomer stages | ☐ |
| 1.6 | Request EPC Certificate document from customer | Customer receives document request email with upload link | ☐ |
| 1.7 | Customer uploads EPC to the correct folder | Document Request status = "Received"; case owner receives notification | ☐ |
| 1.8 | Staff emails documents to lender | Email sent to lender `SubmissionEmail` address; event logged in Stage History and Messages | ☐ |
| 1.9 | Advance to Stage 13 (Completion) | Flow 16 fires: completion email sent to customer, outstanding Document Requests auto-closed | ☐ |
| 1.10 | Advance to Stage 14 (Post-Completion) | Commission records confirmed; Case status = "Completed" | ☐ |

---

## Test 2 — End-to-End Bridging Case

**Scenario:** Bridging loan lifecycle — skipping optional stages 5, 6, and 7.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 2.1 | Create a Bridging case (£500,000 loan, 12-month term) | CaseID generated (e.g. BRG-2026-0001); Fact-Find + folders created | ☐ |
| 2.2 | Advance to Stage 4 (Product Research) | Stage History record created | ☐ |
| 2.3 | Advance to Stage 5 — option to skip is available | Skip option displayed in stage advancement UI | ☐ |
| 2.4 | Skip Stage 5 (Present Options) | Stage History records skip event with `IsSkipped = Yes` and reason | ☐ |
| 2.5 | Skip Stages 6 and 7 | Both recorded as skipped in Timeline tab with individual reasons | ☐ |
| 2.6 | Stage 8 (Full Application) is reachable | No blocker from skipped optional stages; case advances normally | ☐ |
| 2.7 | Continue through remaining stages to completion | All stages 8–14 complete normally | ☐ |

---

## Test 3 — Portfolio Client

**Scenario:** Create 2 cases for the same applicant to confirm data isolation.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 3.1 | Create Case 1 (BTL) for David Patel (david@example.com) | CaseID generated; Fact-Find item created; independent folders created | ☐ |
| 3.2 | Create Case 2 (Refurb) for David Patel (same email) | Separate CaseID; separate Fact-Find item; separate document folders | ☐ |
| 3.3 | Set `IsPortfolioLandlord = Yes` on Case 2 Fact-Find | Portfolio section fields become relevant and display correctly | ☐ |
| 3.4 | Filter Cases list by applicant email | Both cases visible in filtered view | ☐ |
| 3.5 | Verify Case 1 fact-find is independent of Case 2 | Editing one fact-find does not affect the other | ☐ |
| 3.6 | Verify Case 1 folders are independent of Case 2 | Documents uploaded to Case 1 do not appear in Case 2 | ☐ |

---

## Test 4 — Commission Calculation

**Scenario:** Verify proc fee + Advisory Fee (AdvFee) + referral fee records.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 4.1 | Assign lender with 1.5% proc fee to BTL case (£285,000) | Flow 11 calculates £4,275 expected proc fee; Commission record created | ☐ |
| 4.2 | Add Advisory Fee (AdvFee) record of £1,500 | `TotalCommissionExpected` on Case updates to £5,775 | ☐ |
| 4.3 | Add Referral Fee (15% split) for a known Referrer | Referral Fee Commission record created; linked to correct ReferrerID | ☐ |
| 4.4 | Mark Proc Fee as received | `IsReceived = Yes`; `ReceivedDate` stamped with current date | ☐ |
| 4.5 | Verify Commission Dashboard | Dashboard updates to reflect new received total | ☐ |
| 4.6 | Verify Case record totals | `TotalCommissionExpected` and received figures match Commission records | ☐ |

---

## Test 5 — SLA Reminder

**Scenario:** Confirm the daily SLA reminder flow fires correctly.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 5.1 | Set `StageChangedDate` to a date past the stage SLA | Flow 9 (daily 08:00 schedule) detects the overdue case | ☐ |
| 5.2 | Check Teams notification | Case owner receives Teams alert with overdue case details (CaseID, stage, days overdue) | ☐ |
| 5.3 | Set `StageChangedDate` to 1 day before SLA due date | "At risk" notification sent to case owner | ☐ |
| 5.4 | Verify case with no SLA breach | No notification sent for that case | ☐ |
| 5.5 | Verify multiple overdue cases | Each overdue case generates its own notification | ☐ |

---

## Test 6 — Email to Lender

**Scenario:** Compile and send application documents to the lender.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 6.1 | Navigate to case Documents tab | All 13 folders visible with uploaded test documents | ☐ |
| 6.2 | Select 3 files from case document folders | Files are selectable from the document selection UI | ☐ |
| 6.3 | Click [Email Lender] | Flow 12 sends formatted email to `LenderID.SubmissionEmail` with 3 file attachments | ☐ |
| 6.4 | Check Stage History | Email event logged with timestamp and list of files sent | ☐ |
| 6.5 | Check Messages | System message created recording the lender email event | ☐ |
| 6.6 | Verify email received at lender address | Email contains all 3 attachments in correct format | ☐ |

---

## Test 7 — Customer Portal

**Scenario:** Guest access, fact-find completion, document upload, and message isolation.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 7.1 | Open fact-find guest link in a private/incognito browser window | Form loads without requiring M365 login | ☐ |
| 7.2 | Complete all fact-find sections and submit | Saves successfully; Flow 15 notifies case owner via Teams | ☐ |
| 7.3 | Open document upload folder link | Folder is accessible; file upload works | ☐ |
| 7.4 | View portal progress page | Correct stages shown with customer-friendly names | ☐ |
| 7.5 | Verify Stage 4 (Product Research) is NOT visible on portal | Internal-only stage is hidden from customer view | ☐ |
| 7.6 | View messages on portal | Advisor messages visible; customer can reply | ☐ |
| 7.7 | Log in as a different customer (Customer B) | Cannot see Customer A's data, fact-find, folders, or messages | ☐ |

---

## Test 8 — Dashboard Accuracy

**Scenario:** Verify all 4 KPI dashboards match underlying list data.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 8.1 | Count active cases in Cases list | Matches Pipeline Dashboard "Active Cases" count | ☐ |
| 8.2 | Sum `LoanAmountRequired` for active cases | Matches Pipeline Dashboard "Total Pipeline" value | ☐ |
| 8.3 | Count completions this month in Cases list | Matches Performance Dashboard "Completions" for current month | ☐ |
| 8.4 | Count Fact-Find items with Status = "Reviewed" | Matches Compliance Dashboard "Complete & Reviewed" count | ☐ |
| 8.5 | Sum Commission records where `IsReceived = Yes` | Matches Commission Dashboard "Received" total | ☐ |
| 8.6 | Verify dashboard refresh | Dashboards update after a new case is added or status changes | ☐ |

---

## Test 9 — Permissions

**Scenario:** Confirm customers cannot access any internal data.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 9.1 | Customer opens their fact-find guest link | Only their own Fact-Find item is accessible and editable | ☐ |
| 9.2 | Customer attempts to browse the Fact-Find Responses list | Access Denied | ☐ |
| 9.3 | Customer opens folder 05-Fact-Find directly via URL | Access Denied | ☐ |
| 9.4 | Customer opens another case's upload folder via URL | Access Denied | ☐ |
| 9.5 | Customer views portal progress page | Internal stage names (e.g. "Product Research") are NOT shown | ☐ |
| 9.6 | Customer views messages on portal | Internal messages (Direction = "Internal") are NOT shown | ☐ |
| 9.7 | Customer attempts to access Cases list via URL | Access Denied | ☐ |
| 9.8 | Customer attempts to access Stage Configuration list | Access Denied | ☐ |

---

## Test 10 — Stage Skip Audit

**Scenario:** Verify skipped stages are correctly recorded with full audit trail.

| # | Test Step | Expected Result | Pass/Fail |
|---|---|---|---|
| 10.1 | Advance a Bridging case from Stage 4 directly to Stage 8 (skipping 5, 6, 7) | Each skipped stage creates a Stage History record with `IsSkipped = Yes` | ☐ |
| 10.2 | View Timeline tab for the case | Skipped stages are labelled clearly (e.g. "Skipped — Stage 5: Present Options") | ☐ |
| 10.3 | Verify skip reason is recorded | Each skipped Stage History record includes a SkipReason value | ☐ |
| 10.4 | Attempt to edit a Stage History record from the Staff App | No edit option available — Stage History is read-only | ☐ |
| 10.5 | Verify stage numbering after skips | CurrentStageNumber on Case reflects Stage 8, not a sequential count | ☐ |

---

## Master Pass/Fail Tracker

| Test # | Test Name | Total Steps | Passed | Failed | Status |
|---|---|---|---|---|---|
| 1 | End-to-End BTL Case | 10 | ___ | ___ | ☐ |
| 2 | End-to-End Bridging Case | 7 | ___ | ___ | ☐ |
| 3 | Portfolio Client | 6 | ___ | ___ | ☐ |
| 4 | Commission Calculation | 6 | ___ | ___ | ☐ |
| 5 | SLA Reminder | 5 | ___ | ___ | ☐ |
| 6 | Email to Lender | 6 | ___ | ___ | ☐ |
| 7 | Customer Portal | 7 | ___ | ___ | ☐ |
| 8 | Dashboard Accuracy | 6 | ___ | ___ | ☐ |
| 9 | Permissions | 8 | ___ | ___ | ☐ |
| 10 | Stage Skip Audit | 5 | ___ | ___ | ☐ |
| **Total** | | **66** | ___ | ___ | |

---

## Sign-Off

| Role | Name | Date | Signature |
|---|---|---|---|
| Developer | | | |
| System Owner | | | |

> **All 10 tests must pass before proceeding to go-live.** Any failed test must be resolved and re-run before sign-off.
