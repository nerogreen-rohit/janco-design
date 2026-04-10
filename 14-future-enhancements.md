<a>← Back to Index</a>

# 14. Future Enhancements

This section outlines the planned roadmap for the Specialist Lending Platform beyond the initial v1.0 launch.

---

## 14.1 Roadmap

| Version | Enhancement | Priority | Estimated Effort | License Impact |
|---|---|---|---|---|
| **v1.1** | E-Sign integration | High | Medium | Power Automate Premium |
| **v1.1** | PDF export of Fact-Find | High | Low–Medium | None (Encodian / Power Automate Premium) |
| **v1.2** | EPC API integration | Medium | Medium | Power Automate Premium (HTTP) |
| **v1.2** | Automated Fact-Find PDF on case creation | Medium | Low | None |
| **v1.3** | SMS notifications to customer | Medium | Low | Twilio connector (per message) |
| **v1.3** | Automated lender sourcing comparison table | Medium | High | None |
| **v2.0** | Portfolio Client view (all cases per client side-by-side) | Medium | Medium | None |
| **v2.0** | Power BI dashboards (replacing in-app charts) | Low–Medium | Medium | Power BI Pro |
| **v2.1** | Clone Fact-Find for portfolio clients | Medium | Low | None |
| **v2.1** | Referrer self-service portal | Low | High | SharePoint guest |
| **v2.2** | Mobile-optimised Staff App | Low | Medium | None |
| **v2.2** | Automated DIP submission (API integration) | Low | High | Premium connectors |
| **v3.0** | Full CRM integration (HubSpot / Dynamics) | Low | Very High | Additional licensing TBC |
| **v3.0** | AI-assisted case summarisation | Low | High | Copilot Studio licensing |

---

## 14.2 E-Sign Integration (v1.1 Detail)

E-sign is the highest priority future enhancement. It will replace the current manual process of posting/emailing Terms of Business for signature.

**Options considered:**

| Provider | Connector | Cost | Notes |
|---|---|---|---|
| DocuSign | Power Automate Premium | £ | Market leader, strong compliance |
| Adobe Acrobat Sign | Power Automate Premium | £ | Deep Microsoft 365 integration |
| Box Sign | Power Automate Premium | £ | Included in Box Business — if already subscribed |
| Microsoft Syntex eSign | Power Automate Standard | ££ (per transaction) | Native Microsoft — future option |

**Design:** When implemented, e-sign will be triggered at Stage 3 (Terms of Business). The flow will:
1. Generate the TOB from a template
2. Send to customer via the chosen e-sign provider
3. Monitor for completion
4. Store signed document in 06-Terms-of-Business folder
5. Advance case to Stage 4 automatically

---

## 14.3 EPC API Integration (v1.2 Detail)

The EPC (Energy Performance Certificate) API from the Ministry of Housing allows lookup of EPC ratings by property address/UPRN.

**Design:** A Power Automate flow will:
1. Trigger when a new case is created with a UK property address
2. Call the EPC API with the property postcode
3. If a match is found, populate EpcRating on the Case record and Fact-Find item automatically
4. Flag if EPC rating is below "C" (relevant for BTL lenders)

---

## 14.4 PDF Fact-Find Export (v1.1 Detail)

A formatted PDF of the completed Fact-Find will be generated automatically when the fact-find status is set to "Submitted". This will be:
- Stored in the 05-Fact-Find folder
- Attached to the case for quick reference
- Available to share with lenders as part of the application pack

**Options:** Encodian (premium connector) or HTML-to-PDF via Power Automate HTTP action.

---

## 14.5 Power BI Dashboards (v2.0 Detail)

The in-app dashboards (Section 11) will be enhanced with Power BI reports when the team scales to 4+ users and needs richer reporting.

Power BI reports will connect directly to SharePoint lists and provide:
- Interactive filtering and drill-down
- Trend charts over time
- Forecasting views
- Automatic email reports to the system owner

**Licensing:** Power BI Pro required per user who needs to share/consume embedded reports (£8.60/user/month).
