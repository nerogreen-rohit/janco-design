# 🏦 Specialist Lending Platform — High-Level Design Document

**Version:** 2.2  
**Date:** 10 April 2026  
**Platform:** Microsoft Power Platform + SharePoint Online

---

## Document Index

| # | Section | File |
|---|---|---|
| 1 | [Executive Summary](01-executive-summary.md) | [01-executive-summary.md](01-executive-summary.md) |
| 2 | [Solution Architecture Overview](02-solution-architecture.md) | [02-solution-architecture.md](02-solution-architecture.md) |
| 3 | [Product Types Supported](03-product-types.md) | [03-product-types.md](03-product-types.md) |
| 4 | [Lifecycle Stages](04-lifecycle-stages.md) | [04-lifecycle-stages.md](04-lifecycle-stages.md) |
| 5 | [SharePoint List Schemas](05-sharepoint-list-schemas.md) | [05-sharepoint-list-schemas.md](05-sharepoint-list-schemas.md) |
| 6 | [Document Library Structure](06-document-library.md) | [06-document-library.md](06-document-library.md) |
| 7 | [Screen Wireframes — Internal App](07-screen-wireframes-internal.md) | [07-screen-wireframes-internal.md](07-screen-wireframes-internal.md) |
| 8 | [External Customer Portal](08-external-customer-portal.md) | [08-external-customer-portal.md](08-external-customer-portal.md) |
| 9 | [End-to-End Process Flow](09-end-to-end-process.md) | [09-end-to-end-process.md](09-end-to-end-process.md) |
| 10 | [Automation Summary](10-automation-summary.md) | [10-automation-summary.md](10-automation-summary.md) |
| 11 | [KPI Dashboards](11-kpi-dashboards.md) | [11-kpi-dashboards.md](11-kpi-dashboards.md) |
| 12 | [Portfolio Clients](12-portfolio-clients.md) | [12-portfolio-clients.md](12-portfolio-clients.md) |
| 13 | [Licensing Recommendation](13-licensing.md) | [13-licensing.md](13-licensing.md) |
| 14 | [Future Enhancements](14-future-enhancements.md) | [14-future-enhancements.md](14-future-enhancements.md) |
| 15 | [Transformation Summary](15-before-vs-after.md) | [15-before-vs-after.md](15-before-vs-after.md) |
| 16 | [Implementation Timeline](16-implementation-timeline.md) | [16-implementation-timeline.md](16-implementation-timeline.md) |
| 17 | [Key Design Decisions](17-key-design-decisions.md) | [17-key-design-decisions.md](17-key-design-decisions.md) |

---

### Terminology

| Term | Meaning |
|---|---|
| **System Owner** | The business owner/operator of the platform |
| **Staff / Internal Users** | Team members who use the internal app |
| **Case Owner** | The staff member assigned to manage a specific case |
| **Advisor** | The professional advising the customer (used in customer-facing contexts) |
| **Customer** | The external applicant/borrower |
