# Task P1-03: Create the Fact-Find Responses List

**Phase:** 1 — Fact-Find  
**Story:** As a developer, I need to create the Fact-Find Responses list (full schema — all sections) so that customers can complete their fact-find  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours (many columns)  
**Depends on:** Cases list created (Task P1-01)

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Go to **Site Contents** → **+ New** → **List** → **Blank list**
2. Name: `Fact-Find Responses`
3. Description: `Stores the fact-find data for each case as a single SharePoint list item`
4. Click **Create**

### Step 2 — Identity Columns

| Column Name | Type | Required | Notes |
|---|---|---|---|
| CaseID | Single line of text | Yes | FK to Cases list — set by Flow 4 |
| CaseRef | Single line of text | No | Human-readable case reference |

> `Title` column will be set to CaseID by Flow 4.

### Step 3 — Applicant 1 Personal Details

| Column Name | Type | Configuration |
|---|---|---|
| A1Title | Choice | Choices: `Mr`, `Mrs`, `Miss`, `Ms`, `Dr`, `Other` |
| A1FirstName | Single line of text | |
| A1LastName | Single line of text | |
| A1DateOfBirth | Date and Time | Date only |
| A1Nationality | Single line of text | |
| A1NiNumber | Single line of text | National Insurance number |
| A1MaritalStatus | Choice | Choices: `Single`, `Married`, `Civil Partner`, `Separated`, `Divorced`, `Widowed` |
| A1ResidentialStatus | Choice | Choices: `Owner Occupier`, `Renting`, `Living with Family`, `Other` |
| A1CurrentAddressLine1 | Single line of text | |
| A1CurrentAddressLine2 | Single line of text | |
| A1CurrentAddressTown | Single line of text | |
| A1CurrentAddressPostcode | Single line of text | |
| A1TimeAtAddressYears | Number | 0 decimal places |
| A1TimeAtAddressMonths | Number | 0 decimal places |
| A1PreviousAddressLine1 | Single line of text | If < 3 years at current address |
| A1PreviousAddressPostcode | Single line of text | |
| A1MobilePhone | Single line of text | |
| A1Email | Single line of text | |

### Step 4 — Applicant 2 Personal Details

| Column Name | Type | Configuration |
|---|---|---|
| A2Title | Choice | Choices: `Mr`, `Mrs`, `Miss`, `Ms`, `Dr`, `Other` |
| A2FirstName | Single line of text | |
| A2LastName | Single line of text | |
| A2DateOfBirth | Date and Time | Date only |
| A2Nationality | Single line of text | |
| A2NiNumber | Single line of text | |
| A2MaritalStatus | Choice | Choices: `Single`, `Married`, `Civil Partner`, `Separated`, `Divorced`, `Widowed` |
| A2ResidentialStatus | Choice | Choices: `Owner Occupier`, `Renting`, `Living with Family`, `Other` |
| A2CurrentAddressLine1 | Single line of text | |
| A2CurrentAddressPostcode | Single line of text | |
| A2MobilePhone | Single line of text | |
| A2Email | Single line of text | |

### Step 5 — Income & Employment (Applicant 1)

| Column Name | Type | Configuration |
|---|---|---|
| A1EmploymentStatus | Choice | Choices: `Employed`, `Self-Employed`, `Director`, `Retired`, `Not Employed`, `Other` |
| A1EmployerName | Single line of text | |
| A1JobTitle | Single line of text | |
| A1TimeEmployedYears | Number | 0 decimal places |
| A1GrossAnnualSalary | Currency | |
| A1OtherIncome | Currency | |
| A1OtherIncomeSource | Single line of text | |
| A1SelfEmpTradingAs | Single line of text | If self-employed |
| A1SelfEmpYearsTrading | Number | 0 decimal places |
| A1SelfEmpNetProfit1 | Currency | Year 1 (most recent) |
| A1SelfEmpNetProfit2 | Currency | Year 2 |
| A1SelfEmpNetProfit3 | Currency | Year 3 |

### Step 6 — Income & Employment (Applicant 2)

| Column Name | Type | Configuration |
|---|---|---|
| A2EmploymentStatus | Choice | Same choices as A1EmploymentStatus |
| A2EmployerName | Single line of text | |
| A2JobTitle | Single line of text | |
| A2TimeEmployedYears | Number | 0 decimal places |
| A2GrossAnnualSalary | Currency | |
| A2OtherIncome | Currency | |
| A2OtherIncomeSource | Single line of text | |
| A2SelfEmpTradingAs | Single line of text | |
| A2SelfEmpYearsTrading | Number | 0 decimal places |
| A2SelfEmpNetProfit1 | Currency | |
| A2SelfEmpNetProfit2 | Currency | |
| A2SelfEmpNetProfit3 | Currency | |

### Step 7 — Property Details

| Column Name | Type | Configuration |
|---|---|---|
| PropertyAddress | Multiple lines of text | Plain text |
| Postcode | Single line of text | |
| PropertyType | Choice | Choices: `Residential`, `HMO`, `MUB`, `Commercial`, `Semi-Commercial`, `Land`, `Other` |
| Tenure | Choice | Choices: `Freehold`, `Leasehold`, `Commonhold` |
| LeaseYearsRemaining | Number | 0 decimal places |
| YearBuilt | Number | 0 decimal places |
| Bedrooms | Number | 0 decimal places |
| Bathrooms | Number | 0 decimal places |
| IsListed | Yes/No | Default: No |
| IsNonStandard | Yes/No | Default: No |
| CurrentUse | Single line of text | |
| IntendedUse | Single line of text | |
| PurchaseOrRefinance | Choice | Choices: `Purchase`, `Refinance` |
| EstimatedValue | Currency | |
| PurchasePrice | Currency | |
| OutstandingMortgageBalance | Currency | |
| ExistingLender | Single line of text | |
| MonthlyRentalIncome | Currency | |
| IsHMO | Yes/No | Default: No |
| NumberOfUnits | Number | 0 decimal places |
| EpcRating | Choice | Choices: `A`, `B`, `C`, `D`, `E`, `F`, `G` |

### Step 8 — Portfolio Details

| Column Name | Type | Configuration |
|---|---|---|
| IsPortfolioLandlord | Yes/No | Default: No |
| TotalPortfolioProperties | Number | 0 decimal places |
| TotalPortfolioValue | Currency | |
| TotalPortfolioMortgageBalance | Currency | |
| TotalPortfolioRentalIncome | Currency | |
| PortfolioDetails | Multiple lines of text | Plain text |

### Step 9 — Company / SPV

| Column Name | Type | Configuration |
|---|---|---|
| IsCompanyApplication | Yes/No | Default: No |
| CompanyName | Single line of text | |
| CompanyNumber | Single line of text | |
| CompanyType | Choice | Choices: `SPV`, `Trading Company`, `LLP`, `Partnership`, `Other` |
| IncorporationDate | Date and Time | Date only |
| CompanyAddress | Multiple lines of text | Plain text |
| NatureOfBusiness | Single line of text | |
| Turnover | Currency | |
| NetProfit | Currency | |
| DirectorsNames | Multiple lines of text | Plain text |

### Step 10 — Loan Requirements

| Column Name | Type | Configuration |
|---|---|---|
| LoanAmountRequired | Currency | |
| LoanPurpose | Single line of text | |
| RepaymentType | Choice | Choices: `Interest Only`, `Capital & Interest` |
| PreferredTermMonths | Number | 0 decimal places |
| MaxMonthlyPayment | Currency | |
| HasExistingFinance | Yes/No | Default: No |
| ExistingFinanceDetails | Multiple lines of text | Plain text |
| AdverseCredit | Yes/No | Default: No |
| AdverseCreditDetails | Multiple lines of text | Plain text |

### Step 11 — Bridging / Development

| Column Name | Type | Configuration |
|---|---|---|
| ExitStrategy | Choice | Choices: `Refinance`, `Sale`, `Other` |
| ExitStrategyDetails | Multiple lines of text | Plain text |
| AuctionDate | Date and Time | Date only |
| CompletionDeadlineDate | Date and Time | Date only |
| WorksRequired | Yes/No | Default: No |
| WorksDescription | Multiple lines of text | Plain text |
| WorksBudget | Currency | |
| BuildCost | Currency | |
| EstimatedGDV | Currency | |
| ProfitOnCosts | Number | Percentage, 2 decimal places |
| DrawdownSchedule | Multiple lines of text | Plain text |
| PlanningPermissionStatus | Choice | Choices: `Not Required`, `Permitted Development`, `Applied`, `Granted`, `Refused` |

### Step 12 — Additional Info & Status

| Column Name | Type | Configuration |
|---|---|---|
| Solicitor | Single line of text | |
| SolicitorPhone | Single line of text | |
| SolicitorEmail | Single line of text | |
| PowerOfAttorney | Yes/No | Default: No |
| SpecialCircumstances | Multiple lines of text | Plain text |
| CustomerDeclarationAgreed | Yes/No | Default: No |
| CustomerDeclarationDate | Date and Time | Date only |
| LastUpdatedByCustomer | Date and Time | Date and time — updated by Flow 15 |
| Status | Choice | Choices: `Not Started`, `In Progress`, `Submitted`, `Reviewed`. Default: `Not Started` |

### Step 13 — Verify

- [ ] List appears in Site Contents as "Fact-Find Responses"
- [ ] All sections are present (count: ~90+ columns)
- [ ] Choice columns have correct values
- [ ] Status field defaults to "Not Started"
- [ ] A test item can be created and all fields saved

---

## Importable Content

See [`imports/factfind-columns.csv`](imports/factfind-columns.csv) for the complete column reference.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Too many columns to add manually | Use PnP PowerShell with the CSV reference, or add columns in batches per section |
| Choice column won't save multiple values | Only multi-select Choice columns need "Allow multiple selections" — most Fact-Find choices are single-select |
