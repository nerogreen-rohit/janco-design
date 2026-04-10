# Task P2-01: Create the Leads SharePoint List

**Phase:** 2 — Lead Management  
**Story:** As a developer, I need to create the Leads SharePoint list so that enquiries can be captured and tracked  
**Priority:** 🔴 Critical  
**Estimated effort:** 30–45 minutes  
**Depends on:** Referrers list (P2-02) for ReferrerID lookup

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Go to **Site Contents** → **+ New** → **List** → **Blank list**
2. Name: `Leads`
3. Description: `Capture and track inbound enquiries before conversion to a case`
4. Click **Create**

### Step 2 — Add Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| LeadID | Single line of text | No | Auto-set by Flow 1 |
| FirstName | Single line of text | Yes | |
| LastName | Single line of text | Yes | |
| Email | Single line of text | Yes | |
| Phone | Single line of text | No | |
| ProductInterest | Choice | Yes | Choices: `BTL`, `Bridging`, `Commercial`, `Dev Finance`, `Refurb`, `Auction` |
| LoanAmountRequired | Currency | No | |
| PropertyAddress | Multiple lines of text | No | Plain text |
| LeadSource | Choice | No | Choices: `Website`, `Referral`, `Introducer`, `Direct`, `Other` |
| ReferrerID | Lookup | No | Lookup list: `Referrers`, column: `Title` |
| LeadStatus | Choice | Yes | Choices: `New`, `Contacted`, `Qualified`, `Converted`, `Lost`, `Dormant`. Default: `New` |
| AssignedTo | Person or Group | No | Allow selection of people only |
| Notes | Multiple lines of text | No | Plain text |
| ConvertedToCaseID | Single line of text | No | Populated on conversion |
| ConvertedDate | Date and Time | No | Date only |

> `Title` column will be set to LeadID by Flow 1.

### Step 3 — Create Views

**Active Leads view:**
- Filter: `LeadStatus` not equal to `Converted` AND not equal to `Lost` AND not equal to `Dormant`
- Columns: LeadID, FirstName, LastName, ProductInterest, LeadStatus, AssignedTo, Created
- Sort: Created (descending)

**All Leads view:**
- No filter
- Columns: LeadID, FirstName, LastName, ProductInterest, LeadStatus, AssignedTo, ConvertedToCaseID

### Step 4 — Verify

- [ ] List appears in Site Contents as "Leads"
- [ ] All columns are present with correct types
- [ ] LeadStatus defaults to "New"
- [ ] ReferrerID lookup connects to the Referrers list
- [ ] Active Leads view hides Converted/Lost/Dormant leads

---

## Importable Content

**CSV file:** [`imports/leads-list-columns.csv`](imports/leads-list-columns.csv)
