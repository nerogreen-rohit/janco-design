# Task P6-05: Staff Training Plan and Materials

**Phase:** 6 — UAT & Go Live  
**Task:** Train all staff (2–4 users) on the Specialist Lending Platform before go-live  
**Priority:** 🔴 Critical  
**Estimated effort:** 3 hours total training time (across 7 sessions)  
**Depends on:** All Phases 1–5 complete; production site configured (P6-03)

---

## Overview

This training plan covers 7 sessions designed to give all staff the confidence to use the platform from day one. Sessions are structured to be delivered in order over 1–2 days. Each session includes a brief introduction, a guided walkthrough, and a hands-on exercise.

**Trainer:** Developer (or System Owner for Session 7)  
**Audience:** All staff unless noted otherwise  
**Environment:** Production site (or final staging) with test data

---

## Training Schedule Template

| Session | Topic | Duration | Audience | Scheduled Date | Scheduled Time | Completed |
|---|---|---|---|---|---|---|
| 1 | Creating leads and converting to cases | 30 mins | All staff | | | ☐ |
| 2 | Case Workspace — all 6 tabs | 45 mins | All staff | | | ☐ |
| 3 | Document requests and email-to-lender | 20 mins | All staff | | | ☐ |
| 4 | Commission records and Advisory Fees (AdvFee) | 20 mins | All staff | | | ☐ |
| 5 | Dashboard interpretation | 20 mins | All staff + System Owner | | | ☐ |
| 6 | Stage advancement and skipping | 15 mins | All staff | | | ☐ |
| 7 | Admin screens — Stage Config, Doc Types | 20 mins | System Owner only | | | ☐ |
| | **Total** | **2 hrs 50 mins** | | | | |

---

## Session 1: Creating Leads and Converting to Cases

**Duration:** 30 minutes  
**Audience:** All staff

### Key Points

- How to create a new lead from the Staff App home screen
- Required fields: Applicant name, email, phone, product type, loan amount
- How LeadID is auto-generated (e.g. LEAD-0001)
- Converting a lead to a full case — what happens automatically:
  - CaseID generated (e.g. BTL-2026-0001)
  - Fact-Find item created with guest share link
  - 13 document folders created; folders 01–04 shared with customer
  - Welcome email sent to customer with portal links
- When to use each product type (BTL, Bridging, Commercial, Dev Finance, Refurb, Auction)
- Setting the Case Owner

### Hands-On Exercise

1. Each staff member creates a test lead (use fictional data)
2. Convert the lead to a case
3. Verify the CaseID appears
4. Check that the welcome email was sent (use a test email address)
5. Open the customer portal link to see what the customer sees
6. Delete test data after the exercise

---

## Session 2: Case Workspace — All 6 Tabs

**Duration:** 45 minutes  
**Audience:** All staff

### Key Points

Walk through each of the 6 Case Workspace tabs:

| Tab | Purpose | Key Actions |
|---|---|---|
| **Overview** | Case summary, applicant details, current stage | Edit case details; view stage progress bar |
| **Fact-Find** | Read-only view of customer's fact-find | Review all sections; mark as "Reviewed" |
| **Documents** | All 13 document folders | Browse folders; upload files; view customer-uploaded files |
| **Timeline** | Stage History — every stage change and event | View chronological history; identify skipped stages |
| **Messages** | Communication log with customer | Send message to customer; view internal notes |
| **Commission** | Proc fee, AdvFee, referral fee records | View commission breakdown; mark as received |

### Hands-On Exercise

1. Open the test case created in Session 1
2. Navigate through all 6 tabs
3. On the Fact-Find tab: review the fact-find and mark it as "Reviewed"
4. On the Documents tab: upload a sample file to folder 06-Terms-of-Business
5. On the Messages tab: send a test message to the customer
6. On the Commission tab: view the commission record (if lender is assigned)

---

## Session 3: Document Requests and Email-to-Lender

**Duration:** 20 minutes  
**Audience:** All staff

### Key Points

- How to create a Document Request for a customer
  - Select the document type from the Document Types list
  - Customer receives an email notification with upload instructions
  - Track status: Requested → Received
- How to email documents to a lender
  - Select files from the Documents tab
  - Click [Email Lender] — sends to the lender's `SubmissionEmail`
  - Verify the event is logged in Stage History and Messages
- Understanding the email-to-lender flow (Flow 12)
- What the customer sees when they receive a document request

### Hands-On Exercise

1. Create a Document Request for "EPC Certificate" on the test case
2. Simulate the customer uploading a file to the correct folder
3. Verify the Document Request status updates to "Received"
4. Select 2 files from the Documents tab and click [Email Lender]
5. Verify the Stage History shows the email event

---

## Session 4: Commission Records and Advisory Fees (AdvFee)

**Duration:** 20 minutes  
**Audience:** All staff

### Key Points

- Understanding the 3 types of commission:
  - **Proc Fee** — percentage of loan, paid by lender (calculated automatically by Flow 11)
  - **Advisory Fee (AdvFee)** — flat fee charged to customer
  - **Referral Fee** — split percentage paid to referrer
- How Flow 11 automatically calculates the proc fee when a lender is assigned
- How to add an AdvFee record manually
- How to add a Referral Fee record and link to a Referrer
- Marking commission as received (`IsReceived = Yes`, `ReceivedDate` stamped)
- How `TotalCommissionExpected` on the Case record is updated
- Commission Dashboard overview

### Hands-On Exercise

1. Assign a lender to the test case (verify proc fee is calculated)
2. Add an Advisory Fee (AdvFee) of £1,000
3. Add a Referral Fee with a 15% split
4. Mark the proc fee as received
5. Check the Commission Dashboard reflects the changes

---

## Session 5: Dashboard Interpretation

**Duration:** 20 minutes  
**Audience:** All staff + System Owner

### Key Points

Walk through all 4 dashboards:

| Dashboard | Key Metrics | Used By |
|---|---|---|
| **Pipeline** | Active cases count, total pipeline value (£), cases by stage, cases by product type | All staff |
| **Performance** | Completions this month/quarter, average days per stage, conversion rate | System Owner |
| **Compliance** | Fact-find completion rate, fact-finds reviewed vs outstanding, document request status | All staff |
| **Commission** | Total expected vs received, proc fees outstanding, AdvFee totals, referral fees | System Owner |

- How to read the pipeline funnel
- How to identify bottleneck stages
- How to use compliance dashboard for fact-find follow-up
- How to track commission received vs expected

### Hands-On Exercise

1. Open the Pipeline Dashboard — identify the number of active cases and total pipeline value
2. Open the Performance Dashboard — find the average days per stage
3. Open the Compliance Dashboard — identify any outstanding fact-finds
4. Open the Commission Dashboard — check total commission expected vs received
5. Discuss: "If the Pipeline Dashboard shows 5 cases stuck at Stage 8, what does that tell us?"

---

## Session 6: Stage Advancement and Skipping

**Duration:** 15 minutes  
**Audience:** All staff

### Key Points

- How to advance a case to the next stage
  - Click the advance button on the Case Workspace Overview tab
  - Flow 7 creates a Stage History record
  - Flow 8 notifies the customer (if the stage is VisibleToCustomer)
- How to skip an optional stage
  - Stages 5, 6, and 7 are optional for Bridging cases
  - Skipping creates a Stage History record with `IsSkipped = Yes`
  - A reason must be provided for every skip
- How the Timeline tab displays skipped stages
- Why Stage History records are read-only (audit trail integrity)
- SLA implications: the SLA clock resets on each stage change

### Hands-On Exercise

1. Advance the test case from Stage 1 to Stage 2
2. Verify the Stage History record appears in the Timeline tab
3. Skip a stage (if using a Bridging case) and enter a reason
4. Verify the skipped stage is labelled in the Timeline
5. Try to edit a Stage History record — confirm it is read-only

---

## Session 7: Admin Screens — Stage Config, Doc Types

**Duration:** 20 minutes  
**Audience:** System Owner only

### Key Points

- **Stage Configuration list** — how to view and edit stage definitions:
  - 14 stages × 6 product types = 84 rows
  - Fields: StageName, StageNumber, ProductType, SLADays, IsOptional, VisibleToCustomer, CustomerFriendlyName
  - When to change SLA days (e.g. lender turnaround times change)
  - How changes affect existing cases (only new stage advances use updated config)

- **Document Types list** — how to manage document types:
  - Adding a new document type (e.g. a new lender requires a specific form)
  - Mapping to the correct folder (FolderNumber 01–13)
  - Setting IsRequired flags

- **Lenders list** — managing the lender panel:
  - Adding new lenders
  - Updating SubmissionEmail addresses
  - Adjusting ProcFeePercentage

- **Referrers list** — managing referrer records:
  - Adding new referrers
  - Setting DefaultSplitPercentage

- **Counters list** — understanding auto-ID generation:
  - When counters might need manual adjustment
  - What happens if a counter is accidentally modified

### Hands-On Exercise

1. Open the Stage Configuration list and review the BTL stages
2. Change the SLA for one stage (e.g. Stage 9 from 5 days to 7 days) — then revert
3. Open the Document Types list and add a test document type — then delete it
4. Open the Lenders list and verify a lender's SubmissionEmail is correct
5. Open the Counters list and review current values

---

## Training Delivery Tips

| Tip | Details |
|---|---|
| Use real (or realistic) test data | Staff engage better when they can see realistic case names and amounts |
| Screen share or projector | Demonstrate each feature live before the hands-on exercise |
| Record sessions | Use Teams recording for staff who cannot attend live |
| Provide a cheat sheet | A 1-page quick reference card with common actions (create case, advance stage, request document) |
| Allow questions throughout | These sessions are small-group — encourage interruptions |
| Clean up after exercises | Delete all test data created during training before go-live |

---

## Post-Training Verification

- [ ] All staff have attended Sessions 1–6
- [ ] System Owner has attended Session 7
- [ ] Each staff member has successfully created a test case
- [ ] Each staff member has successfully advanced a case through stages
- [ ] Each staff member has successfully created a document request
- [ ] System Owner can navigate and edit admin lists
- [ ] All training test data has been cleaned up
- [ ] Staff feel confident to use the platform (verbal confirmation)

---

## Sign-Off

| Role | Name | Date | Signature |
|---|---|---|---|
| Trainer (Developer) | | | |
| System Owner | | | |
| Staff Member 1 | | | |
| Staff Member 2 | | | |
| Staff Member 3 | | | |
| Staff Member 4 | | | |

> **All staff must complete training before the platform goes live.**
