# Phase 5 — Commissions & Dashboards

**Goal:** Commission tracking (proc fees, advisory fees, referral fees) and all 4 KPI dashboards.  
**Estimated duration:** 1 week (Week 7)  
**Depends on:** Phase 1 (Cases list), Phase 2 (Referrers list), Phase 3 (Stage History, Leads list)  
**Business value:** Real-time commission visibility replaces monthly manual spreadsheet reconciliation. Four dashboards give management live KPIs — pipeline, performance, commissions, and compliance.

---

## What This Phase Delivers

At the end of Phase 5, the following is operational:

1. A **Commissions list** tracks all proc fees, advisory fees (AdvFee), and referral fees per case.
2. **Flow 10** automatically validates and records Advisory Fees (AdvFee); updates `TotalCommissionExpected` on the Case record.
3. **Flow 11** automatically calculates expected proc fees from `LoanAmountRequired × LenderProcFeePercent`; creates Commission records.
4. **Flow 16** fires on case completion: sends completion email to customer, closes outstanding document requests, triggers commission review reminder to case owner.
5. The **Commissions Overview screen** in the Staff App shows all commission records filterable by type, product, tax year, and received status.
6. The **Commission tab** on the Case Workspace shows commission records for that specific case.
7. All 4 **KPI dashboards** are live in the Staff App Dashboard screen:
   - Pipeline Dashboard (cases by stage and product)
   - Performance Dashboard (team conversion rates, SLA performance)
   - Commission Dashboard (income summary by type, product, and month)
   - Compliance Dashboard (TOB status, fact-find completion, overdue document requests)

---

## SharePoint Lists Required

### 5.10 Commissions List (new in Phase 5)

| Column | Type | Notes |
|---|---|---|
| Title | Single line | Set to `{CaseID}-{CommissionType}` by flow |
| CaseID | Single line | FK to Cases list |
| CaseName | Single line | Denormalized |
| LenderName | Single line | Denormalized |
| ProductType | Single line | Denormalized |
| CommissionType | Choice | Proc Fee, **Advisory Fee (AdvFee)**, Introducer Fee, Referral Fee |
| ExpectedAmount | Currency | |
| ActualAmount | Currency | |
| IsReceived | Yes/No | |
| ReceivedDate | Date | |
| InvoiceNumber | Single line | |
| PaymentMethod | Choice | BACS, Cheque, Deducted at Completion, Other |
| SplitPercentage | Number | If shared — percentage to this party |
| ReferrerID | Lookup → Referrers | If referral fee |
| Notes | Multiple lines | |
| TaxYear | Single line | e.g. "2025/26" — for filtering in Commission Dashboard |

> **Terminology note:** Advisory fees are **always** labelled "Advisory Fee (AdvFee)" in the `CommissionType` choice value, in flow variable names, and in all UI labels. Never use alternative terminology.  
> **Source:** `05-sharepoint-list-schemas.md` lines 438–461; `17-key-design-decisions.md` §17.8

---

### Cases List — Commission summary columns (update existing list)

These columns are defined in the Cases list schema (Phase 1) but are populated in Phase 5:

| Column | Type | Notes |
|---|---|---|
| ProcFeeExpected | Currency | Updated by Flow 11 |
| AdvFeeCharged | Currency | Set by staff; validated by Flow 10 |
| TotalCommissionExpected | Currency | Calculated total — updated by flows 10 and 11 |
| CommissionReceived | Yes/No | |
| CommissionReceivedDate | Date | |

> **Source:** `05-sharepoint-list-schemas.md` lines 91–95

---

## Power Automate Flows Required

### Flow 10 — Calculate Advisory Fee

| Attribute | Value |
|---|---|
| Trigger | When Commission item created or modified |
| Action | Validates Advisory Fee (AdvFee) amount; updates `TotalCommissionExpected` on Case record; flags if AdvFee not yet received |
| Notes | Advisory Fee is labelled "Advisory Fee (AdvFee)" in all flow variables and Commission Type values |

> **Source:** `10-automation-summary.md` line 23

---

### Flow 11 — Calculate Proc Fee

| Attribute | Value |
|---|---|
| Trigger | When Commission item created or modified |
| Action | Calculates expected proc fee from `LoanAmountRequired × LenderProcFeePercent` (from Lenders list); creates Commission record if one does not already exist for this case |

> **Source:** `10-automation-summary.md` line 24

---

### Flow 16 — Case Completion Wrap-Up

| Attribute | Value |
|---|---|
| Trigger | When `CaseStatus` set to "Completed" on Case item |
| Action | 1. Sends completion email to customer; 2. Marks all outstanding Document Requests as "Not Required"; 3. Sends commission review reminder to case owner |

> **Source:** `10-automation-summary.md` line 29

---

## Staff App Screens Required

### Case Workspace — Commission Tab

Shows all Commission records linked to the current case.

```
╔══════════════════════════════════════════════════════════════════════╗
║  BTL-2026-0001 — John & Jane Doe                                     ║
╠══════════════════════════════════════════════════════════════════════╣
║  [Overview] [Fact-Find] [Documents] [Messages] [Timeline] [Commission]║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                       ║
║  COMMISSIONS FOR BTL-2026-0001         [+ Add Commission Record]      ║
║                                                                       ║
║  ┌──────────────────┬──────────────┬──────────────┬──────────────┐   ║
║  │  Type            │  Expected    │  Received    │  Status      │   ║
║  ├──────────────────┼──────────────┼──────────────┼──────────────┤   ║
║  │  Proc Fee        │  £2,137      │  £2,137      │  ✅ Received  │   ║
║  │  Advisory Fee    │  £1,500      │  —           │  ⏳ Pending   │   ║
║  │  (AdvFee)        │              │              │              │   ║
║  └──────────────────┴──────────────┴──────────────┴──────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

### Commissions Overview Screen

All commission records across all cases. Filterable by commission type, product, tax year, received status, and case owner.

---

### Dashboard Screen — All 4 Dashboards

#### Pipeline Dashboard

**Data source:** Cases list — filtered by `CaseStatus = "Active"`  
**Key metrics:** Count by `CurrentStageName`, sum of `LoanAmountRequired`, groupBy `ProductType`  
Shows: Pipeline by stage (bar chart), pipeline by product (count), total pipeline value, average case size.

> **Source:** `11-kpi-dashboards.md` §11.1

---

#### Performance Dashboard

**Data sources:** Cases list, Leads list, Case Stage History list  
**Key metrics:** Count by `AssignedTo`, conversion rates, average stage durations from `TimeInPreviousStageHours`  
Shows: Team summary (leads, active cases, completions, fee income by staff member), lead→case and case→completion conversion rates, SLA performance (on track / at risk / overdue counts).

> **Source:** `11-kpi-dashboards.md` §11.2

---

#### Commission Dashboard

**Data source:** Commissions list  
**Key metrics:** Sum by `CommissionType`, `IsReceived`, `TaxYear`, `ProductType`  
Shows: Income summary table (Proc Fees, Advisory Fees/AdvFee, Referral Fees — expected vs received vs %), income by product (bar), outstanding commissions list, AdvFee month-by-month chart.

> **Source:** `11-kpi-dashboards.md` §11.3

---

#### Compliance Dashboard

**Data sources:** Cases list (Stage 3 docs present), Fact-Find Responses list (Status field), Document Requests list (Status + DueDate), Fact-Find Responses (AdverseCredit)  
Shows: TOB status (signed/pending/not issued), Fact-Find completion (complete/in-progress/not started), overdue document requests list, cases with adverse credit flagged.

> **Source:** `11-kpi-dashboards.md` §11.4

---

## Advisory Fee (AdvFee) — Before vs After

| Aspect | Before | After (Phase 5) |
|---|---|---|
| How tracked | Separate Excel row | Commission record per case (`CommissionType = "Advisory Fee (AdvFee)"`) |
| Visibility | System owner only | All staff can see commission records for their cases |
| Receipt confirmation | Manual tick on spreadsheet | `IsReceived` + `ReceivedDate` fields; Commission Dashboard |
| Invoice reference | Noted in email | `InvoiceNumber` field on Commission record |
| Tax year reporting | Manual annual summary | `TaxYear` field — filtered in Commission Dashboard |

> **Source:** `15-before-vs-after.md` §15.3

---

## Phase 5 Acceptance Criteria

| # | Criterion |
|---|---|
| 1 | Staff can add Commission records for Proc Fee, Advisory Fee (AdvFee), and Referral Fee |
| 2 | Advisory fees are labelled "Advisory Fee (AdvFee)" in all UI labels and flow records |
| 3 | Flow 11 auto-calculates expected proc fee when a Lender is assigned to a case |
| 4 | `TotalCommissionExpected` on the Case record updates automatically |
| 5 | Staff can mark a commission as received (IsReceived = Yes, ReceivedDate) |
| 6 | Flow 16 fires on case completion: customer email, Document Requests closed, commission reminder sent |
| 7 | Commission Dashboard shows accurate totals by type, product, and tax year |
| 8 | Pipeline Dashboard shows live case counts by stage and by product type |
| 9 | Performance Dashboard shows lead→case and case→completion conversion rates |
| 10 | Compliance Dashboard shows fact-find completion status and overdue document requests |
| 11 | All 4 dashboard metrics match the underlying list data |

---

## What Phase 5 Does NOT Include

| Capability | Delivered in Version |
|---|---|
| Power BI dashboards | Future v2.0 |
| Referrer self-service portal | Future v2.1 |
| Automated DIP submission via API | Future v2.2 |
| AI-assisted case summarisation | Future v3.0 |
