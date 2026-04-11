# Task P6-04: Data Migration Guide

**Phase:** 6 — UAT & Go Live  
**Task:** Migrate open cases from existing Excel/Word spreadsheets to the platform  
**Priority:** 🟡 High  
**Estimated effort:** 1–3 hours (depends on number of open cases)  
**Depends on:** P6-03 (Production configuration) complete

---

## Overview

This guide covers migrating active/recent cases from the current Excel/Word process to the new Specialist Lending Platform. Only **open and active cases** need to be migrated — closed or archived cases do not need to be brought across.

**Who performs the migration:** System Owner (with Developer support)  
**Environment:** Production SharePoint site

---

## Step 1 — Identify Cases to Migrate

Review the current spreadsheets and identify cases that qualify for migration.

### Migration Criteria

| Criteria | Include? |
|---|---|
| Active case — currently in progress | ✅ Yes |
| Recent enquiry — less than 30 days old, not yet started | ✅ Yes |
| Case on hold — expected to resume | ✅ Yes (set CaseStatus = "On Hold") |
| Completed case | ❌ No — no need to migrate |
| Withdrawn or declined case | ❌ No — no need to migrate |
| Archived case (older than 6 months, no activity) | ❌ No — no need to migrate |

### Migration Source Inventory

List all cases to migrate before starting:

| # | Applicant Name | Product Type | Current Stage (Approx.) | Source File/Sheet | Migrate? |
|---|---|---|---|---|---|
| 1 | | | | | ☐ |
| 2 | | | | | ☐ |
| 3 | | | | | ☐ |
| 4 | | | | | ☐ |
| 5 | | | | | ☐ |

> Add more rows as needed.

---

## Step 2 — Create Each Case on the Platform

For each case identified in Step 1, follow these steps:

### 2.1 Create the Case

1. Open the **Staff App** → **New Case** form
2. Enter the applicant details:
   - Applicant 1: First Name, Last Name, Email, Phone
   - Applicant 2 (if applicable): First Name, Last Name, Email
3. Enter the property and loan details:
   - Property Address, Postcode, Property Type
   - Estimated Value, Purchase Price, Loan Amount Required
   - LTV, Interest Rate Type, Term, Repayment Type
4. Select the correct **Product Type** (BTL, Bridging, Commercial, etc.)
5. Set **Enquiry Date** to the original enquiry date from the spreadsheet
6. Click **Create**
7. Wait for automated flows to complete:
   - CaseID generated
   - Fact-Find item created
   - Document folders created
   - Welcome email queued (may need to suppress — see note below)

> **Note:** The welcome email will fire automatically. If the customer has already been onboarded, you may want to temporarily disable Flow 6 (Welcome Email) during migration, then re-enable it afterwards. Alternatively, warn the customer in advance that they will receive a new portal link.

### 2.2 Set the Current Stage

After case creation, the case starts at Stage 1 (Lead/Enquiry). To set it to the correct current stage:

1. Open the case in the **Case Workspace**
2. Use the **Stage Advancement** feature to advance to the correct stage
3. For each intermediate stage, you can either:
   - **Advance through each stage** (creates a full Stage History trail), or
   - **Skip stages** to reach the target stage faster (records skips with `IsSkipped = Yes`)

> **Recommendation:** Skip stages for migrated cases, with the skip reason set to "Pre-migration — case was at this stage before platform launch". This is faster and clearly marks the case as migrated.

### 2.3 Upload Existing Documents (Optional)

If the case has existing documents (ID, income proof, valuation reports, etc.):

1. Open the case **Documents tab**
2. Upload files to the correct subfolders (01-Identity, 02-Income, etc.)
3. This step is optional — documents can also be uploaded later

### 2.4 Set Additional Fields

After creation, edit the case record to set any additional data:

| Field | Action |
|---|---|
| LenderID | Set if a lender has already been selected |
| ReferrerID | Set if the case was referred |
| IsPortfolioLandlord | Set to Yes if applicable |
| TargetCompletionDate | Set if known |
| StageNotes | Add any relevant context about the case history |

---

## Step 3 — Notify Customers

For each migrated case, notify the customer about their new portal access:

1. The welcome email (Flow 6) will have sent the customer their portal links automatically
2. If welcome email was disabled during migration, manually send the customer:
   - Their Fact-Find guest link
   - Their document upload folder links (01–04)
   - A brief explanation that the portal is their new way to interact
3. If the customer has already completed their fact-find (in the old process), update the Fact-Find Status to "Submitted" or "Reviewed" as appropriate

---

## Step 4 — Post-Migration Verification

After all cases have been migrated, verify each one:

### Per-Case Verification Checklist

For each migrated case, confirm:

- [ ] CaseID is correctly generated
- [ ] Product Type is correct
- [ ] Applicant details match the source spreadsheet
- [ ] Current Stage is set to the correct stage
- [ ] Stage History shows migration skip records (if applicable)
- [ ] Fact-Find item exists and is linked to the case
- [ ] Document folders (01–13) exist
- [ ] Customer portal links work (test in private browser)
- [ ] Customer has been notified with new links

### Aggregate Verification

- [ ] Total migrated cases in Cases list matches the migration inventory count
- [ ] Pipeline Dashboard "Active Cases" count is correct
- [ ] No duplicate CaseIDs exist
- [ ] Counters list values are above the highest migrated CaseID number
- [ ] Flow 6 (Welcome Email) re-enabled (if it was disabled)

---

## Per-Case Migration Tracker

Use this table to track the migration of each case:

| # | Source Case / Applicant | Target CaseID | Product Type | Stage Set | Customer Notified | Verified | Notes |
|---|---|---|---|---|---|---|---|
| 1 | | | | ☐ | ☐ | ☐ | |
| 2 | | | | ☐ | ☐ | ☐ | |
| 3 | | | | ☐ | ☐ | ☐ | |
| 4 | | | | ☐ | ☐ | ☐ | |
| 5 | | | | ☐ | ☐ | ☐ | |
| 6 | | | | ☐ | ☐ | ☐ | |
| 7 | | | | ☐ | ☐ | ☐ | |
| 8 | | | | ☐ | ☐ | ☐ | |
| 9 | | | | ☐ | ☐ | ☐ | |
| 10 | | | | ☐ | ☐ | ☐ | |

> Add more rows as needed.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Duplicate CaseID generated | Check Counters list — the counter value may be lower than expected. Increase it manually and recreate the case |
| Welcome email sent to customer prematurely | Contact the customer and explain the new portal. No data is exposed — the links are new |
| Fact-Find already completed in old process | Set Fact-Find Status to "Submitted" and staff can mark as "Reviewed" |
| Stage advancement not working | Ensure Stage Configuration has the correct rows for the product type |
| Documents in wrong folder | Move files between folders in the Case Documents library |
| Customer cannot access portal link | Check guest sharing is enabled at tenant level; verify the link has not expired |

---

## Sign-Off

| Role | Name | Date | Signature |
|---|---|---|---|
| System Owner | | | |
| Developer | | | |

> **Migration is complete when all cases are tracked, verified, and customers have been notified.**
