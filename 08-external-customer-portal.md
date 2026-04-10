<a>← Back to Index</a>

# 8. External Customer Portal

The customer portal is built using SharePoint Pages and shared SharePoint list items — no additional Power Apps licensing is required for customers.

---

## 8.1 Customer Landing Page

```
╔══════════════════════════════════════════════════════════════════════╗
║                                                                      ║
║         🏦  YOUR LENDING APPLICATION                                 ║
║         Specialist Lending Hub — Customer Portal                     ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  Welcome, John & Jane Doe                                            ║
║  Case Reference: BTL-2026-0001                                       ║
║                                                                      ║
║  ─────────────────────────────────────────────────────────────────  ║
║  APPLICATION PROGRESS                                                ║
║  ─────────────────────────────────────────────────────────────────  ║
║                                                                      ║
║   ✅ Application Received                                            ║
║   ✅ Information Gathering                                           ║
║   ✅ Terms Agreed                                                    ║
║   ✅ Options Presented                                               ║
║   ✅ Illustrations Prepared                                          ║
║   ✅ Initial Decision                                                ║
║   ▶  Application Submitted  ← YOU ARE HERE                          ║
║   ○  Valuation Arranged                                              ║
║   ○  Lender Review                                                   ║
║   ○  Offer Issued                                                    ║
║   ○  Legal Work                                                      ║
║   ○  Funds Released                                                  ║
║                                                                      ║
║  ─────────────────────────────────────────────────────────────────  ║
║  WHAT YOU NEED TO DO                                                 ║
║  ─────────────────────────────────────────────────────────────────  ║
║                                                                      ║
║  ⚠  Please upload your bank statements (last 6 months)              ║
║  ⚠  Your EPC certificate was rejected — please re-upload            ║
║                                                                      ║
║  [📤 Upload Documents]                                               ║
║                                                                      ║
║  ─────────────────────────────────────────────────────────────────  ║
║  MESSAGES                                                            ║
║  ─────────────────────────────────────────────────────────────────  ║
║                                                                      ║
║  ✉  You have 1 unread message from your advisor                      ║
║                                                                      ║
║  [📨 View Messages]                                                  ║
║                                                                      ║
║  ─────────────────────────────────────────────────────────────────  ║
║  LINKS                                                               ║
║  ─────────────────────────────────────────────────────────────────  ║
║                                                                      ║
║  [📋 Update Your Information (Fact-Find)]                            ║
║  [📁 View Your Documents]                                            ║
║                                                                      ║
║  ─────────────────────────────────────────────────────────────────  ║
║                                                                      ║
║  Questions? Contact your advisor: sarah@[firmname].co.uk             ║
║                                                                      ║
╚══════════════════════════════════════════════════════════════════════╝
```

> **Note:** The customer sees **CustomerFriendlyName** values from the Stage Configuration list (e.g. "Information Gathering" instead of "Fact-Find") and only sees stages where `VisibleToCustomer = Yes`.

---

## 8.2 Customer Fact-Find Experience

The fact-find is a standard SharePoint list item edit form, accessed via a guest-shared direct link. The link is unique to each customer's fact-find item.

```
╔══════════════════════════════════════════════════════════════════════╗
║                                                                      ║
║  📋  YOUR APPLICATION INFORMATION                                    ║
║  BTL-2026-0001 — Please fill in all sections below                  ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  PERSONAL DETAILS — APPLICANT 1                                      ║
║  ┌─────────────────────────────────────────────────────────────┐    ║
║  │  Title:         [Mr ▼]                                       │    ║
║  │  First Name:    [John                               ]        │    ║
║  │  Last Name:     [Doe                                ]        │    ║
║  │  Date of Birth: [12/03/1978]                                 │    ║
║  │  NI Number:     [AB 12 34 56 C                      ]        │    ║
║  │  Marital Status:[Married ▼]                                  │    ║
║  └─────────────────────────────────────────────────────────────┘    ║
║                                                                      ║
║  CURRENT ADDRESS                                                     ║
║  ┌─────────────────────────────────────────────────────────────┐    ║
║  │  Address Line 1: [14 Maple Road                     ]        │    ║
║  │  Address Line 2: [                                  ]        │    ║
║  │  Town/City:      [Bristol                           ]        │    ║
║  │  Postcode:       [BS5 0AB                           ]        │    ║
║  │  Time at address:[6    ] years  [0    ] months               │    ║
║  └─────────────────────────────────────────────────────────────┘    ║
║                                                                      ║
║  INCOME — APPLICANT 1                                                ║
║  ┌─────────────────────────────────────────────────────────────┐    ║
║  │  Employment Status: [Employed ▼]                            │    ║
║  │  Employer Name:     [Bristol City Council           ]        │    ║
║  │  Job Title:         [Senior Analyst                 ]        │    ║
║  │  Gross Annual Salary: [£52,000                      ]        │    ║
║  └─────────────────────────────────────────────────────────────┘    ║
║                                                                      ║
║  [APPLICANT 2 DETAILS ▼]  [PROPERTY DETAILS ▼]  [LOAN DETAILS ▼]   ║
║                                                                      ║
║  Declaration: ☑ I confirm the information provided is accurate       ║
║                                                                      ║
║                       [💾 Save & Continue]  [📤 Submit]             ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## 8.3 Technical Flow — Customer Portal Access

```
┌──────────────────────────────────────────────────────────────────────┐
│                    CUSTOMER PORTAL TECHNICAL FLOW                   │
└──────────────────────────────────────────────────────────────────────┘

STEP 1: CASE CREATION
┌─────────────┐     Converts Lead      ┌─────────────┐
│  CASE OWNER │ ─────────────────────▶ │  Cases List │
│  (Staff App)│                        │  (New Item) │
└─────────────┘                        └──────┬──────┘
                                              │ Triggers
                                              ▼
STEP 2: AUTOMATED SETUP (Power Automate)
┌─────────────────────────────────────────────────────────────────┐
│  Flow: "Case Creation Setup"                                    │
│                                                                 │
│  1. Generate CaseID (from Counters list)                        │
│  2. Create Fact-Find Responses item (linked to CaseID)          │
│  3. Create case folder + 13 subfolders in Case Documents        │
│  4. Generate guest share link for Fact-Find item                │
│  5. Generate guest share link for upload folders (01-04)        │
│  6. Send welcome email to customer with portal links            │
└─────────────────────────────────────────────────────────────────┘
                                              │
                                              ▼
STEP 3: CUSTOMER ACCESS
┌─────────────────────────────────────────────────────────────────┐
│  Customer receives email with:                                  │
│  • Link to SharePoint page (progress view)                      │
│  • Direct link to Fact-Find item edit form                      │
│  • Link to document upload folders                              │
│  • Link to messages list filtered to their case                 │
│                                                                 │
│  No Power Apps license required for the customer.               │
│  No Microsoft Forms required.                                   │
│  Guest access controlled via SharePoint sharing.                │
└─────────────────────────────────────────────────────────────────┘
                                              │
                                              ▼
STEP 4: CUSTOMER UPDATES FACT-FIND
┌─────────────┐   Edits list item    ┌─────────────────────────┐
│  CUSTOMER   │ ────────────────────▶│  Fact-Find Responses    │
│  (Guest)    │                      │  List Item (their item  │
└─────────────┘                      │  only — direct link)    │
                                     └─────────────┬───────────┘
                                                   │ Triggers
                                                   ▼
                                     ┌─────────────────────────┐
                                     │  Flow: Update timestamp  │
                                     │  → Notify case owner     │
                                     └─────────────────────────┘

STEP 5: CUSTOMER UPLOADS DOCUMENTS
┌─────────────┐  Uploads to shared   ┌─────────────────────────┐
│  CUSTOMER   │ ────────────────────▶│  Case Documents Library │
│  (Guest)    │  folder link         │  /CaseID/01-Identity/   │
└─────────────┘                      │  /CaseID/02-Income/     │
                                     │  etc.                   │
                                     └─────────────┬───────────┘
                                                   │ Triggers
                                                   ▼
                                     ┌─────────────────────────┐
                                     │  Flow: Create Doc        │
                                     │  Request item as         │
                                     │  "Received" + notify     │
                                     │  case owner              │
                                     └─────────────────────────┘

PERMISSIONS SUMMARY
┌─────────────────────────────────────────────────────────────────┐
│  Customer can ACCESS:                                           │
│  • Their Fact-Find item (edit — direct guest shared link)       │
│  • Doc upload folders 01, 02, 03, 04 (contribute — guest link) │
│  • Messages list filtered to their CaseID (read/contribute)     │
│  • Progress page (read only)                                    │
│                                                                 │
│  Customer CANNOT access:                                        │
│  • Other customers' data                                        │
│  • Internal staff notes                                         │
│  • Commission records                                           │
│  • Internal-only stage names                                    │
│  • Folders 05–13 (staff only)                                   │
└─────────────────────────────────────────────────────────────────┘
```
