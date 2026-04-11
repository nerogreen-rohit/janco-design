# Phase 2 — Lead Management: User Stories & Task Index

**Goal:** Structured lead capture, qualification, and one-click conversion to a case.  
**Duration:** 1 week (Week 3)  
**Depends on:** Phase 1 complete (Cases list, Counters list, Flows 3/4/5 operational)  
**Business value:** Replaces email inbox, sticky notes, and spreadsheets with a structured pipeline.

---

## Feature 1: SharePoint Lists Setup

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P2-01 | As a **developer**, I need to create the Leads SharePoint list so that enquiries can be captured and tracked | [task-01-create-leads-list.md](task-01-create-leads-list.md) | 🔴 Critical |
| P2-02 | As a **developer**, I need to create the Referrers list so that introducer/referral partners can be tracked | [task-02-create-referrers-list.md](task-02-create-referrers-list.md) | 🟡 High |
| P2-03 | As a **developer**, I need to add the LEAD counter row to the Counters list so that LeadIDs can be auto-generated | [task-03-add-lead-counter.md](task-03-add-lead-counter.md) | 🔴 Critical |

---

## Feature 2: Power Automate Flows

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P2-04 | As a **system**, when a new lead is created I need to generate a unique LeadID (e.g. LEAD-0001) | [task-04-flow-generate-lead-id.md](task-04-flow-generate-lead-id.md) | 🔴 Critical |
| P2-05 | As a **staff member**, when I click [Convert to Case] on a qualified lead, the system should create a Case record pre-populated from the lead data and mark the lead as converted | [task-05-flow-convert-lead-to-case.md](task-05-flow-convert-lead-to-case.md) | 🔴 Critical |

---

## Feature 3: Staff App Screens

| Story ID | User Story | Task Guide | Priority |
|---|---|---|---|
| P2-06 | As a **staff member**, I want a Leads screen with filtering so that I can view, search, and manage all enquiries | [task-06-staff-app-leads-screen.md](task-06-staff-app-leads-screen.md) | 🔴 Critical |
| P2-07 | As a **staff member**, I want a Lead detail/edit form and a [Convert to Case] button so that I can manage leads and convert qualified ones | [task-07-staff-app-lead-detail.md](task-07-staff-app-lead-detail.md) | 🔴 Critical |

---

## Acceptance Criteria (Phase 2 Complete)

- [ ] Staff can create a new lead from the Leads screen
- [ ] LeadID is auto-generated (e.g. LEAD-0024) on lead creation
- [ ] Staff can update lead status through the pipeline (New → Contacted → Qualified → Lost)
- [ ] [Convert to Case] button is visible when LeadStatus = "Qualified"
- [ ] Clicking [Convert to Case] creates a Case record pre-populated with lead data
- [ ] Lead is updated with ConvertedToCaseID and Status = "Converted"
- [ ] Customer receives welcome email (fact-find link + upload folders) via the existing Flow 5
- [ ] Converted leads are no longer shown in the active leads filter
- [ ] Staff can assign leads to team members and filter by assigned staff
- [ ] Referrers list is populated and can be selected on a lead (ReferrerID lookup)
