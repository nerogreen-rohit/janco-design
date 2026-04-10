# 🏦 Specialist Lending Platform — High-Level Design Document

**Version:** 2.2  
**Date:** 10 April 2026  
**Platform:** Microsoft Power Platform + SharePoint Online

---

## Document Index

| # | Section | File |
|---|---|---|
| 1 | <a>Executive Summary</a> | `01-executive-summary.md` |
| 2 | <a>Solution Architecture Overview</a> | `02-solution-architecture.md` |
| 3 | <a>Product Types Supported</a> | `03-product-types.md` |
| 4 | <a>Lifecycle Stages</a> | `04-lifecycle-stages.md` |
| 5 | <a>SharePoint List Schemas</a> | `05-sharepoint-list-schemas.md` |
| 6 | <a>Document Library Structure</a> | `06-document-library.md` |
| 7 | <a>Screen Wireframes — Internal App</a> | `07-screen-wireframes-internal.md` |
| 8 | <a>External Customer Portal</a> | `08-external-customer-portal.md` |
| 9 | <a>End-to-End Process Flow</a> | `09-end-to-end-process.md` |
| 10 | <a>Automation Summary</a> | `10-automation-summary.md` |
| 11 | <a>KPI Dashboards</a> | `11-kpi-dashboards.md` |
| 12 | <a>Portfolio Clients</a> | `12-portfolio-clients.md` |
| 13 | <a>Licensing Recommendation</a> | `13-licensing.md` |
| 14 | <a>Future Enhancements</a> | `14-future-enhancements.md` |
| 15 | <a>Transformation Summary</a> | `15-before-vs-after.md` |
| 16 | <a>Implementation Timeline</a> | `16-implementation-timeline.md` |
| 17 | <a>Key Design Decisions</a> | `17-key-design-decisions.md` |

---

### Terminology

| Term | Meaning |
|---|---|
| **System Owner** | The business owner/operator of the platform |
| **Staff / Internal Users** | Team members who use the internal app |
| **Case Owner** | The staff member assigned to manage a specific case |
| **Advisor** | The professional advising the customer (used in customer-facing contexts) |
| **Customer** | The external applicant/borrower |
