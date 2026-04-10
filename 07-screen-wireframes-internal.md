[← Back to Index](00-index.md)

# 7. Screen Wireframes — Internal Staff App

All wireframes use ASCII art to represent screen layouts. The internal staff app is a Power Apps Canvas App accessible to 2–4 licensed staff members.

---

## 7.1 Dashboard Screen

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  SPECIALIST LENDING HUB          [Staff Member Name]  [Logout]   ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Dashboard]  [Leads]  [Cases]  [Commissions]  [Lenders]  [Admin]    ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  PIPELINE SUMMARY                          TODAY'S TASKS             ║
║  ┌─────────────┬──────────┬────────────┐   ┌──────────────────────┐  ║
║  │  Active     │ 12 Cases │  £4.2m     │   │ ⚠ Chase valuation    │  ║
║  │  Cases      │          │  Pipeline  │   │   BTL-2026-0004      │  ║
║  ├─────────────┼──────────┼────────────┤   │ ⚠ Send DIP docs      │  ║
║  │  New Leads  │  3 Today │  8 Week    │   │   BRIDGE-2026-0002   │  ║
║  ├─────────────┼──────────┼────────────┤   │ ✅ TOB signed        │  ║
║  │  Completions│  2 Month │  £18,400   │   │   BTL-2026-0007      │  ║
║  │             │          │  Fees      │   └──────────────────────┘  ║
║  └─────────────┴──────────┴────────────┘                             ║
║                                                                      ║
║  CASES BY STAGE                          CASES BY PRODUCT            ║
║  ┌────────────────────────────────────┐  ┌────────────────────────┐  ║
║  │  Fact-Find          ████░░░░  4    │  │  BTL         ██████  6 │  ║
║  │  Full Application   ███░░░░░  3    │  │  Bridging    ████    4 │  ║
║  │  Underwriting       ██░░░░░░  2    │  │  Commercial  ██      2 │  ║
║  │  Legals             █░░░░░░░  1    │  │  Dev Finance █       1 │  ║
║  │  Other              ██░░░░░░  2    │  │  Other       ░       0 │  ║
║  └────────────────────────────────────┘  └────────────────────────┘  ║
║                                                                      ║
║  SLA ALERTS                                                          ║
║  ┌──────────────────────────────────────────────────────────────┐    ║
║  │  🔴 BTL-2026-0003 — Underwriting — 5 days overdue            │    ║
║  │  🟡 BRIDGE-2026-0001 — Valuation — due tomorrow              │    ║
║  │  🟢 BTL-2026-0008 — Fact-Find — on track                     │    ║
║  └──────────────────────────────────────────────────────────────┘    ║ 
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7.2 Leads Screen

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  SPECIALIST LENDING HUB          [Staff Member Name]  [Logout]   ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Dashboard]  [Leads]  [Cases]  [Commissions]  [Lenders]  [Admin]    ║
╠══════════════════════════════════════════════════════════════════════╣
║  LEADS                                              [+ New Lead]     ║
║                                                                      ║
║  Filter: [All Status ▼]  [All Products ▼]  [All Assigned ▼]  [🔍]    ║
║                                                                      ║
║  ┌────────────┬──────────────┬──────────┬──────────┬────────────┐    ║
║  │  Lead ID   │  Name        │ Product  │ Status   │ Assigned   │    ║
║  ├────────────┼──────────────┼──────────┼──────────┼────────────┤    ║
║  │ LEAD-0021  │ James Hart   │ BTL      │ New      │ Sarah      │    ║
║  │ LEAD-0022  │ Maria Chen   │ Bridging │ Contacted│ Tom        │    ║
║  │ LEAD-0023  │ Paul Singh   │ Dev      │ Qualified│ Sarah      │    ║
║  │ LEAD-0024  │ Emma Scott   │ BTL      │ New      │ Unassigned │    ║
║  └────────────┴──────────────┴──────────┴──────────┴────────────┘    ║
║                                                                      ║
║  [Select lead to view details or convert to case]                    ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7.3 Case List Screen

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  SPECIALIST LENDING HUB          [Staff Member Name]  [Logout]  ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Dashboard]  [Leads]  [Cases]  [Commissions]  [Lenders]  [Admin]  ║
╠══════════════════════════════════════════════════════════════════════╣
║  CASES                                                               ║
║                                                                      ║
║  Filter: [Active ▼]  [All Products ▼]  [All Owners ▼]  [🔍 Search] ║
║                                                                      ║
║  ┌────────────────┬─────────────────┬──────────┬────────────────┬──────────┐ ║
║  │  Case ID       │  Applicant      │ Product  │ Current Stage  │ Owner   │ ║
║  ├────────────────┼─────────────────┼──────────┼────────────────┼──────────┤ ║
║  │ BTL-2026-0001  │ John & Jane Doe │ BTL      │ 8. Full App    │ Sarah   │ ║
║  │ BTL-2026-0002  │ Mark Williams   │ BTL      │ 10. Underwrite │ Tom     │ ║
║  │ BRIDGE-2026-001│ Lisa Turner     │ Bridging │ 9. Valuation   │ Sarah   │ ║
║  │ COMM-2026-0001 │ Apex Ltd        │ Commercial│ 5. Options    │ Tom     │ ║
║  │ DEV-2026-0001  │ Grove Dev Ltd   │ Dev      │ 8. Full App    │ Sarah   │ ║
║  └────────────────┴─────────────────┴──────────┴────────────────┴──────────┘ ║
║                                                                      ║
║  [Click any case to open Case Workspace]                             ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7.4 Case Workspace Overview Tab

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  BTL-2026-0001 — John & Jane Doe              [← Back to Cases] ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Overview] [Fact-Find] [Documents] [Messages] [Timeline] [Commission] ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  CASE HEADER                                                         ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  Case ID:    BTL-2026-0001   Product: Buy to Let              │   ║
║  │  Applicant:  John & Jane Doe Status:  Active                  │   ║
║  │  Loan Req:   £285,000        LTV:     72%                     │   ║
║  │  Property:   14 Maple Rd, Bristol BS5 0AB                     │   ║
║  │  Lender:     Foundation Home Loans    Case Owner: Sarah       │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  PROGRESS — STAGE 8 OF 14                                            ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  ●─●─●─●─●─●─●─●─○─○─○─○─○─○                               │   ║
║  │  1  2  3  4  5  6  7  8  9 10 11 12 13 14                   │   ║
║  │                      ↑                                        │   ║
║  │              Full Application                                 │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  CURRENT STAGE: Full Application                                     ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  Stage Notes:  Application pack sent to lender 08/04/2026    │   ║
║  │  Stage Date:   08 April 2026  (2 days)                        │   ║
║  │  SLA Due:      15 April 2026                                  │   ║
║  │                                                                │   ║
║  │  [Advance to Stage 9: Valuation ▶]  [Mark as On Hold]        │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  PRODUCT DETAILS — BTL                                               ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  Monthly Rent: £1,350    ICR Multiplier: 1.25                 │   ║
║  │  Max Loan (ICR): £280,800  EPC Rating: C                      │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

> **Dynamic progress bar:** The progress bar dots are calculated from `CurrentStageNumber / 14`. Completed stages show filled dots (●), the current stage shows a highlighted dot, and future stages show empty dots (○).

---

## 7.5 Case Workspace — Fact-Find Tab

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  BTL-2026-0001 — John & Jane Doe                                ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Overview] [Fact-Find] [Documents] [Messages] [Timeline] [Commission] ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  FACT-FIND                                  Status: In Progress      ║
║  Last updated by customer: 09 Apr 2026 14:32                         ║
║                                                                      ║
║  Section: [Personal ▼]                    [📋 View All] [📤 Export] ║
║                                                                      ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  APPLICANT 1                     APPLICANT 2                  │   ║
║  │  Name:  John Doe                 Name:  Jane Doe              │   ║
║  │  DOB:   12/03/1978               DOB:   04/09/1981            │   ║
║  │  NI:    AB123456C                NI:    CD789012E             │   ║
║  │  Status: Married                 Status: Married              │   ║
║  │  Address: 14 Maple Rd, Bristol                                │   ║
║  │  Time at address: 6 years                                     │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  INCOME                                                        │   ║
║  │  A1 Employment: Employed at Bristol City Council              │   ║
║  │  A1 Gross Salary: £52,000/yr                                  │   ║
║  │  A2 Employment: Self-Employed (Marketing Consultant)          │   ║
║  │  A2 Net Profit (Yr1): £38,500  (Yr2): £35,200                 │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  [Send Reminder to Customer]  [Copy Portal Link]  [Mark Reviewed]   ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7.6 Case Workspace — Documents Tab

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  BTL-2026-0001 — John & Jane Doe                                ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Overview] [Fact-Find] [Documents] [Messages] [Timeline] [Commission] ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  DOCUMENTS                    [+ Request Document]  [📧 Email Lender] ║
║                                                                      ║
║  ┌──────────────────┬──────────┬────────────┬──────────┬──────────┐ ║
║  │ Document         │ Category │ Requested  │ Status   │ Actions  │ ║
║  ├──────────────────┼──────────┼────────────┼──────────┼──────────┤ ║
║  │ Passport (A1)    │ Identity │ 01/04/2026 │ ✅ Recv'd │ [View]  │ ║
║  │ Passport (A2)    │ Identity │ 01/04/2026 │ ✅ Recv'd │ [View]  │ ║
║  │ 3 Months Payslip │ Income   │ 01/04/2026 │ ✅ Recv'd │ [View]  │ ║
║  │ SA302 2023-24    │ Income   │ 01/04/2026 │ ✅ Recv'd │ [View]  │ ║
║  │ Bank Stmts 6mo   │ Bank     │ 01/04/2026 │ ⏳ Pndng  │ [Chase] │ ║
║  │ EPC Certificate  │ Property │ 05/04/2026 │ ❌ Rjectd │ [Re-req]│ ║
║  │ Title Register   │ Property │ 05/04/2026 │ ⏳ Pndng  │ [Chase] │ ║
║  └──────────────────┴──────────┴────────────┴──────────┴──────────┘ ║
║                                                                      ║
║  DOCUMENT LIBRARY FOLDERS                                            ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  📁 01-Identity (2 files)   📁 02-Income (3 files)           │   ║
║  │  📁 04-Bank-Statements (0)  📁 03-Property (0 files)         │   ║
║  │  [Open Case Folder in SharePoint ↗]                          │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7.7 Case Workspace — Messages Tab

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  BTL-2026-0001 — John & Jane Doe                                ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Overview] [Fact-Find] [Documents] [Messages] [Timeline] [Commission] ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  MESSAGES                           [Internal Only]  [All Messages] ║
║                                                                      ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │                                                                │   ║
║  │  [Sarah (Advisor) → Customer]  08 Apr 2026, 10:14             │   ║
║  │  ┌──────────────────────────────────────────────────────┐     │   ║
║  │  │ Hi John & Jane, I've submitted your application to   │     │   ║
║  │  │ the lender. Please send over your bank statements    │     │   ║
║  │  │ as soon as possible. Thanks, Sarah                   │     │   ║
║  │  └──────────────────────────────────────────────────────┘     │   ║
║  │                                                                │   ║
║  │  [Customer → Advisor]  08 Apr 2026, 15:42    ✉ Unread         │   ║
║  │  ┌──────────────────────────────────────────────────────┐     │   ║
║  │  │ Thanks Sarah. I'll upload the bank statements        │     │   ║
║  │  │ tomorrow. Quick question — when will we get the      │     │   ║
║  │  │ valuation date?                                      │     │   ║
║  │  └──────────────────────────────────────────────────────┘     │   ║
║  │                                                                │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  New Message:  [Advisor → Customer ▼]                         │   ║
║  │  ┌──────────────────────────────────────────────────────┐     │   ║
║  │  │                                                      │     │   ║
║  │  └──────────────────────────────────────────────────────┘     │   ║
║  │  [Attach File]                              [Send Message]    │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7.8 Case Workspace — Timeline Tab

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  BTL-2026-0001 — John & Jane Doe                                ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Overview] [Fact-Find] [Documents] [Messages] [Timeline] [Commission] ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  CASE TIMELINE — Stage History                                       ║
║                                                                      ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  08 Apr 2026  Stage 8: Full Application                       │   ║
║  │  ● ENTERED by Sarah — "Application pack sent to Foundation"  │   ║
║  │  Duration in Stage 7 (DIP): 3 days                           │   ║
║  │                                                                │   ║
║  │  05 Apr 2026  Stage 7: Decision in Principle                  │   ║
║  │  ● ENTERED by Sarah — "DIP approved by Foundation HL"        │   ║
║  │  Duration in Stage 6 (Illustrations): 2 days                 │   ║
║  │                                                                │   ║
║  │  03 Apr 2026  Stage 6: Produce Illustrations                  │   ║
║  │  ● ENTERED by Sarah — "ESIS prepared for Foundation HL"      │   ║
║  │                                                                │   ║
║  │  01 Apr 2026  Stage 5: Present Options & Agree                │   ║
║  │  ● ENTERED by Tom — "Options presented to applicants"        │   ║
║  │                                                                │   ║
║  │  25 Mar 2026  Stage 4: Product Research                       │   ║
║  │  ● ENTERED by Sarah — "Researching BTL lender panel"         │   ║
║  │                                                                │   ║
║  │  20 Mar 2026  Stage 2: Fact-Find                              │   ║
║  │  ● Stage 3 (TOB & GDPR) SKIPPED — reason: "Fast-tracked"    │   ║
║  │                                                                │   ║
║  │  18 Mar 2026  Stage 1: Customer Enquiry                       │   ║
║  │  ● CASE CREATED from Lead LEAD-0015 — assigned to Sarah      │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7.9 Case Workspace — Commission Tab

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  BTL-2026-0001 — John & Jane Doe                                ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Overview] [Fact-Find] [Documents] [Messages] [Timeline] [Commission] ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  COMMISSION & FEES                         [+ Add Commission Record] ║
║                                                                      ║
║  ┌──────────────────┬──────────────┬──────────┬──────────┬────────┐ ║
║  │ Type             │ Expected     │ Actual   │ Received │ Date   │ ║
║  ├──────────────────┼──────────────┼──────────┼──────────┼────────┤ ║
║  │ Proc Fee         │ £2,137.50    │ —        │ ❌ No    │ —      │ ║
║  │ Advisory Fee     │ £1,500.00    │ £1,500   │ ✅ Yes   │ 01/04  │ ║
║  │ Referral Fee     │ £500.00      │ —        │ ❌ No    │ —      │ ║
║  └──────────────────┴──────────────┴──────────┴──────────┴────────┘ ║
║                                                                      ║
║  SUMMARY                                                             ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  Total Expected:   £4,137.50                                  │   ║
║  │  Total Received:   £1,500.00                                  │   ║
║  │  Outstanding:      £2,637.50                                  │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  Proc Fee = £285,000 × 0.75% = £2,137.50 (Foundation HL rate)       ║
║  Advisory Fee (AdvFee) = flat fee agreed with client at outset       ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7.10 Commissions Overview Screen

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  SPECIALIST LENDING HUB — COMMISSIONS                           ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Dashboard]  [Leads]  [Cases]  [Commissions]  [Lenders]  [Admin]  ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  Filter: [This Tax Year ▼]  [All Products ▼]  [All Types ▼]        ║
║                                                                      ║
║  SUMMARY CARDS                                                       ║
║  ┌────────────────┬────────────────┬────────────────┬─────────────┐ ║
║  │  Proc Fees     │  AdvFees       │  Referral Fees │  TOTAL      │ ║
║  │  £18,240       │  £12,500       │  £3,200        │  £33,940    │ ║
║  │  Expected      │  Received      │  Expected      │  Expected   │ ║
║  └────────────────┴────────────────┴────────────────┴─────────────┘ ║
║                                                                      ║
║  COMMISSION RECORDS                                                  ║
║  ┌─────────────────┬────────────┬──────────────┬──────────┬───────┐ ║
║  │ Case ID         │ Type       │ Amount       │ Received │ Notes │ ║
║  ├─────────────────┼────────────┼──────────────┼──────────┼───────┤ ║
║  │ BTL-2026-0001   │ Proc Fee   │ £2,137.50    │ ❌       │       │ ║
║  │ BTL-2026-0001   │ AdvFee     │ £1,500.00    │ ✅       │       │ ║
║  │ BTL-2026-0002   │ Proc Fee   │ £1,875.00    │ ✅       │       │ ║
║  │ BRIDGE-2026-001 │ Proc Fee   │ £3,500.00    │ ❌       │       │ ║
║  │ COMM-2026-0001  │ AdvFee     │ £2,500.00    │ ❌       │       │ ║
║  └─────────────────┴────────────┴──────────────┴──────────┴───────┘ ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 7.11 Lenders Screen

```
╔══════════════════════════════════════════════════════════════════════╗
║  🏦  SPECIALIST LENDING HUB — LENDERS                               ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Dashboard]  [Leads]  [Cases]  [Commissions]  [Lenders]  [Admin]  ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  LENDER PANEL                                         [+ Add Lender] ║
║                                                                      ║
║  Filter: [All Types ▼]  [All Products ▼]                [🔍 Search] ║
║                                                                      ║
║  ┌───────────────────────┬────────────┬──────────┬───────┬────────┐ ║
║  │  Lender               │  Type      │ Products │ MaxLTV│ Proc % │ ║
║  ├───────────────────────┼────────────┼──────────┼───────┼────────┤ ║
║  │  Foundation Home Loans│  Specialist│ BTL      │ 75%   │ 0.75%  │ ║
║  │  Together Money       │  Specialist│ Bridge   │ 70%   │ 1.00%  │ ║
║  │  Shawbrook Bank       │  Challenger│ BTL,Comm │ 75%   │ 0.50%  │ ║
║  │  Precise Mortgages    │  Specialist│ BTL,Refurb│ 75%  │ 0.60%  │ ║
║  │  Allica Bank          │  Challenger│ Commercial│ 70%  │ 0.50%  │ ║
║  │  Masthaven Bank       │  Specialist│ Bridge,Dev│ 65%  │ 1.00%  │ ║
║  └───────────────────────┴────────────┴──────────┴───────┴────────┘ ║
║                                                                      ║
║  [Click lender to view full details and BDM contact]                 ║
╚══════════════════════════════════════════════════════════════════════╝
```
