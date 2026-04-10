# Phase 1 — Fact-Find: User Stories & Task Index

**Goal:** Customer can complete their fact-find online. Staff can create a case and review the fact-find in the app.  
**Duration:** 2 weeks (Weeks 1–2)  
**Business value:** Immediate replacement of the Word-document-by-email fact-find process.

---

## Feature 1: SharePoint Lists Setup

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P1-01 | As a **developer**, I need to create the Cases SharePoint list (Phase 1 columns) so that case records can be stored | [task-01-create-cases-list.md](task-01-create-cases-list.md) | 🔴 Critical |
| P1-02 | As a **developer**, I need to create the Stage Configuration list with stages 1–3 so that case lifecycle tracking is ready | [task-02-create-stage-config-list.md](task-02-create-stage-config-list.md) | 🔴 Critical |
| P1-03 | As a **developer**, I need to create the Fact-Find Responses list (full schema — all sections) so that customers can complete their fact-find | [task-03-create-factfind-list.md](task-03-create-factfind-list.md) | 🔴 Critical |
| P1-04 | As a **developer**, I need to create the Counters list with all product type rows so that CaseIDs can be auto-generated | [task-04-create-counters-list.md](task-04-create-counters-list.md) | 🔴 Critical |

---

## Feature 2: Document Library Setup

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P1-05 | As a **developer**, I need to create the Case Documents library so that case files have a structured home | [task-05-create-document-library.md](task-05-create-document-library.md) | 🔴 Critical |

---

## Feature 3: Power Automate Flows

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P1-06 | As a **system**, when a new case is created I need to generate a unique CaseID (e.g. BTL-2026-0001) so that every case has a trackable reference | [task-06-flow-generate-case-id.md](task-06-flow-generate-case-id.md) | 🔴 Critical |
| P1-07 | As a **system**, when a new case is created I need to create a Fact-Find Responses item and generate a guest share link so that the customer can fill in their fact-find | [task-07-flow-create-factfind.md](task-07-flow-create-factfind.md) | 🔴 Critical |
| P1-08 | As a **system**, when a new case is created I need to create the 13 document subfolders and share folders 01–04 with the customer so that they can upload documents | [task-08-flow-create-folders.md](task-08-flow-create-folders.md) | 🔴 Critical |
| P1-09 | As a **system**, when a customer updates their fact-find I need to notify the case owner via Teams so that they can review changes promptly | [task-09-flow-factfind-notification.md](task-09-flow-factfind-notification.md) | 🟡 High |
| P1-10 | As a **system**, once all case setup flows complete I need to send a welcome email to the customer with their fact-find link and document upload links | [task-10-flow-welcome-email.md](task-10-flow-welcome-email.md) | 🔴 Critical |

---

## Feature 4: Staff App Screens

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P1-11 | As a **staff member**, I want a simple case creation form so that I can create a new case and trigger the automated setup | [task-11-staff-app-case-creation.md](task-11-staff-app-case-creation.md) | 🔴 Critical |
| P1-12 | As a **staff member**, I want a Fact-Find tab in the Case Workspace showing a read-only view of the customer's fact-find so that I can review their information | [task-12-staff-app-factfind-tab.md](task-12-staff-app-factfind-tab.md) | 🔴 Critical |

---

## Feature 5: Customer Experience & Permissions

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P1-13 | As a **customer**, I want to open my unique fact-find link without needing an M365 account so that I can complete my application easily | [task-13-customer-factfind-access.md](task-13-customer-factfind-access.md) | 🔴 Critical |
| P1-14 | As a **developer**, I need to configure permissions so that customers can only access their own data (fact-find item + folders 01–04) | [task-14-configure-permissions.md](task-14-configure-permissions.md) | 🔴 Critical |

---

## Acceptance Criteria (Phase 1 Complete)

- [ ] Staff can create a new case record from the Staff App
- [ ] CaseID is auto-generated (e.g. BTL-2026-0001) within 30 seconds of case creation
- [ ] A Fact-Find Responses item is created automatically and linked to the case
- [ ] Customer receives welcome email with working fact-find link and upload folder links
- [ ] Customer can open fact-find link without an M365 account
- [ ] Customer can fill in and save the fact-find form (multiple saves allowed)
- [ ] Case owner receives Teams notification when customer updates fact-find
- [ ] Staff can view all fact-find sections in the Fact-Find tab (read-only)
- [ ] Staff can mark fact-find as Reviewed
- [ ] Customer can upload files to folders 01–04
- [ ] Customer cannot access any other case's data or folders 05–13
