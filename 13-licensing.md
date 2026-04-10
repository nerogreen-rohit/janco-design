[← Back to Index](00-index.md)

# 13. Licensing Recommendation

---

## 13.1 Summary

The platform is designed to operate at **£0 additional licensing cost** on top of existing Microsoft 365 Business Standard subscriptions.

---

## 13.2 License Requirements

| User Type | Count | License Required | Cost |
|---|---|---|---|
| Staff (internal app users) | 2–4 | M365 Business Standard (already held) | £0 additional |
| System Owner / Admin | 1 | M365 Business Standard (already held) | £0 additional |
| Customers (portal access) | 10–20 | None — SharePoint guest sharing | £0 |
| Power Apps | 2–4 staff | Included in M365 Business Standard (Power Apps for M365) | £0 additional |
| Power Automate | All flows | Included in M365 Business Standard | £0 additional |
| SharePoint | All lists + library | Included in M365 Business Standard | £0 additional |

---

## 13.3 What's Included in M365 Business Standard

| Feature | Included | Notes |
|---|---|---|
| SharePoint Online | ✅ | Unlimited lists, up to 1TB storage per site |
| Power Apps for Microsoft 365 | ✅ | Canvas apps connected to SharePoint — included |
| Power Automate for Microsoft 365 | ✅ | Standard connectors including SharePoint — included |
| SharePoint Guest Sharing | ✅ | External sharing included — requires tenant admin to allow |
| Power BI (basic) | ✅ | Power BI free — publish to web only |

---

## 13.4 Licensing Constraints to be Aware Of

| Constraint | Detail | Mitigation |
|---|---|---|
| Power Apps for M365 | Only connects to Microsoft 365 data sources (SharePoint, Excel, Teams) — NOT Dataverse, SQL, etc. | Mitigated by SharePoint-only design |
| Power Automate for M365 | Standard connectors only — no premium connectors (e.g. DocuSign HTTP, PDF generation) | For e-sign and advanced PDF: would require Power Automate Premium (£12.20/user/month) or per-flow plan |
| SharePoint Guest Access | Tenant admin must enable external sharing; guests can access shared items/folders only | Ensure IT admin has enabled "New and existing guests" sharing at tenant level |
| Power BI Shared | Full Power BI sharing with colleagues requires Power BI Pro (£8.60/user/month) | Initial dashboards built in Power Apps (no extra cost); Power BI as future enhancement |
| E-Sign | DocuSign/Adobe Sign/Box Sign premium connectors require Power Automate Premium | Budget for premium plan when e-sign is activated (v1.1) |

---

## 13.5 Future License Costs (Planned Enhancements)

| Enhancement | Additional License | Estimated Cost |
|---|---|---|
| E-Sign integration (DocuSign/Adobe/Box) | Power Automate Premium per user | £12.20/user/month |
| Power BI dashboards (shared with team) | Power BI Pro per user | £8.60/user/month |
| EPC API integration | Power Automate Premium (HTTP connector) | £12.20/user/month |
| Scale to 5+ staff | Additional M365 Business Standard seats | £9.40/user/month (approx.) |

---

## 13.6 Current Annual Cost Estimate

| Item | Cost |
|---|---|
| 2 × M365 Business Standard (existing) | Already subscribed |
| All platform components (SharePoint, Power Apps, Power Automate) | £0 additional |
| Customer portal access (10–20 customers) | £0 |
| **TOTAL ADDITIONAL COST** | **£0** |

> **Note:** This assumes the organisation already holds M365 Business Standard licenses for the 2–4 staff users. If not, M365 Business Standard is approximately £9.40–£12.50/user/month depending on contract.
