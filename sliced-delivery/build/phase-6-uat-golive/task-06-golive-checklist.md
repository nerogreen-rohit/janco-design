# Task P6-06: Go-Live Day Checklist and Post-Launch Support

**Phase:** 6 — UAT & Go Live  
**Task:** Execute the go-live day checklist and establish post-launch support  
**Priority:** 🔴 Critical  
**Estimated effort:** Full day (go-live) + 2 weeks (post-launch support)  
**Depends on:** P6-01 to P6-05 all complete

---

## Overview

This checklist covers everything needed before, during, and after go-live day. The platform is considered live once the System Owner gives final confirmation and new cases are being created in the production environment.

---

## Part 1: Pre-Go-Live

All of these must be complete **before** go-live day.

| # | Task | Owner | Status | Date Completed |
|---|---|---|---|---|
| 1.1 | All 10 test scenarios passed (P6-01) | Developer + System Owner | ☐ | |
| 1.2 | All 57 permission tests passed (P6-02) | Developer | ☐ | |
| 1.3 | Production configuration complete (P6-03) | Developer + IT Admin | ☐ | |
| 1.4 | Data migration complete — all open cases migrated (P6-04) | System Owner | ☐ | |
| 1.5 | All staff trained (P6-05) — Sessions 1–6 complete | Developer | ☐ | |
| 1.6 | System Owner trained on admin screens (P6-05, Session 7) | Developer | ☐ | |
| 1.7 | System owner sign-off on all screens and flows | System Owner | ☐ | |
| 1.8 | Test data removed from production site | Developer | ☐ | |
| 1.9 | All 16 flows verified and turned On in production | Developer | ☐ | |
| 1.10 | Flow 9 (SLA Reminder) schedule confirmed — daily 08:00 | Developer | ☐ | |
| 1.11 | Staff App bookmarked on all staff devices | System Owner | ☐ | |
| 1.12 | Support contact details shared with all staff | Developer | ☐ | |

---

## Part 2: Go-Live Day

Execute these tasks on go-live day, in order.

| # | Task | Owner | Status | Time Completed |
|---|---|---|---|---|
| 2.1 | **Morning check:** All 16 flows show "On" in Power Automate | Developer | ☐ | |
| 2.2 | Switch off any remaining test/staging sharing links | Developer | ☐ | |
| 2.3 | Verify production guest sharing links are active for migrated cases | Developer / Case Owner | ☐ | |
| 2.4 | Send new portal welcome emails to any active customers (if not already sent during migration) | Case Owner | ☐ | |
| 2.5 | Create the first real case in production to verify full flow | System Owner | ☐ | |
| 2.6 | Verify CaseID generation works (check Counters list incremented) | Developer | ☐ | |
| 2.7 | Verify welcome email is received by test/first customer | Developer | ☐ | |
| 2.8 | Verify Fact-Find guest link works (open in private browser) | Developer | ☐ | |
| 2.9 | Confirm SLA reminder flow ran at 08:00 (or trigger manually) | Developer | ☐ | |
| 2.10 | Announce to all staff: **"The platform is now live"** | System Owner | ☐ | |
| 2.11 | Retire the old Excel/Word process — archive existing spreadsheets | System Owner | ☐ | |
| 2.12 | Monitor for first 4 hours — check flow run history for errors | Developer | ☐ | |
| 2.13 | End-of-day check: review all flow runs from the day | Developer | ☐ | |
| 2.14 | System Owner final confirmation: **"Platform is operational"** | System Owner | ☐ | |

---

## Part 3: Post-Launch Support

A **2-week priority support window** begins on go-live day.

### Support Window Details

| Item | Detail |
|---|---|
| **Duration** | 2 weeks from go-live date |
| **Start date** | _________________ |
| **End date** | _________________ |
| **Support contact** | Developer (direct Teams/email) |
| **Response time — critical issues** | < 4 business hours |
| **Response time — non-critical issues** | < 1 business day |
| **Escalation path** | Staff → System Owner → Developer |

### Support Commitments

| # | Commitment | Details |
|---|---|---|
| 3.1 | Bug fixes prioritised above any new feature work | Any confirmed bug takes precedence during the support window |
| 3.2 | System Owner can raise issues directly with the developer | Direct communication channel (Teams chat or dedicated channel) |
| 3.3 | Flow failure monitoring and resolution | Any flow failures resolved within < 1 business day |
| 3.4 | Additional training availability | Staff can request additional 1-on-1 training on specific features |
| 3.5 | Daily monitoring of flow run history | Developer checks Power Automate run history each morning during support window |

### Daily Monitoring Checklist (First 2 Weeks)

Run this checklist each morning:

| # | Check | Status |
|---|---|---|
| 3.6 | Flow 9 (SLA Reminder) ran successfully at 08:00 | ☐ |
| 3.7 | No failed flow runs in the last 24 hours | ☐ |
| 3.8 | No "Access Denied" reports from staff or customers | ☐ |
| 3.9 | No duplicate CaseID issues | ☐ |
| 3.10 | Dashboards displaying correctly | ☐ |

### Issue Log

Track any issues reported during the support window:

| # | Date Reported | Reported By | Description | Severity | Status | Resolution | Date Resolved |
|---|---|---|---|---|---|---|---|
| 1 | | | | | ☐ Open | | |
| 2 | | | | | ☐ Open | | |
| 3 | | | | | ☐ Open | | |
| 4 | | | | | ☐ Open | | |
| 5 | | | | | ☐ Open | | |

---

## Part 4: Post-Support Transition

After the 2-week support window ends:

| # | Task | Owner | Status |
|---|---|---|---|
| 4.1 | Review all issues logged during support window | Developer + System Owner | ☐ |
| 4.2 | Confirm all critical issues resolved | Developer | ☐ |
| 4.3 | Document any known issues or workarounds | Developer | ☐ |
| 4.4 | Hand over ongoing platform ownership to System Owner | Developer | ☐ |
| 4.5 | Agree ongoing support terms (if applicable) | Developer + System Owner | ☐ |
| 4.6 | Schedule a 30-day review meeting | System Owner | ☐ |

---

## Part 5: Future Enhancements Roadmap

After successful go-live, the following enhancements are planned for future versions. These are additions to the live platform — no rework of existing lists or flows is required.

### v1.1 — High Priority

| Enhancement | Description | License Impact |
|---|---|---|
| **E-Sign integration** | DocuSign/Adobe/Box Sign at Stage 3 (Terms of Business). Flow generates TOB, sends for e-signature, stores signed document in folder 06, advances case automatically. | Power Automate Premium |
| **PDF export of Fact-Find** | Auto-generate formatted PDF when Fact-Find Status = "Submitted". Stored in folder 05-Fact-Find. Available to share with lenders. | None / Encodian |

### v1.2 — Medium Priority

| Enhancement | Description | License Impact |
|---|---|---|
| **EPC API integration** | Auto-populate EpcRating from property postcode using Ministry of Housing EPC API. Flags properties below EPC rating "C" for BTL lenders. | Power Automate Premium (HTTP) |

### v1.3 — Medium Priority

| Enhancement | Description | License Impact |
|---|---|---|
| **SMS notifications** | Send SMS notifications to customers for key stage changes and document requests via Twilio connector. | Twilio connector (per message) |

### v2.0 — Medium Priority

| Enhancement | Description | License Impact |
|---|---|---|
| **Portfolio Client view** | Dedicated view showing all cases for the same client side-by-side. Filter and compare across a landlord's portfolio. | None |
| **Power BI dashboards** | Replace in-app charts with interactive Power BI reports. Adds drill-down, trend charts, forecasting, and automated email reports. | Power BI Pro |

### v2.1 — Medium–Low Priority

| Enhancement | Description | License Impact |
|---|---|---|
| **Clone Fact-Find** | For portfolio clients, copy personal and income fields from an existing Fact-Find to a new case. Saves re-entry time. | None |
| **Referrer self-service portal** | Referrers can submit leads and view their referral fee status via a guest SharePoint portal. | SharePoint guest |

### v3.0 — Low Priority (Long-Term)

| Enhancement | Description | License Impact |
|---|---|---|
| **Full CRM integration** | Integrate with HubSpot or Dynamics 365 for lead management, marketing automation, and advanced reporting. | Additional licensing TBC |

---

## Final Sign-Off

### Go-Live Day Sign-Off

| Role | Name | Date | Confirmation |
|---|---|---|---|
| Developer | | | ☐ Platform is live and all flows operational |
| System Owner | | | ☐ Platform is accepted — staff are using it |
| IT Admin | | | ☐ Tenant configuration is confirmed |

### Post-Support Window Sign-Off (2 weeks later)

| Role | Name | Date | Confirmation |
|---|---|---|---|
| Developer | | | ☐ All support issues resolved |
| System Owner | | | ☐ Platform is stable — ready for independent operation |

---

> **The platform is officially live when the System Owner confirms "Platform is operational" on go-live day.** The Excel/Word process is retired from that point forward.
