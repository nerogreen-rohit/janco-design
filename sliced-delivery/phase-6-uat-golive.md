# Phase 6 — UAT & Go Live

**Goal:** End-to-end testing across all case types, staff training, production handover, and go-live.  
**Estimated duration:** 1 week (Week 8)  
**Depends on:** Phases 1–5 all complete  
**Business value:** Confirms the platform is fit for purpose before switching off existing Excel/Word processes.

---

## What This Phase Delivers

At the end of Phase 6:

1. All functional tests have passed (10 core test scenarios).
2. System owner has signed off the platform.
3. Staff (2–4 users) have been trained on the Staff App and key flows.
4. Real open cases have been migrated from spreadsheets (where applicable).
5. Production guest sharing links have been configured (replacing test links).
6. The SLA reminder schedule is confirmed and running.
7. The platform is live. Excel/Word processes are retired.
8. A 2-week post-launch priority support window is open.

---

## Testing — 10 Core Test Scenarios

### Test 1 — End-to-End BTL Case

**Scenario:** Full lifecycle from lead to completion for a Buy to Let case.

| Step | Expected Result |
|---|---|
| Create lead (BTL, £285,000) | LeadID generated (e.g. LEAD-0001) |
| Convert lead to case | CaseID generated (e.g. BTL-2026-0001); Fact-Find item created; folders created; welcome email sent |
| Customer completes fact-find | Flow 15 notifies case owner; fact-find Status updates to "In Progress" |
| Advance to Stage 3 (TOB) | Customer receives automated email; Stage History record created |
| Advance through stages 4–8 | Each stage logs to Stage History; customer notified on VisibleToCustomer stages |
| Request EPC Certificate document | Customer receives document request email |
| Customer uploads EPC | Document Request status = "Received"; case owner notified |
| Staff emails docs to lender | Email sent to lender SubmissionEmail; event logged in Stage History and Messages |
| Advance to Stage 13 (Completion) | Flow 16 fires: completion email sent, outstanding Doc Requests closed |
| Advance to Stage 14 (Post-Completion) | Commission records confirmed; Case status = "Completed" |

---

### Test 2 — End-to-End Bridging Case

**Scenario:** Bridging loan — skipping optional stages.

| Test | Expected Result |
|---|---|
| Advance to Stage 5 | Option to skip Stage 5 (Present Options) is available |
| Skip Stage 5 | Stage History records skip event with reason |
| Skip Stages 6 and 7 | Both recorded as skipped in Timeline tab |
| Stage 8 (Full Application) reachable | No blocker from skipped optional stages |

---

### Test 3 — Portfolio Client

**Scenario:** Create 2 cases for the same applicant.

| Test | Expected Result |
|---|---|
| Create Case 1 (BTL) for David Patel | Fact-find item created; independent to Case 2 |
| Create Case 2 (Refurb) for David Patel | Separate CaseID, separate Fact-find item, separate folders |
| `IsPortfolioLandlord = Yes` on Case 2 Fact-Find | Portfolio section fields become relevant |
| Filter Cases list by applicant email | Both cases visible |

---

### Test 4 — Commission Calculation

**Scenario:** Verify proc fee + AdvFee + referral fee records.

| Test | Expected Result |
|---|---|
| Assign lender with 1.5% proc fee to BTL case (£285,000) | Flow 11 calculates £4,275 expected proc fee; Commission record created |
| Add Advisory Fee (AdvFee) record of £1,500 | `TotalCommissionExpected` on Case updates to £5,775 |
| Add Referral Fee (15% split) for a known Referrer | Referral Fee Commission record created; linked to ReferrerID |
| Mark Proc Fee as received | `IsReceived = Yes`; `ReceivedDate` stamped; Commission Dashboard updates |

---

### Test 5 — SLA Reminder

**Scenario:** Confirm the daily reminder flow fires correctly.

| Test | Expected Result |
|---|---|
| Set `StageChangedDate` to a date past the stage SLA | Flow 9 (daily 08:00) detects overdue case |
| Check Teams notification | Case owner receives Teams alert with overdue case details |
| Set `StageChangedDate` to 1 day before SLA due | "At risk" notification sent |
| Case with no SLA breach | No notification sent for that case |

---

### Test 6 — Email to Lender

**Scenario:** Compile and send application documents to lender.

| Test | Expected Result |
|---|---|
| Select 3 files from case Documents tab | Files are selectable from case folder |
| Click [Email Lender] | Flow 12 sends formatted email to `LenderID.SubmissionEmail` with 3 attachments |
| Check Stage History | Email event logged with timestamp and files listed |
| Check Messages | System message created recording the lender email event |

---

### Test 7 — Customer Portal

**Scenario:** Guest access, fact-find, upload, and messages.

| Test | Expected Result |
|---|---|
| Open fact-find guest link in a private browser window | Form loads without M365 login |
| Complete all fact-find sections | Saves successfully; Flow 15 notifies case owner |
| Open document upload folder link | Folder accessible; file upload works |
| View portal progress page | Correct stages shown with customer-friendly names |
| Stage 4 (Product Research) | NOT visible on portal |
| View messages | Advisor messages visible; customer can reply |
| Log in as a different customer | Cannot see first customer's data |

---

### Test 8 — Dashboard Accuracy

**Scenario:** Verify all KPI figures match list data.

| Test | Expected Result |
|---|---|
| Count active cases in Cases list | Matches Pipeline Dashboard "Active Cases" count |
| Sum of LoanAmountRequired for active cases | Matches Pipeline Dashboard "Total Pipeline" value |
| Count completions this month | Matches Performance Dashboard "Completions" for current month |
| Fact-find Status = "Reviewed" count | Matches Compliance Dashboard "Complete & Reviewed" count |
| Sum of Commission records (IsReceived = Yes) | Matches Commission Dashboard "Received" total |

---

### Test 9 — Permissions

**Scenario:** Confirm customers cannot access any internal data.

| Test | Expected Result |
|---|---|
| Customer opens their fact-find link | Only their own item is accessible |
| Customer attempts to browse the Fact-Find Responses list | Access denied |
| Customer opens folder 05-Fact-Find directly | Access denied |
| Customer opens another case's upload folder | Access denied |
| Customer views portal progress | Internal stage names (e.g. "Product Research") are NOT shown |
| Customer views messages | Internal messages (Direction = "Internal") are NOT shown |

---

### Test 10 — Stage Skip Audit

**Scenario:** Verify skipped stages are correctly recorded.

| Test | Expected Result |
|---|---|
| Advance a Bridging case from Stage 4 directly to Stage 8 (skipping 5, 6, 7) | Each skipped stage creates a Stage History record with `IsSkipped = Yes` |
| View Timeline tab | Skipped stages are labelled clearly |
| Attempt to edit a Stage History record from the Staff App | No edit option available — read-only |

---

## Go Live Checklist

### Pre-Go-Live

| Task | Owner | Status |
|---|---|---|
| All 10 test scenarios passed | Developer + System Owner | ☐ |
| System owner sign-off on all screens and flows | System Owner | ☐ |
| Production SharePoint site confirmed (not test site) | IT Admin | ☐ |
| Guest sharing enabled at tenant level | IT Admin | ☐ |
| Guest link expiry set to appropriate duration | IT Admin | ☐ |
| Stage Configuration list — all 84 rows confirmed | System Owner | ☐ |
| Document Types list — all types entered | System Owner | ☐ |
| Lenders list — lender panel entered | System Owner | ☐ |
| Referrers list — known referrers entered | System Owner | ☐ |
| Counters list — all counters set to starting values | Developer | ☐ |
| SLA reminder flow schedule confirmed (daily 08:00) | Developer | ☐ |
| Test cases and test data removed from production | Developer | ☐ |

### Data Migration (if applicable)

| Task | Owner | Notes |
|---|---|---|
| Identify open cases to migrate from spreadsheets | System Owner | Only active/recent cases — no need to migrate closed cases |
| Create Case records manually for each open case | System Owner | Use Staff App case creation screen |
| Set current stage to match actual current stage | System Owner | Stage advancement is manual — no history import |
| Notify customers with new portal links | Case Owner | Send updated welcome email with new guest links |

> **Source:** `16-implementation-timeline.md` Phase 6, lines 118–125

---

### Staff Training

| Topic | Audience | Duration |
|---|---|---|
| Creating leads and converting to cases | All staff | 30 mins |
| Case Workspace — all 6 tabs | All staff | 45 mins |
| Document requests and email-to-lender | All staff | 20 mins |
| Commission records and Advisory Fees (AdvFee) | All staff | 20 mins |
| Dashboard interpretation | All staff + System Owner | 20 mins |
| Stage advancement and skipping | All staff | 15 mins |
| Admin screens (Stage Config, Doc Types) | System Owner only | 20 mins |

> **Source:** `16-implementation-timeline.md` line 121

---

### Go Live Day

| Task | Owner |
|---|---|
| Switch off test sharing links | Developer |
| Generate production sharing links for active cases | Developer / Case Owner |
| Send new portal welcome emails to active customers | Case Owner |
| Confirm SLA flow is running in production | Developer |
| Monitor for first 24 hours | Developer |
| System Owner confirms platform is operational | System Owner |

---

## Post-Launch Support

A **2-week priority support window** is provided after go-live.

During this window:
- Bug fixes are prioritised above any new feature work
- System Owner can raise issues directly with the developer
- Any flow failures are monitored and resolved within 1 business day
- Staff can request additional training on specific features

> **Source:** `16-implementation-timeline.md` line 125

---

## Post-Launch: Future Enhancements Roadmap

After successful go-live, the following enhancements are planned. These are additions to the live platform — no rework of existing lists or flows is required.

| Version | Enhancement | Priority | License Impact |
|---|---|---|---|
| **v1.1** | E-Sign integration (DocuSign/Adobe/Box) at Stage 3 | High | Power Automate Premium |
| **v1.1** | PDF export of completed Fact-Find (auto on Status = Submitted) | High | None / Encodian |
| **v1.2** | EPC API integration — auto-populate EpcRating from property postcode | Medium | Power Automate Premium (HTTP) |
| **v1.3** | SMS notifications to customer | Medium | Twilio connector |
| **v2.0** | Portfolio Client view (all cases per client side-by-side) | Medium | None |
| **v2.0** | Power BI dashboards (replacing in-app charts) | Low–Medium | Power BI Pro |
| **v2.1** | Clone Fact-Find for portfolio clients (copy personal/income fields) | Medium | None |
| **v2.1** | Referrer self-service portal | Low | SharePoint guest |
| **v3.0** | Full CRM integration (HubSpot / Dynamics) | Low | Additional licensing TBC |

> **Source:** `14-future-enhancements.md` §14.1

---

## Dependencies & Risk Register

| Dependency | Risk | Mitigation |
|---|---|---|
| IT admin enabling SharePoint guest sharing | Medium — requires tenant admin access | Raise at project start (Week 1) |
| Stage Configuration data entry (84 rows) | Low — manual but straightforward | System owner completes in Phase 1 |
| Customer email deliverability | Medium — automated emails may hit spam | Test sender reputation; use tenant email |
| Guest sharing link lifespan | Low — links expire if not configured | Set link expiry to "Never" or long duration |
| Power Automate Premium for e-sign/EPC | Low — not required for v1.0 | Budget for Premium when v1.1 is activated |

> **Source:** `16-implementation-timeline.md` §16.3
