# Task P6-02: Run Permissions & Security Verification

**Phase:** 6 — UAT & Go Live  
**Task:** Verify that permissions are correctly enforced across all actors and resources  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** All Phases 1–5 complete; test cases created for Customer A, Customer B, and Staff

---

## Overview

This task provides a comprehensive permissions test matrix. Every combination of actor and resource must be tested. Both **positive tests** (access should work) and **negative tests** (access should be denied) are covered.

**Who runs the tests:** Developer  
**Environment:** Production SharePoint site  
**Test accounts required:**

| Actor | Account Type | Description |
|---|---|---|
| Customer A | External guest | Has an active case (e.g. BTL-2026-0001) |
| Customer B | External guest | Has a different active case (e.g. BRG-2026-0002) |
| Staff Member | M365 user (Members group) | Standard case owner |
| Site Owner | M365 user (Owners group) | System administrator |

---

## Test URLs

Replace `[tenant]` and `[site]` with your actual values.

| Resource | URL Pattern |
|---|---|
| Cases list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Specialist%20Lending%20Cases` |
| Fact-Find Responses list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Fact-Find%20Responses` |
| Case Documents library | `https://[tenant].sharepoint.com/sites/[site]/Case%20Documents` |
| Folders 01–04 (customer) | `https://[tenant].sharepoint.com/sites/[site]/Case%20Documents/{CaseID}/01-Identity` (through 04) |
| Folders 05–13 (internal) | `https://[tenant].sharepoint.com/sites/[site]/Case%20Documents/{CaseID}/05-Fact-Find` (through 13) |
| Messages list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Messages` |
| Commission list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Commission` |
| Stage Configuration list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Stage%20Configuration` |
| Stage History list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Stage%20History` |
| Lenders list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Lenders` |
| Referrers list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Referrers` |
| Document Requests list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Document%20Requests` |
| Document Types list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Document%20Types` |
| Counters list | `https://[tenant].sharepoint.com/sites/[site]/Lists/Counters` |

---

## Permissions Matrix — Customer A

Customer A has guest access to their own case only (via shared links).

| # | Resource | Action | Expected Result | Pass/Fail |
|---|---|---|---|---|
| A-01 | Their own Fact-Find item (guest link) | Open and edit | ✅ Access granted — can view and edit their fact-find | ☐ |
| A-02 | Fact-Find Responses list (direct URL) | Browse all items | ❌ Access Denied | ☐ |
| A-03 | Customer B's Fact-Find item (direct URL) | View | ❌ Access Denied | ☐ |
| A-04 | Their folders 01-Identity | Open via guest link, upload file | ✅ Access granted — can upload files | ☐ |
| A-05 | Their folders 02-Income | Open via guest link, upload file | ✅ Access granted — can upload files | ☐ |
| A-06 | Their folders 03-Property | Open via guest link, upload file | ✅ Access granted — can upload files | ☐ |
| A-07 | Their folders 04-Bank-Statements | Open via guest link, upload file | ✅ Access granted — can upload files | ☐ |
| A-08 | Their folder 05-Fact-Find (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-09 | Their folder 06-Terms-of-Business (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-10 | Their folders 07–13 (direct URL) | Browse any internal folder | ❌ Access Denied | ☐ |
| A-11 | Customer B's folder 01-Identity (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-12 | Cases list (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-13 | Messages list (direct URL) | Browse all messages | ❌ Access Denied | ☐ |
| A-14 | Messages (portal view) | View their own messages | ✅ Only their messages visible; Internal messages hidden | ☐ |
| A-15 | Commission list (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-16 | Stage Configuration list (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-17 | Stage History list (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-18 | Lenders list (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-19 | Referrers list (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-20 | Document Requests list (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-21 | Counters list (direct URL) | Browse | ❌ Access Denied | ☐ |
| A-22 | Portal progress page | View stages | ✅ Customer-friendly stage names only; internal stages hidden | ☐ |
| A-23 | Case Documents library root (direct URL) | Browse | ❌ Access Denied | ☐ |

---

## Permissions Matrix — Customer B (Cross-Case Isolation)

Customer B has guest access to their own case only. This section verifies they cannot access Customer A's data.

| # | Resource | Action | Expected Result | Pass/Fail |
|---|---|---|---|---|
| B-01 | Their own Fact-Find item (guest link) | Open and edit | ✅ Access granted | ☐ |
| B-02 | Customer A's Fact-Find item (direct URL) | View | ❌ Access Denied | ☐ |
| B-03 | Customer A's folder 01-Identity (direct URL) | Browse | ❌ Access Denied | ☐ |
| B-04 | Customer A's folder 02-Income (direct URL) | Browse | ❌ Access Denied | ☐ |
| B-05 | Their folders 01–04 (guest links) | Upload files | ✅ Access granted | ☐ |
| B-06 | Their folders 05–13 (direct URL) | Browse | ❌ Access Denied | ☐ |
| B-07 | Portal messages | View | ✅ Only their messages visible | ☐ |
| B-08 | Customer A's messages (via URL manipulation) | View | ❌ Access Denied or no data returned | ☐ |

---

## Permissions Matrix — Staff Member

Staff members are in the M365 Members group with Edit permissions on the site.

| # | Resource | Action | Expected Result | Pass/Fail |
|---|---|---|---|---|
| S-01 | Cases list | View, create, edit cases | ✅ Full edit access | ☐ |
| S-02 | Fact-Find Responses list | View all items, mark as Reviewed | ✅ Full edit access | ☐ |
| S-03 | Case Documents library | View all folders, upload files, delete files | ✅ Full access to all case folders | ☐ |
| S-04 | Messages list | Create, view, edit messages | ✅ Full edit access | ☐ |
| S-05 | Commission list | Create, view, edit records | ✅ Full edit access | ☐ |
| S-06 | Stage History list | View records | ✅ Read access (records created by flows only) | ☐ |
| S-07 | Stage Configuration list | View stages | ✅ Read access | ☐ |
| S-08 | Stage Configuration list | Edit stages | ❌ Access Denied (read-only for Members) | ☐ |
| S-09 | Document Requests list | Create, view, edit | ✅ Full edit access | ☐ |
| S-10 | Lenders list | View lenders | ✅ Read access | ☐ |
| S-11 | Referrers list | View referrers | ✅ Read access | ☐ |
| S-12 | Counters list | View counters | ✅ Read access (updated by flows only) | ☐ |
| S-13 | Document Types list | View document types | ✅ Read access | ☐ |

---

## Permissions Matrix — Site Owner

Site Owner has Full Control over the entire site.

| # | Resource | Action | Expected Result | Pass/Fail |
|---|---|---|---|---|
| O-01 | Cases list | Full control (view, create, edit, delete) | ✅ Full control | ☐ |
| O-02 | Fact-Find Responses list | Full control | ✅ Full control | ☐ |
| O-03 | Case Documents library | Full control | ✅ Full control | ☐ |
| O-04 | Stage Configuration list | Full control (add, edit, delete stages) | ✅ Full control | ☐ |
| O-05 | Stage History list | Full control | ✅ Full control | ☐ |
| O-06 | Commission list | Full control | ✅ Full control | ☐ |
| O-07 | Messages list | Full control | ✅ Full control | ☐ |
| O-08 | Lenders list | Full control (manage lender panel) | ✅ Full control | ☐ |
| O-09 | Referrers list | Full control (manage referrer records) | ✅ Full control | ☐ |
| O-10 | Document Types list | Full control (manage document types) | ✅ Full control | ☐ |
| O-11 | Counters list | Full control (reset counters if needed) | ✅ Full control | ☐ |
| O-12 | Document Requests list | Full control | ✅ Full control | ☐ |
| O-13 | Site permissions | Manage site access and groups | ✅ Full control | ☐ |

---

## Summary Tracker

| Actor | Total Tests | Passed | Failed | Status |
|---|---|---|---|---|
| Customer A | 23 | ___ | ___ | ☐ |
| Customer B | 8 | ___ | ___ | ☐ |
| Staff Member | 13 | ___ | ___ | ☐ |
| Site Owner | 13 | ___ | ___ | ☐ |
| **Total** | **57** | ___ | ___ | |

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Customer can browse the full Fact-Find list | The share link is a list link, not an item link — regenerate with the correct flow |
| Customer can access folders 05–13 | Guest sharing was incorrectly applied to the parent folder — reshare only folders 01–04 |
| Staff member can edit Stage Configuration | Break inheritance on the list and set Members group to Read only |
| Customer can see internal messages | Portal message filter is not excluding Direction = "Internal" — fix the portal query |
| Guest link not working | Check tenant-level guest sharing is enabled; verify link has not expired |
| "Access Denied" for staff | Verify staff user is in the Members group via Site Settings → Site permissions |

---

## Sign-Off

| Role | Name | Date | Signature |
|---|---|---|---|
| Developer | | | |
| System Owner | | | |

> **All 57 permission tests must pass.** Any failed negative test (access granted when it should be denied) is a critical security issue and must be resolved before go-live.
