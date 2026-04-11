# Phase 4 — Customer Portal: User Stories & Task Index

**Goal:** Full customer-facing portal with application progress tracker, document upload, messaging, and fact-find access — all via SharePoint.  
**Duration:** 1 week (Week 6)  
**Depends on:** Phase 1 (Fact-Find, guest sharing), Phase 3 (Messages list, stage tracking, Document Requests)  
**Business value:** Customer self-service — customers can track their application, upload documents, communicate with their advisor, and complete their fact-find without calling the office.

> **Key constraint:** No Power Apps license for customers. Built entirely on SharePoint Online (M365 Business Standard). Customers see `CustomerFriendlyName` values, not internal stage names. Stage 4 (Product Research) is hidden.

---

## Feature 1: Portal Page Setup

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P4-01 | As a **developer**, I need to create a SharePoint portal page template for each customer case so that customers have a branded landing page | [task-01-create-portal-page.md](task-01-create-portal-page.md) | 🔴 Critical |

---

## Feature 2: Progress Tracking

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P4-02 | As a **customer**, I want to see my application progress with customer-friendly stage names (✅ ▶ ○) so that I know where my case stands | [task-02-progress-tracker.md](task-02-progress-tracker.md) | 🔴 Critical |

---

## Feature 3: Action Items & Document Requests

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P4-03 | As a **customer**, I want to see what documents are still needed ("What you need to do" panel) so that I can take action promptly | [task-03-action-panel.md](task-03-action-panel.md) | 🔴 Critical |

---

## Feature 4: Fact-Find Access

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P4-04 | As a **customer**, I want a prominent button to open my fact-find form so that I can update my information easily | [task-04-factfind-portal-link.md](task-04-factfind-portal-link.md) | 🟡 High |

---

## Feature 5: Customer Messaging

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P4-05 | As a **customer**, I want to view messages from my advisor and send replies so that I can communicate without calling | [task-05-customer-messages.md](task-05-customer-messages.md) | 🟡 High |

---

## Feature 6: Document Upload & Viewing

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P4-06 | As a **customer**, I want to view and upload my documents so that I can provide everything the advisor needs | [task-06-customer-documents.md](task-06-customer-documents.md) | 🟡 High |

---

## Feature 7: Permissions & Security

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P4-07 | As a **developer**, I need to configure permissions so customers can only see their own data | [task-07-portal-permissions.md](task-07-portal-permissions.md) | 🔴 Critical |

---

## Feature 8: IT Admin Prerequisites

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P4-08 | As a **developer**, I need to verify all IT Admin prerequisites before go-live so that guest sharing and email delivery work correctly | [task-08-it-admin-prerequisites.md](task-08-it-admin-prerequisites.md) | 🔴 Critical |

---

## Acceptance Criteria (Phase 4 Complete)

- [ ] Customer portal page is accessible via a SharePoint guest link
- [ ] Portal shows CustomerFriendlyName stage values (not internal stage names)
- [ ] Stage 4 (Product Research) is NOT shown on the portal
- [ ] Completed stages show ✅, current stage shows ▶, future stages show ○
- [ ] "What you need to do" panel shows any outstanding document requests
- [ ] [Update Your Information] button opens the customer's fact-find edit form
- [ ] Customer can complete and submit the fact-find form from the portal
- [ ] [Upload Documents] link opens the correct upload folders for the customer's case
- [ ] Customer can view messages from their advisor and send a reply
- [ ] Customer cannot navigate to any other customer's data
- [ ] Customer cannot see folders 05–13, Commission records, or internal stage notes
- [ ] When case advances to a VisibleToCustomer stage, portal updates without manual intervention
