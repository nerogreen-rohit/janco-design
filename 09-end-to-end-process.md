[← Back to Index](00-index.md)

# 9. End-to-End Process Flow

---

## 9.1 Full Lifecycle Process

```
┌──────────────────────────────────────────────────────────────────────┐
│                    END-TO-END PROCESS FLOW                          │
└──────────────────────────────────────────────────────────────────────┘

PHASE 1 — LEAD INTAKE
─────────────────────
[Enquiry Received]
      │
      ▼
[Staff creates Lead in Staff App]
  • Enter name, contact, product interest, loan amount
  • Assign to staff member
  • Set LeadStatus = "New"
      │
      ▼
[Staff qualifies lead]
  • Phone/email contact
  • Update LeadStatus = "Qualified" or "Lost"
      │ (if Qualified)
      ▼
[Convert Lead → Case]
  • Staff clicks [Convert to Case] in Staff App
  • Flow triggered:
    – Create Cases item
    – Generate CaseID
    – Create Fact-Find item
    – Create document folders
    – Set Stage to "1. Customer Enquiry"
    – Send welcome email to customer
    – Update Lead: ConvertedToCaseID, Status = "Converted"


PHASE 2 — FACT-FIND & ONBOARDING
──────────────────────────────────
[Stage 2: Fact-Find]
      │
      ├─▶ [Customer completes Fact-Find via portal link]
      │     • Fills in SharePoint list item (personal, income, property)
      │     • Uploads identity & income docs to shared folders
      │
      └─▶ [Case owner reviews Fact-Find in Staff App]
            • Reviews all sections
            • Requests missing documents
            • Marks Fact-Find as "Reviewed"
            │
            ▼
[Stage 3: Terms of Business & GDPR]
      • Case owner issues TOB and privacy notice
      • Customer signs (email/post — future: e-sign)
      • Staff uploads signed TOB to 06-Terms-of-Business folder


PHASE 3 — PRODUCT RESEARCH & PRESENTATION
───────────────────────────────────────────
[Stage 4: Product Research]  — Internal only, not visible to customer
      • Case owner researches lender panel
      • Identifies suitable products
      • Records findings in Stage Notes

[Stage 5: Present Options & Agree]
      • Case owner presents options to customer
      • Customer agrees preferred product/lender
      • Staff records decision in Case record

[Stage 6: Produce Illustrations]
      • Case owner generates ESIS/KFI documents
      • Uploads to 07-Illustrations folder
      • Sends to customer via Messages tab


PHASE 4 — APPLICATION
──────────────────────
[Stage 7: Decision in Principle]
      • Case owner submits DIP to lender
      • Records DIP result in Case
      • Uploads DIP confirmation to 08-DIP folder

[Stage 8: Full Application]
      • Case owner compiles full application pack
      • Uses "Email Docs to Lender" feature to send application
      • Updates Case record with submission details


PHASE 5 — LENDER PROCESS
─────────────────────────
[Stage 9: Valuation]
      • Lender instructs valuer
      • Valuation carried out
      • Staff uploads valuation report to 10-Valuation folder
      • Updates Case with valuation figure

[Stage 10: Underwriting]
      • Lender reviews application
      • Case owner manages queries and chases
      • Stage SLA alert fires if overdue

[Stage 11: Loan Offer]
      • Lender issues formal offer
      • Case owner uploads offer to 11-Offer folder
      • Customer notified via automated message


PHASE 6 — LEGALS & COMPLETION
───────────────────────────────
[Stage 12: Legals]
      • Solicitors instructed
      • Legal work progresses
      • Case owner manages timeline
      • Documents stored in 12-Legal folder

[Stage 13: Completion]
      • Funds released
      • Case owner confirms completion in Staff App
      • Customer receives automated completion notification
      • Commission records updated

[Stage 14: Post-Completion & Logging]
      • Final commission records entered/confirmed
      • Case archived
      • Documents filed
      • Case status set to "Completed"
```

---

## 9.2 Customer Journey

```
┌──────────────────────────────────────────────────────────────────────┐
│                        CUSTOMER JOURNEY                             │
└──────────────────────────────────────────────────────────────────────┘

CUSTOMER EXPERIENCE TIMELINE

Day 1
┌─────────────────────────────────────────────────────────────────┐
│  Customer submits enquiry (phone / email / website)             │
│  ↓                                                              │
│  Staff creates lead and makes initial contact                   │
│  ↓                                                              │
│  Lead converted to case — customer receives welcome email:      │
│    • Portal access link                                         │
│    • Fact-find link (direct to their SharePoint item)           │
│    • Document upload folder links                               │
│    • "Contact your advisor: sarah@[firm].co.uk"                 │
└─────────────────────────────────────────────────────────────────┘
      ↓

Days 1–5 (Fact-Find Stage)
┌─────────────────────────────────────────────────────────────────┐
│  Customer logs into portal and:                                 │
│    • Sees application progress tracker                          │
│    • Completes fact-find (personal, income, property details)   │
│    • Uploads identity and income documents                      │
│    • Sends message to advisor if questions arise                │
│                                                                 │
│  Case owner receives notification of each update and:           │
│    • Reviews fact-find in Staff App                             │
│    • Requests additional documents if needed                    │
│    • Responds to customer messages                              │
└─────────────────────────────────────────────────────────────────┘
      ↓

Days 5–10 (Research & Options)
┌─────────────────────────────────────────────────────────────────┐
│  Customer receives message:                                     │
│    "We've reviewed your information and are now researching     │
│     the best options for you."                                  │
│                                                                 │
│  Customer checks portal progress — stages 4 & 5 update         │
│                                                                 │
│  Customer receives illustration documents from advisor          │
└─────────────────────────────────────────────────────────────────┘
      ↓

Days 10–20 (Application & DIP)
┌─────────────────────────────────────────────────────────────────┐
│  Customer portal shows:                                         │
│    ▶ "Initial Decision" — DIP submitted                         │
│    ▶ "Application Submitted" — full application sent to lender  │
│                                                                 │
│  Customer messaged by advisor with updates                      │
└─────────────────────────────────────────────────────────────────┘
      ↓

Days 20–60 (Lender Process)
┌─────────────────────────────────────────────────────────────────┐
│  Portal shows stage progression:                                │
│    ▶ Valuation Arranged                                         │
│    ▶ Lender Review                                              │
│    ▶ Offer Issued  ← automated message sent to customer         │
│                                                                 │
│  Customer receives formal offer via advisor                     │
└─────────────────────────────────────────────────────────────────┘
      ↓

Days 60–90 (Legals & Completion)
┌─────────────────────────────────────────────────────────────────┐
│  Portal shows:                                                  │
│    ▶ Legal Work                                                 │
│    ▶ Funds Released  ← automated completion message             │
│                                                                 │
│  Customer receives congratulations message from advisor         │
│  Portal updated: Application Completed ✅                       │
└─────────────────────────────────────────────────────────────────┘

CUSTOMER TOUCHPOINTS SUMMARY

| Touchpoint | Method | Owner |
|---|---|---|
| Welcome email | Automated (Power Automate) | System |
| Fact-find | SharePoint list item (guest link) | Customer + Case Owner |
| Document upload | SharePoint folder (guest link) | Customer |
| Progress view | SharePoint page | Customer (read only) |
| Messages | SharePoint Messages list | Customer ↔ Advisor |
| Milestone notifications | Automated emails | System |
| Document requests | Email from Power Automate | Case Owner → Customer |
| Offer notification | Automated message | System |
| Completion notification | Automated message | System |
```
