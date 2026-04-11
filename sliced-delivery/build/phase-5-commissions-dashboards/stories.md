# Phase 5 — Commissions & Dashboards: User Stories & Task Index

**Goal:** Commission tracking (proc fees, advisory fees, referral fees) and all 4 KPI dashboards.  
**Duration:** 1 week (Week 7)  
**Depends on:** Phase 1 (Cases list), Phase 2 (Referrers list), Phase 3 (Stage History, Lenders list)  
**Business value:** Real-time commission visibility replaces monthly manual spreadsheet reconciliation. Four dashboards give management live KPIs — pipeline, performance, commissions, and compliance.

---

## Feature 1: SharePoint Lists

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P5-01 | As a **developer**, I need to create the Commissions SharePoint list so that commission records can be stored per case | [task-01-create-commissions-list.md](task-01-create-commissions-list.md) | 🔴 Critical |
| P5-02 | As a **developer**, I need to activate the commission summary columns on the Cases list (created in Phase 1, now populated) so that case-level commission totals are visible | [task-02-activate-case-commission-columns.md](task-02-activate-case-commission-columns.md) | 🔴 Critical |

---

## Feature 2: Power Automate Flows

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P5-03 | As a **system**, when a Commission record of type "Advisory Fee (AdvFee)" is created or modified I need to validate the amount and update the case total so that advisory fees are tracked accurately | [task-03-flow-calculate-advfee.md](task-03-flow-calculate-advfee.md) | 🔴 Critical |
| P5-04 | As a **system**, when a Lender is assigned to a case I need to auto-calculate the expected proc fee from `LoanAmountRequired × TypicalProcFeePercent` so that proc fee estimates are always current | [task-04-flow-calculate-proc-fee.md](task-04-flow-calculate-proc-fee.md) | 🔴 Critical |
| P5-05 | As a **system**, when a case status changes to "Completed" I need to send a completion email, close outstanding document requests, and remind the case owner to review commissions | [task-05-flow-case-completion.md](task-05-flow-case-completion.md) | 🔴 Critical |

---

## Feature 3: Staff App Screens

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P5-06 | As a **staff member**, I want a Commission tab on the Case Workspace showing all commission records for the current case so that I can view and add commissions in context | [task-06-staff-app-commission-tab.md](task-06-staff-app-commission-tab.md) | 🔴 Critical |
| P5-07 | As a **staff member**, I want a Commissions Overview screen showing all commission records across all cases so that I can filter, search, and export commission data | [task-07-staff-app-commissions-overview.md](task-07-staff-app-commissions-overview.md) | 🔴 Critical |
| P5-08 | As a **manager**, I want a Pipeline Dashboard showing live case counts by stage and product type so that I can see the current pipeline at a glance | [task-08-dashboard-pipeline.md](task-08-dashboard-pipeline.md) | 🟡 High |
| P5-09 | As a **manager**, I want a Performance Dashboard showing conversion rates and SLA performance so that I can track team efficiency | [task-09-dashboard-performance.md](task-09-dashboard-performance.md) | 🟡 High |
| P5-10 | As a **manager**, I want a Commission Dashboard showing income totals by type, product, and tax year so that I can monitor revenue and outstanding commissions | [task-10-dashboard-commission.md](task-10-dashboard-commission.md) | 🟡 High |
| P5-11 | As a **compliance officer**, I want a Compliance Dashboard showing TOB status, fact-find completion, and overdue documents so that I can identify compliance gaps | [task-11-dashboard-compliance.md](task-11-dashboard-compliance.md) | 🟡 High |
| P5-12 | As a **staff member**, I want a combined Dashboard screen with tabs for Pipeline, Performance, Commission, and Compliance so that all KPIs are in one place | [task-12-dashboard-screen-layout.md](task-12-dashboard-screen-layout.md) | 🟡 High |

---

## Acceptance Criteria (Phase 5 Complete)

- [ ] Staff can add Commission records for Proc Fee, Advisory Fee (AdvFee), and Referral Fee
- [ ] Advisory fees are labelled "Advisory Fee (AdvFee)" in all UI labels and flow records
- [ ] Flow 11 auto-calculates expected proc fee when a Lender is assigned to a case
- [ ] `TotalCommissionExpected` on the Case record updates automatically
- [ ] Staff can mark a commission as received (IsReceived = Yes, ReceivedDate)
- [ ] Flow 16 fires on case completion: customer email, Document Requests closed, commission reminder sent
- [ ] Commission Dashboard shows accurate totals by type, product, and tax year
- [ ] Pipeline Dashboard shows live case counts by stage and by product type
- [ ] Performance Dashboard shows lead→case and case→completion conversion rates
- [ ] Compliance Dashboard shows fact-find completion status and overdue document requests
- [ ] All 4 dashboard metrics match the underlying list data
