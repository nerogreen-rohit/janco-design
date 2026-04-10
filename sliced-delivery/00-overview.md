# Specialist Lending Platform — Sliced Delivery Plan

**Version:** 1.0  
**Date:** 10 April 2026  
**Based on design:** v2.2 (`00-index.md` → `17-key-design-decisions.md`)

---

## Overview

This folder contains the phased delivery plan for the Specialist Lending Platform. The design documents describe the full system. This plan slices that system into **6 sequential delivery phases**, each building on the last and each delivering standalone business value.

**Fact-Find is the first delivery.** It is the highest-value capability for the business (replaces Word-document-by-email immediately), it is self-contained, and it unblocks lender submissions without requiring the full platform to be live.

---

## Phase Summary

| Phase | Name | Weeks | Key Output |
|---|---|---|---|
| **1** | Fact-Find | 1–2 | Customer completes fact-find online; staff reviews in app |
| **2** | Lead Management | 3 | Structured lead capture, qualification, conversion to case |
| **3** | Case Management & Lifecycle | 4–5 | Full 14-stage lifecycle, documents, messages, SLA alerts |
| **4** | Customer Portal | 6 | Full customer-facing portal (progress, messaging, uploads) |
| **5** | Commissions & Dashboards | 7 | Commission tracking and all 4 KPI dashboards |
| **6** | UAT & Go Live | 8 | End-to-end testing, staff training, production handover |

---

## Phasing Rationale

### Why Fact-Find first?

The design explicitly documents three reasons that make Fact-Find the ideal first delivery (`17-key-design-decisions.md`, section 17.3):

1. **Single SharePoint list item per case** — no dependency on Microsoft Forms, no data sync, no secondary copy. The item is created by a single Power Automate flow (Flow 4) and accessed by the customer via a guest-shared direct link.
2. **No dependency on a rich UI** — the customer uses SharePoint's default list item edit form. No Power Apps portal screen is required for the customer in Phase 1.
3. **No dependency on the lead management pipeline** — Phase 1 allows staff to create a case record manually (bypassing the Lead → Convert flow) while Phase 2 (Lead Management) is built in parallel or subsequently.

### How the phases are connected

- Phase 1 creates the **Cases list** (minimal columns) and **Fact-Find Responses list** (full schema). All subsequent phases add to or extend the Cases list — they do not replace it.
- Phase 2 adds the **Leads list** and the **Convert Lead to Case** flow, which slots directly into the existing case creation mechanism built in Phase 1.
- Phase 3 adds the full stage lifecycle, document requests, messages, and tasks — all as new lists that reference the Cases list created in Phase 1.
- Phase 4 wraps Phase 1's customer Fact-Find access and Phase 3's document/message capabilities into a polished SharePoint portal page.
- Phase 5 adds commission tracking and dashboards — purely additive, no changes to existing lists or flows.
- Phase 6 is UAT and go-live across the full platform.

---

## Design Constraints (All Phases)

| Constraint | Detail | Source |
|---|---|---|
| No Dataverse | SharePoint Lists + Libraries only | `17-key-design-decisions.md` §17.1 |
| £0 additional licensing | M365 Business Standard only for v1.0 | `13-licensing.md` |
| No Power Apps license for customers | SharePoint guest sharing only | `17-key-design-decisions.md` §17.4 |
| No Microsoft Forms | Fact-find via SharePoint list item | `17-key-design-decisions.md` §17.3 |
| Advisory fee always labelled AdvFee | All flows, columns, UI labels | `17-key-design-decisions.md` §17.8 |
| Stage History is immutable | Written once by flow, never edited | `17-key-design-decisions.md` §17.9 |

---

## Document Index

| File | Contents |
|---|---|
| `00-overview.md` | This file — phasing rationale and summary |
| `phase-1-fact-find.md` | Phase 1 detail — Fact-Find first delivery |
| `phase-2-lead-management.md` | Phase 2 detail — Lead Management |
| `phase-3-case-management.md` | Phase 3 detail — Case Management & Lifecycle |
| `phase-4-customer-portal.md` | Phase 4 detail — Customer Portal |
| `phase-5-commissions-dashboards.md` | Phase 5 detail — Commissions & Dashboards |
| `phase-6-uat-golive.md` | Phase 6 detail — UAT & Go Live |
