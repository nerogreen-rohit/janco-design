[← Back to Index](00-index.md)

# 12. Portfolio Clients

Portfolio clients are customers who have **4 or more mortgaged buy-to-let properties** (as defined by PRA regulations) or who have **multiple active cases** with the firm.

---

## 12.1 Identification

A customer is flagged as a portfolio client when either:
- `IsPortfolioLandlord = Yes` in the Fact-Find Responses list
- They have 2 or more active or completed cases in the Cases list linked to the same email address / applicant name

---

## 12.2 Multiple Cases per Client

The platform supports multiple cases per client. Each case is a separate item in the Cases list with its own CaseID, lifecycle, documents, and commission records.

```
Example: Portfolio client — David & Sue Patel

Cases List:
┌─────────────────┬────────────────┬──────────────┬────────────────┐
│  CaseID         │  Property      │  Product     │  Status        │
├─────────────────┼────────────────┼──────────────┼────────────────┤
│  BTL-2026-0003  │  32 Oak St,    │  BTL         │  Completed     │
│                 │  Manchester    │              │                │
├─────────────────┼────────────────┼──────────────┼────────────────┤
│  BTL-2026-0005  │  7 Park Lane,  │  BTL         │  Active        │
│                 │  Salford       │              │  Stage 8       │
├─────────────────┼────────────────┼──────────────┼────────────────┤
│  REFURB-2026-001│  12 High St,   │  Refurb      │  Active        │
│                 │  Leeds         │              │  Stage 4       │
└─────────────────┴────────────────┴──────────────┴────────────────┘
```

---

## 12.3 Portfolio Landlord — Additional Fields

When `IsPortfolioLandlord = Yes`, the following additional fields in the Fact-Find Responses list become relevant:

| Field | Purpose |
|---|---|
| TotalPortfolioProperties | Total number of mortgaged investment properties |
| TotalPortfolioValue | Estimated aggregate value of portfolio |
| TotalPortfolioMortgageBalance | Outstanding mortgages across portfolio |
| TotalPortfolioRentalIncome | Monthly rental income across portfolio |
| PortfolioDetails | Free text description of portfolio |

These fields are shown as a **Portfolio section** in both the Staff App (Fact-Find tab) and the Customer Portal fact-find form when `IsPortfolioLandlord = Yes`.

---

## 12.4 Portfolio View in Staff App

Staff can filter the Case List by applicant email/name to see all cases for a portfolio client. A future enhancement (v2.0) will add a dedicated "Portfolio Client" view showing all cases for a client side by side.

---

## 12.5 Commission Aggregation for Portfolio Clients

Each case maintains its own Commission records. The Commission Dashboard can be filtered by applicant name (via case name field) to show aggregate income from a single portfolio client across all their cases.

---

## 12.6 Lender Requirements for Portfolio Landlords

Portfolio landlords require additional underwriting information from most specialist lenders. The platform handles this through:
- Portfolio-specific fields in the Fact-Find Responses list
- Portfolio document types available in the Document Types list (e.g. "Schedule of Existing Properties", "Portfolio Summary Spreadsheet")
- Stage Notes field on the Case record for recording lender-specific portfolio assessment requirements

---

## 12.7 Fact-Find for Portfolio Clients with Multiple Cases

**Design decision:** Each case has its own Fact-Find Responses item. For portfolio clients with multiple active cases:
- Each case's Fact-Find item is independent
- The case owner can copy standard personal/income fields manually or via a future "Clone Fact-Find" flow
- This ensures each case has a complete, self-contained data record appropriate to that specific application

> **Rationale:** Separating fact-find by case avoids data contamination between different applications and preserves the integrity of the audit trail for each individual mortgage application.
