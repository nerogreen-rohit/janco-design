[← Back to Index](00-index.md)

# 2. Solution Architecture Overview

```
┌──────────────────────────────────────────────────────────────────────┐
│                        MICROSOFT 365 TENANT                          │
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────────┐ │
│  │                  SHAREPOINT ONLINE SITE                         │ │
│  │              "Specialist Lending Hub"                           │ │
│  │                                                                 │ │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────────┐    │ │
│  │  │  Leads   │ │  Cases   │ │ Fact-Find│ │ Doc Requests     │    │ │
│  │  │  List    │ │  List    │ │ Responses│ │ List             │    │ │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────────────┘    │ │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────────┐    │ │
│  │  │  Stage   │ │Commission│ │Referrers │ │ Document         │    │ │
│  │  │  History │ │  List    │ │  List    │ │ Types            │    │ │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────────────┘    │ │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────────┐    │ │
│  │  │ Messages │ │ Lenders  │ │ Stage    │ │  Case Documents  │    │ │
│  │  │  List    │ │  List    │ │ Config   │ │  Library         │    │ │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────────────┘    │ │
│  │  ┌──────────┐ ┌──────────┐                                      │ │
│  │  │  Tasks   │ │ Counters │  Total: 13 lists + 1 doc library     │ │
│  │  │  List    │ │  List    │                                      │ │
│  │  └──────────┘ └──────────┘                                      │ │
│  └─────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│  ┌──────────────────────┐    ┌──────────────────────────────────┐    │
│  │   POWER APPS         │    │   POWER AUTOMATE                 │    │
│  │   (Canvas App)       │    │   (Cloud Flows)                  │    │
│  │  ┌────────────────┐  │    │  • ID Generation                 │    │
│  │  │ STAFF APP      │  │    │  • Lead → Case Conversion        │    │
│  │  │ (Internal)     │  │    │  • Fact-Find Item Creation       │    │
│  │  │ 2-4 users      │  │    │  • Stage Change Logging          │    │
│  │  └────────────────┘  │    │  • Doc Request Emails            │    │
│  └──────────────────────┘    │  • SLA Reminders                 │    │
│                              │  • Commission Calculation        │    │
│  ┌──────────────────────┐    │  • Email Docs to Lender          │    │
│  │  SHAREPOINT PAGES    │    │  • Customer Status Updates       │    │
│  │  (External Portal)   │    │  • Message Notifications         │    │
│  │  • Progress View     │    └──────────────────────────────────┘    │
│  │  • Doc Upload        │                                            │
│  │  • Messages          │    ┌──────────────────────────────────┐    │
│  │  • Fact-Find         │    │  FUTURE INTEGRATIONS             │    │
│  │  • E-Sign (future)   │    │  • E-Sign (DocuSign/Adobe/Box)   │    │
│  │  10-20 customers     │    │  • EPC API Lookup                │    │
│  └──────────────────────┘    │  • Power BI Dashboards           │    │
│                              └──────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────────────┘
```

| Component | Technology | Users |
|---|---|---|
| **Data Store** | SharePoint Online (13 lists + 1 document library) | — |
| **Internal App** | Power Apps (Canvas App) | 2–4 staff |
| **Automation** | Power Automate (16 cloud flows) | — |
| **Customer Portal** | SharePoint Pages + Shared List Items + Shared Folders | 10–20 external customers |
| **Future** | E-Sign integration, EPC API, Power BI | — |
