# Phase 3 — Case Management & Lifecycle: User Stories & Task Index

**Goal:** Full 14-stage lifecycle, document requests, messaging, SLA alerts, and email-to-lender.  
**Duration:** 2 weeks (Weeks 4–5)  
**Depends on:** Phase 1 (Cases list, Fact-Find, Counters, document library) and Phase 2 (Leads list, conversion flow)  
**Business value:** Staff have full case management — track every case from enquiry to completion with audit trail, SLA monitoring, document tracking, and direct lender email.

---

## Feature 1: SharePoint Lists

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P3-01 | As a **developer**, I need to create the Case Stage History list so that every stage change is recorded as an immutable audit log | [task-01-create-stage-history-list.md](task-01-create-stage-history-list.md) | 🔴 Critical |
| P3-02 | As a **developer**, I need to create the Document Types configuration list so that document categories and requirements are centrally managed | [task-02-create-document-types-list.md](task-02-create-document-types-list.md) | 🔴 Critical |
| P3-03 | As a **developer**, I need to create the Document Requests list so that staff can track requested and received documents per case | [task-03-create-document-requests-list.md](task-03-create-document-requests-list.md) | 🔴 Critical |
| P3-04 | As a **developer**, I need to create the Tasks list so that staff can assign and track internal tasks linked to cases | [task-04-create-tasks-list.md](task-04-create-tasks-list.md) | 🟡 High |
| P3-05 | As a **developer**, I need to create the Messages list so that case-level communication between staff and customers is stored | [task-05-create-messages-list.md](task-05-create-messages-list.md) | 🔴 Critical |
| P3-06 | As a **developer**, I need to create the Lenders list so that lender contact details and submission emails are centrally managed | [task-06-create-lenders-list.md](task-06-create-lenders-list.md) | 🟡 High |
| P3-07 | As a **developer**, I need to add stages 4–14 to the Stage Configuration list so that the full 14-stage lifecycle is available | [task-07-add-remaining-stages.md](task-07-add-remaining-stages.md) | 🔴 Critical |

---

## Feature 2: Power Automate Flows

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P3-08 | As a **system**, when a case stage changes I need to log it to Stage History with text snapshots so that an immutable audit trail exists | [task-08-flow-log-stage-change.md](task-08-flow-log-stage-change.md) | 🔴 Critical |
| P3-09 | As a **system**, when a document request is created I need to email the customer with the document name and upload link so that they know what to submit | [task-09-flow-document-request-email.md](task-09-flow-document-request-email.md) | 🔴 Critical |
| P3-10 | As a **system**, when a customer uploads a file I need to update the Document Request status and notify the case owner so that document receipt is tracked | [task-10-flow-document-received.md](task-10-flow-document-received.md) | 🔴 Critical |
| P3-11 | As a **system**, I need to check all active cases daily for SLA breaches and send Teams alerts so that overdue stages are flagged | [task-11-flow-sla-reminder.md](task-11-flow-sla-reminder.md) | 🟡 High |
| P3-12 | As a **staff member**, I want to email selected case documents to the lender so that I can submit applications directly from the app | [task-12-flow-email-lender.md](task-12-flow-email-lender.md) | 🟡 High |
| P3-13 | As a **system**, when a case moves to a customer-visible stage I need to send the customer a status update email so that they are kept informed | [task-13-flow-customer-status-update.md](task-13-flow-customer-status-update.md) | 🔴 Critical |
| P3-14 | As a **system**, when a staff member sends a message to a customer I need to email the customer a notification so that they know to check the portal | [task-14-flow-message-notification.md](task-14-flow-message-notification.md) | 🟡 High |

---

## Feature 3: Staff App Screens

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P3-15 | As a **staff member**, I want a filterable case list screen so that I can quickly find and open any case | [task-15-staff-app-case-list.md](task-15-staff-app-case-list.md) | 🔴 Critical |
| P3-16 | As a **staff member**, I want a Case Workspace Overview tab with a 14-stage progress bar so that I can see case status and advance stages | [task-16-staff-app-overview-tab.md](task-16-staff-app-overview-tab.md) | 🔴 Critical |
| P3-17 | As a **staff member**, I want a Documents tab showing document requests and statuses so that I can track and manage case documents | [task-17-staff-app-documents-tab.md](task-17-staff-app-documents-tab.md) | 🔴 Critical |
| P3-18 | As a **staff member**, I want a Messages tab so that I can send and view case communications with the customer | [task-18-staff-app-messages-tab.md](task-18-staff-app-messages-tab.md) | 🟡 High |
| P3-19 | As a **staff member**, I want a Timeline tab showing the complete stage history so that I can review the full audit trail for a case | [task-19-staff-app-timeline-tab.md](task-19-staff-app-timeline-tab.md) | 🟡 High |
| P3-20 | As a **staff member**, I want admin screens for Stage Configuration and Document Types so that I can manage reference data without editing lists directly | [task-20-staff-app-admin-screens.md](task-20-staff-app-admin-screens.md) | 🟢 Medium |

---

## Acceptance Criteria (Phase 3 Complete)

- [ ] All 14 stages are present in Stage Configuration list
- [ ] Staff can advance a BTL case through all 14 stages
- [ ] Each stage change creates an immutable Stage History record
- [ ] Stage History cannot be edited by staff from the Staff App
- [ ] Customer receives automated email when case moves to a VisibleToCustomer stage
- [ ] Staff can create a document request; customer receives email with upload link
- [ ] When customer uploads a file, Document Request status updates to "Received" and case owner is notified
- [ ] Staff can send a message to the customer via the Messages tab
- [ ] Customer receives email notification of new message
- [ ] Daily SLA flow sends Teams alert for any case where the SLA deadline has passed
- [ ] Staff can select documents and email them to the lender from the Documents tab
- [ ] Optional stages (5, 6, 7 for Bridge/Auction) can be skipped with a reason recorded in Stage History
- [ ] Timeline tab shows complete chronological stage history for each case
