[← Back to Index](00-index.md)

# 4. Lifecycle Stages (Configurable)

The lifecycle uses a **common 14-stage spine** stored in the **Stage Configuration list**. This list is the **single source of truth** for all stage-related logic. The Cases list references it via a **Lookup**, not a hardcoded Choice.

## 4.1 Stage Definitions

| # | Stage Name | Description | Typical Owner |
|---|---|---|---|
| 1 | **Customer Enquiry** | Initial contact received | Case Owner |
| 2 | **Fact-Find** | Gather personal, financial, property details via shared list item | Case Owner + Customer |
| 3 | **Terms of Business &amp; GDPR** | Issue TOB, privacy notice, obtain consent (future: e-sign) | Case Owner → Customer |
| 4 | **Product Research** | Source suitable products (internal, not visible to customer) | Case Owner |
| 5 | **Present Options &amp; Agree** | Share options with client, agree approach (internal) | Case Owner |
| 6 | **Produce Illustrations** | Generate ESIS/KFI documents | Case Owner |
| 7 | **Decision in Principle (DIP)** | Submit DIP to lender | Case Owner → Lender |
| 8 | **Full Application** | Submit complete application + docs to lender | Case Owner → Lender |
| 9 | **Valuation** | Property valuation arranged and completed | Lender / Valuer |
| 10 | **Underwriting** | Lender reviews application | Lender |
| 11 | **Loan Offer** | Formal offer issued | Lender |
| 12 | **Legals** | Solicitors complete legal work | Solicitors |
| 13 | **Completion** | Funds released, case completes | All parties |
| 14 | **Post-Completion &amp; Logging** | Log final details, archive, commission | System Owner / Admin |

## 4.2 Stage Applicability by Product Type

| Stage | BTL | Bridge | Commercial | Dev Finance | Refurb | Auction |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| 1. Customer Enquiry | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 2. Fact-Find | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 3. TOB &amp; GDPR | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 4. Product Research | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 5. Present Options | ✅ | ⚪ | ✅ | ✅ | ✅ | ⚪ |
| 6. Illustrations | ✅ | ⚪ | ✅ | ⚪ | ⚪ | ⚪ |
| 7. DIP | ✅ | ⚪ | ✅ | ⚪ | ⚪ | ⚪ |
| 8. Full Application | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 9. Valuation | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 10. Underwriting | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 11. Loan Offer | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 12. Legals | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 13. Completion | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 14. Post-Completion | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

✅ = Required ⚪ = Optional / Skip allowed

Total rows in Stage Configuration: 14 stages × 6 products = 84 rows
