[← Back to Index](00-index.md)

# 11. KPI Dashboards

The platform includes 4 built-in dashboard views accessible from the Staff App. These are built using Power Apps galleries and filters — no Power BI license is required for the initial implementation.

---

## 11.1 Pipeline Dashboard

**Purpose:** Real-time view of all active cases by stage and product type.

```
╔══════════════════════════════════════════════════════════════════════╗
║  📊  PIPELINE DASHBOARD                    [Export]  [Refresh]      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  Filter: [Active ▼]  [All Products ▼]  [All Owners ▼]  [This Month]║
║                                                                      ║
║  PIPELINE BY STAGE                                                   ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  1. Enquiry          █  1 case    £220,000                   │   ║
║  │  2. Fact-Find        ████  4 cases  £1,050,000               │   ║
║  │  3. TOB & GDPR       ██  2 cases   £480,000                  │   ║
║  │  4. Product Res.     █  1 case    £350,000                   │   ║
║  │  5. Options          ██  2 cases   £610,000                  │   ║
║  │  6. Illustrations    █  1 case    £195,000                   │   ║
║  │  7. DIP              ██  2 cases   £520,000                  │   ║
║  │  8. Full Application ███  3 cases  £850,000                  │   ║
║  │  9. Valuation        ██  2 cases   £630,000                  │   ║
║  │  10. Underwriting    ██  2 cases   £510,000                  │   ║
║  │  11. Loan Offer      █  1 case    £280,000                   │   ║
║  │  12. Legals          █  1 case    £325,000                   │   ║
║  │  13. Completion      ░  0 cases   £0                         │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  PIPELINE BY PRODUCT                   PIPELINE VALUE               ║
║  ┌────────────────────────┐            ┌─────────────────────────┐  ║
║  │  BTL          6 cases  │            │  Total Pipeline         │  ║
║  │  Bridging     4 cases  │            │  £6,020,000             │  ║
║  │  Commercial   2 cases  │            │                         │  ║
║  │  Dev Finance  1 case   │            │  Avg Case Size          │  ║
║  │  Refurb       1 case   │            │  £293,000               │  ║
║  │  Auction      0 cases  │            │                         │  ║
║  └────────────────────────┘            └─────────────────────────┘  ║
╚══════════════════════════════════════════════════════════════════════╝
```

**Data source:** Cases list — filtered by CaseStatus = "Active"  
**Key metrics:** Count by CurrentStageName, Sum of LoanAmountRequired, groupBy ProductType

---

## 11.2 Performance Dashboard

**Purpose:** Track team and individual performance against targets.

```
╔══════════════════════════════════════════════════════════════════════╗
║  📈  PERFORMANCE DASHBOARD                                           ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  Period: [This Month ▼]   [This Quarter ▼]   [This Tax Year ▼]     ║
║                                                                      ║
║  TEAM SUMMARY                                                        ║
║  ┌──────────────┬──────────┬──────────┬──────────┬────────────────┐ ║
║  │              │  Leads   │  Cases   │ Completes│  Fee Income    │ ║
║  │              │  Created │  Active  │  Month   │  (Month)       │ ║
║  ├──────────────┼──────────┼──────────┼──────────┼────────────────┤ ║
║  │  Sarah       │    8     │    9     │    2     │   £7,450       │ ║
║  │  Tom         │    5     │    7     │    1     │   £3,200       │ ║
║  │  TOTAL TEAM  │   13     │   16     │    3     │  £10,650       │ ║
║  └──────────────┴──────────┴──────────┴──────────┴────────────────┘ ║
║                                                                      ║
║  CONVERSION RATES                                                    ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  Lead → Case:        68%  (13 cases from 19 leads)           │   ║
║  │  Case → Completion:  75%  (6 completions from 8 reached DIP) │   ║
║  │  Avg Days to Complete: 47 days                               │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  SLA PERFORMANCE                                                     ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  Cases on track:     12  (75%)                               │   ║
║  │  Cases at risk:       3  (19%)  — within 2 days of SLA       │   ║
║  │  Cases overdue:       1   (6%)  — action required            │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

**Data sources:** Cases list, Leads list, Case Stage History list  
**Key metrics:** Count by AssignedTo, conversion rates, average stage durations from Stage History

---

## 11.3 Commission Dashboard

**Purpose:** Track advisory fees, proc fees, and referral fees by case, product, and period.

```
╔══════════════════════════════════════════════════════════════════════╗
║  💰  COMMISSION DASHBOARD                                            ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  Period: [2025/26 Tax Year ▼]    [All Products ▼]                   ║
║                                                                      ║
║  INCOME SUMMARY                                                      ║
║  ┌─────────────────────┬──────────────┬──────────────┬────────────┐ ║
║  │  Fee Type           │  Expected    │  Received    │  %         │ ║
║  ├─────────────────────┼──────────────┼──────────────┼────────────┤ ║
║  │  Proc Fees          │  £28,450     │  £18,240     │  64%       │ ║
║  │  Advisory Fees      │  £22,000     │  £14,500     │  66%       │ ║
║  │  Referral Fees      │   £4,800     │   £3,200     │  67%       │ ║
║  ├─────────────────────┼──────────────┼──────────────┼────────────┤ ║
║  │  TOTAL              │  £55,250     │  £35,940     │  65%       │ ║
║  └─────────────────────┴──────────────┴──────────────┴────────────┘ ║
║                                                                      ║
║  INCOME BY PRODUCT                    OUTSTANDING COMMISSIONS        ║
║  ┌────────────────────────────────┐   ┌────────────────────────────┐║
║  │  BTL         ██████  £18,200  │   │  BTL-2026-0001 Proc  £2,137│║
║  │  Bridging    ████    £12,400  │   │  BRIDGE-2026-001 Proc £3,500│║
║  │  Commercial  ███      £9,100  │   │  COMM-2026-001 Adv  £2,500 │║
║  │  Dev Finance ██       £5,600  │   │  TOTAL OUTSTANDING: £19,310│║
║  │  Refurb      █        £3,940  │   │                            │║
║  └────────────────────────────────┘   └────────────────────────────┘║
║                                                                      ║
║  ADVISORY FEES (AdvFee) — MONTH BY MONTH                            ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  Apr  ███  £3,200   Jul  ██  £1,500   Oct  ████  £4,100      │   ║
║  │  May  ██   £1,800   Aug  ███ £2,800   Nov  ██    £2,100      │   ║
║  │  Jun  ████ £3,900   Sep  ██  £2,400   Dec  ░     £0 (YTD)    │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

**Data source:** Commissions list  
**Key metrics:** Sum by CommissionType, IsReceived, TaxYear, ProductType

---

## 11.4 Compliance Dashboard

**Purpose:** Track GDPR consent, Terms of Business completion, and outstanding document requests.

```
╔══════════════════════════════════════════════════════════════════════╗
║  🔒  COMPLIANCE DASHBOARD                                            ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  TERMS OF BUSINESS STATUS                                            ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  Signed & Filed:   13 cases  ✅                              │   ║
║  │  Issued / Pending:  2 cases  ⚠                               │   ║
║  │  Not Yet Issued:    1 case   🔴                               │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  FACT-FIND COMPLETION                                                ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  Complete & Reviewed:   9 cases  ✅                           │   ║
║  │  In Progress:           5 cases  ⏳                           │   ║
║  │  Not Started:           2 cases  🔴  Action required          │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  OUTSTANDING DOCUMENT REQUESTS (Overdue)                            ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  BTL-2026-0003 — Bank Statements — 8 days overdue             │   ║
║  │  BRIDGE-2026-001 — Company Accounts — 5 days overdue          │   ║
║  │  COMM-2026-0001 — Lease Agreement — 3 days overdue            │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
║                                                                      ║
║  CASES WITH ADVERSE CREDIT FLAGGED                                  ║
║  ┌──────────────────────────────────────────────────────────────┐   ║
║  │  2 cases flagged (AdverseCredit = Yes in Fact-Find)           │   ║
║  │  Both have Case Owner notes recorded ✅                       │   ║
║  └──────────────────────────────────────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

**Data sources:** Cases list (Stage 3 completion), Fact-Find Responses list (Status field), Document Requests list (Status + DueDate), Fact-Find Responses (AdverseCredit)

---

## 11.5 Dashboard Technical Notes

| Dashboard | Primary Data Source | Key Columns Used |
|---|---|---|
| Pipeline | Cases list | CurrentStageName, ProductType, LoanAmountRequired, CaseStatus |
| Performance | Cases + Leads + Stage History | AssignedTo, Created, ActualCompletionDate, TimeInPreviousStageHours |
| Commission | Commissions list | CommissionType, ExpectedAmount, ActualAmount, IsReceived, TaxYear |
| Compliance | Cases + Fact-Find + Doc Requests | (Stage 3 docs present), Status, DueDate, AdverseCredit |

> **Power BI option:** All 4 dashboards can be enhanced with Power BI reports connected to the same SharePoint lists for richer visualisations. Power BI Pro is required per user for shared reports — this is a future enhancement. See section 14.
