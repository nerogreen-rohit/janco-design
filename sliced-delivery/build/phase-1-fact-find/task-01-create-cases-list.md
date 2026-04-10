# Task P1-01: Create the Cases SharePoint List

**Phase:** 1 — Fact-Find  
**Story:** As a developer, I need to create the Cases SharePoint list (Phase 1 columns) so that case records can be stored  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** SharePoint site provisioned

---

## Step-by-Step Guide

### Step 1 — Navigate to Site Contents

1. Open your SharePoint site: `https://[tenant].sharepoint.com/sites/lending`
2. Click **⚙️ Settings** → **Site contents**
3. Click **+ New** → **List** → **Blank list**
4. Name: `Specialist Lending Cases`
5. Description: `Central case record for each active or historical lending application`
6. Click **Create**

### Step 2 — Add Phase 1 Required Columns

Add each column in order. Use **+ Add column** or **List Settings → Create column**.

#### Identity Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CaseID | Single line of text | No | Will be set by Flow 3 |
| ProductType | Choice | Yes | Choices: `BTL`, `Bridging`, `Commercial`, `Dev Finance`, `Refurb`, `Auction` |
| CaseStatus | Choice | Yes | Choices: `Active`, `On Hold`, `Withdrawn`, `Declined`, `Completed`, `Archived`. Default: `Active` |
| CaseOwner | Person or Group | Yes | Allow selection of people only |

#### Applicant Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| Applicant1FirstName | Single line of text | Yes | |
| Applicant1LastName | Single line of text | Yes | |
| Applicant1Email | Single line of text | Yes | |
| Applicant1Phone | Single line of text | No | |
| Applicant2FirstName | Single line of text | No | |
| Applicant2LastName | Single line of text | No | |
| Applicant2Email | Single line of text | No | |

#### Property & Loan Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| PropertyAddress | Multiple lines of text | Yes | Plain text |
| PropertyPostcode | Single line of text | Yes | |
| PropertyType | Choice | No | Choices: `Residential`, `HMO`, `MUB`, `Commercial`, `Semi-Commercial`, `Land`, `Other` |
| EstimatedValue | Currency | No | |
| PurchasePrice | Currency | No | |
| LoanAmountRequired | Currency | Yes | |
| LTV | Number | No | Percentage, 0 decimal places |
| InterestRateType | Choice | No | Choices: `Fixed`, `Variable`, `Tracker` |
| TermMonths | Number | No | |
| RepaymentType | Choice | No | Choices: `Interest Only`, `Capital & Interest` |

#### Stage Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CurrentStageID | Lookup | No | Lookup list: `Stage Configuration`, Lookup column: `Title` |
| CurrentStageNumber | Number | No | 0 decimal places |
| CurrentStageName | Single line of text | No | |
| StageChangedDate | Date and Time | No | Date only |
| StageNotes | Multiple lines of text | No | Plain text |

#### Reference Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| LeadID | Single line of text | No | Will be populated by Flow 2 in Phase 2 |
| FactFindItemID | Number | No | FK to Fact-Find Responses list item ID |

#### Date Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| EnquiryDate | Date and Time | Yes | Date only. Default: Today |
| TargetCompletionDate | Date and Time | No | Date only |

#### Future Columns (create now, leave empty until Phase 3+)

| Column Name | Type | Notes |
|---|---|---|
| IsPortfolioLandlord | Yes/No | Default: No |
| MonthlyRent | Currency | BTL only |
| ICRMultiplier | Number | BTL only |
| CalculatedMaxLoan | Currency | BTL only |
| EpcRating | Choice | Choices: `A`, `B`, `C`, `D`, `E`, `F`, `G` |
| AuctionDate | Date and Time | Bridging/Auction |
| CompletionDeadline | Date and Time | Bridging/Auction |
| ExitStrategy | Choice | Choices: `Refinance`, `Sale`, `Other` |
| WorksRequired | Yes/No | |
| WorksBudget | Currency | |
| BuildCost | Currency | Dev Finance/Refurb |
| EstimatedGDV | Currency | Dev Finance/Refurb |
| ProfitOnGDV | Number | Dev Finance — percentage |
| ProcFeeExpected | Currency | Phase 5 |
| AdvFeeCharged | Currency | Phase 5 |
| TotalCommissionExpected | Currency | Phase 5 |
| CommissionReceived | Yes/No | Phase 5 |
| CommissionReceivedDate | Date and Time | Phase 5 |
| LenderID | Lookup | → Lenders list (Phase 3) |
| LenderName | Single line of text | Denormalized |
| ReferrerID | Lookup | → Referrers list (Phase 2) |
| ActualCompletionDate | Date and Time | |

### Step 3 — Rename the Title Column

1. Go to **List Settings** → click **Title** column
2. Change the display name to `Title`
3. Set **Require that this column contains information** = No
4. This column will be auto-set to CaseID by Flow 3

### Step 4 — Create a Default View

1. Go to **List Settings** → **Views** → **Create view**
2. Name: `Active Cases`
3. Filter: `CaseStatus is equal to Active`
4. Columns to show: `CaseID`, `ProductType`, `Applicant1FirstName`, `Applicant1LastName`, `CurrentStageName`, `CaseOwner`, `EnquiryDate`
5. Sort by: `EnquiryDate` (descending)

### Step 5 — Verify

- [ ] List appears in Site Contents as "Specialist Lending Cases"
- [ ] All Phase 1 required columns are present
- [ ] ProductType choice values are correct (6 options)
- [ ] CaseStatus choice values are correct (6 options)
- [ ] CurrentStageID lookup points to Stage Configuration list
- [ ] Default view shows key columns

---

## Importable Content

See [`imports/cases-list-columns.csv`](imports/cases-list-columns.csv) for the complete column definition that can be referenced during creation.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Lookup column for CurrentStageID not available | Create the Stage Configuration list first (Task P1-02), then return to add this column |
| Column limit warning | SharePoint lists support up to 500 columns — this list uses ~40, well within limits |
| Choice values not saving | Ensure each choice is on a separate line in the Choices box |
