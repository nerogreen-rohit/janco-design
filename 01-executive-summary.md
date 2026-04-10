[← Back to Index](00-index.md)

# 1. Executive Summary

**Prepared for:** [Client Name]  
**Date:** 10 April 2026  
**Version:** 2.2  
**Platform:** Microsoft Power Platform + SharePoint Online

---

This document presents the design for a **Specialist Lending Platform** built on Microsoft Power Platform and SharePoint Online. The system replaces manual Excel/Word processes with a structured, automated workflow covering:

- **Lead capture and management**
- **Case/application management** across 6 specialist lending product types
- **14-stage flexible lifecycle** driven by a configurable Stage Configuration list (single source of truth)
- **Customer portal** (external access for document upload, progress view, messaging, fact-find, and e-sign)
- **Fact-find data collection** via a shared SharePoint list item (no Microsoft Forms dependency)
- **Commission tracking** (lender proc fees, advisory fees, introducer/referral fees)
- **Document management** with email-to-lender capability
- **Dashboards and KPIs**

---

## Design Principles

| Principle | Detail |
|---|---|
| **No Dataverse** | SharePoint Lists + Libraries only |
| **Minimum licensing cost** | £0 additional on M365 Business Standard |
| **Configurable lifecycle** | Stage progression driven by a Lookup to a configurable list (not hardcoded Choices) |
| **Scalable** | Start with 2 users, scale to 4+ |
| **External customer access** | Via SharePoint guest sharing (10–20 active customers) — no Power Apps license needed for customers |
| **Single source of truth** | Fact-find collected directly via SharePoint list item — no Forms sync needed |
| **Future-ready** | Provision for e-sign integration (DocuSign, Adobe Sign, or Box Sign) |
