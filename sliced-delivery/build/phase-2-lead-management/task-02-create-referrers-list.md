# Task P2-02: Create the Referrers List

**Phase:** 2 — Lead Management  
**Story:** As a developer, I need to create the Referrers list so that introducer/referral partners can be tracked  
**Priority:** 🟡 High  
**Estimated effort:** 20 minutes  
**Depends on:** SharePoint site provisioned

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Go to **Site Contents** → **+ New** → **List** → **Blank list**
2. Name: `Referrers`
3. Description: `Track introducer and referral partners`
4. Click **Create**

### Step 2 — Add Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CompanyName | Single line of text | No | |
| ContactName | Single line of text | Yes | |
| Email | Single line of text | No | |
| Phone | Single line of text | No | |
| ReferrerType | Choice | No | Choices: `Introducer`, `Referral Partner`, `Network`, `Other` |
| DefaultSplitPercentage | Number | No | 2 decimal places — default referral fee split % |
| IsActive | Yes/No | Yes | Default: Yes |
| Notes | Multiple lines of text | No | Plain text |
| TotalCasesReferred | Number | No | 0 decimal places — updated by flow |
| TotalFeesEarned | Currency | No | Running total — updated by flow in Phase 5 |

> `Title` column stores the referrer/introducer name.

### Step 3 — Enter Initial Data

Populate with known referrers. Example:

| Title | CompanyName | ContactName | ReferrerType | DefaultSplitPercentage | IsActive |
|---|---|---|---|---|---|
| ABC Mortgages | ABC Mortgages Ltd | John Smith | Introducer | 25 | Yes |
| Network Homes | Network Homes Ltd | Sarah Jones | Network | 20 | Yes |

### Step 4 — Verify

- [ ] List appears in Site Contents as "Referrers"
- [ ] All columns present
- [ ] IsActive defaults to Yes
- [ ] Can be used as a Lookup source from the Leads list

---

## Importable Content

**CSV file:** [`imports/referrers-list-columns.csv`](imports/referrers-list-columns.csv)
