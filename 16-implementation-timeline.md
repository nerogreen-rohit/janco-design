[← Back to Index](00-index.md)

# 16. Implementation Timeline

The platform will be built and deployed in **7 weeks** across 6 phases.

---

## 16.1 Timeline Overview

```
WEEK  1    2    3    4    5    6    7
      ├────┤────┤────┤────┤────┤────┤
Ph 1  ████
Ph 2       ████████
Ph 3             ████████
Ph 4                   ████
Ph 5                        ████
Ph 6                             ████
```

---

## 16.2 Phase Detail

### Phase 1 — SharePoint Setup (Week 1)

| Task | Owner | Notes |
|---|---|---|
| Create SharePoint site "Specialist Lending Hub" | System Owner / IT Admin | |
| Create all 13 SharePoint lists with correct columns | Developer | Per schemas in Section 5 |
| Create Case Documents library with no default folders | Developer | Folders created per-case by flow |
| Configure Stage Configuration list — enter all 14 stages × 6 products | System Owner | 84 rows |
| Configure Document Types list — enter all document categories | System Owner | |
| Configure Lenders list — enter initial lender panel | System Owner | |
| Configure Referrers list — enter known referrers | System Owner | |
| Configure Counters list — set up all product counters | Developer | |
| Set SharePoint external sharing settings | IT Admin | Enable guest sharing at site level |

---

### Phase 2 — Power Automate Flows (Weeks 2–3)

| Task | Order | Notes |
|---|---|---|
| Flow 3: Generate Case ID | 1st | Prerequisite for testing case creation |
| Flow 1: Generate Lead ID | 1st | Parallel to Flow 3 |
| Flow 2: Convert Lead to Case | 2nd | Depends on Flow 3 |
| Flow 4: Create Fact-Find Item | 3rd | Triggered by case creation |
| Flow 5: Create Case Folder Structure | 3rd | Triggered by case creation |
| Flow 6: Log Stage Change | 4th | |
| Flow 7: Send Document Request Email | 5th | |
| Flow 8: Document Received Notification | 5th | |
| Flow 9: SLA Reminder (Scheduled) | 6th | Set schedule after flows 3–8 tested |
| Flow 10: Calculate Advisory Fee | 6th | |
| Flow 11: Calculate Proc Fee | 6th | |
| Flow 12: Email Docs to Lender | 6th | |
| Flow 13: Customer Status Update | 7th | Depends on Flow 6 |
| Flow 14: Message Notification | 7th | |
| Flow 15: Fact-Find Updated Notification | 7th | |
| Flow 16: Case Completion Wrap-Up | 8th | |

---

### Phase 3 — Power Apps Staff App (Weeks 3–4)

| Screen | Notes |
|---|---|
| Dashboard screen | Pipeline summary, task list, SLA alerts |
| Leads screen | List, filter, new lead form, convert to case button |
| Case list screen | Filter, search, click to workspace |
| Case Workspace — Overview tab | Header, progress bar, current stage, advance stage |
| Case Workspace — Fact-Find tab | Read-only view of Fact-Find item sections |
| Case Workspace — Documents tab | Doc request list, upload status, email lender button |
| Case Workspace — Messages tab | Message thread, send message, direction selector |
| Case Workspace — Timeline tab | Stage history log |
| Case Workspace — Commission tab | Commission records for this case |
| Commissions overview screen | All commissions, filter, summary cards |
| Lenders screen | Lender panel list, view/edit |
| Admin screen | Stage config, document types, counters |

---

### Phase 4 — Customer Portal (Week 5)

| Task | Notes |
|---|---|
| Create SharePoint page for customer progress view | Embedded list view — filtered by CaseID |
| Configure guest sharing links for Fact-Find items | Test with 1 dummy customer |
| Configure guest sharing links for document upload folders | Folders 01–04 per case |
| Test customer fact-find experience end to end | Complete fact-find as guest user |
| Test document upload as guest user | Upload test file to each folder |
| Test message exchange (Advisor → Customer and return) | |
| Confirm customer cannot see internal data | Check permissions thoroughly |

---

### Phase 5 — Testing & UAT (Week 6)

| Test | Description |
|---|---|
| End-to-end case test (BTL) | Lead → Case → Fact-Find → Docs → Full App → Completion |
| End-to-end case test (Bridging) | Skipping optional stages |
| Portfolio client test | Create 2 cases for same applicant |
| Commission calculation test | Proc fee + AdvFee + referral fee |
| SLA reminder test | Set overdue dates, confirm reminder fires |
| Email to lender test | Select docs, send, confirm email received |
| Customer portal test | Guest access, fact-find, upload, messages |
| Dashboard accuracy test | Verify all KPI figures match list data |
| Permissions test | Confirm customers cannot access other data |
| Stage skip test | Skip optional stage, verify audit trail |

---

### Phase 6 — Go Live & Handover (Week 7)

| Task | Notes |
|---|---|
| System owner sign-off | Review all screens and flows |
| Staff training (2–4 users) | Walkthrough of Staff App and key flows |
| Import real data (if applicable) | Migrate open cases from spreadsheets |
| Configure production sharing links | Replace test links with live case links |
| Documentation handover | This document + quick-reference guides |
| Set up SLA reminder schedule | Confirm daily 08:00 flow is running |
| Go live | ✅ Platform operational |
| Post-launch support window | 2 weeks of priority support |

---

## 16.3 Dependencies & Risks

| Dependency | Risk | Mitigation |
|---|---|---|
| IT admin enabling SharePoint guest sharing | Medium — requires tenant admin access | Raise early in Week 1 |
| Stage Configuration data entry (84 rows) | Low — manual but not complex | System owner completes in Week 1 alongside dev |
| Customer email deliverability | Medium — automated emails may hit spam | Test email sender reputation; use tenant email address |
| Guest sharing link lifespan | Low — SharePoint guest links expire if not configured | Set link expiry to "Never" or long duration for active cases |
| Power Automate Premium (for e-sign, EPC) | Low — not required for v1.0 | v1.0 uses standard connectors only |
