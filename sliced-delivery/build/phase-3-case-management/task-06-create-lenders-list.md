# Task P3-06: Create the Lenders List

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a developer, I need to create the Lenders list so that lender contact details and submission emails are centrally managed  
**Priority:** 🟡 High  
**Estimated effort:** 1 hour  
**Depends on:** SharePoint site provisioned

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Open your SharePoint site: `https://[tenant].sharepoint.com/sites/lending`
2. Click **⚙️ Settings** → **Site contents**
3. Click **+ New** → **List** → **Blank list**
4. Name: `Lenders`
5. Description: `Lender panel — contact details, product offerings, and submission emails for direct lender communication`
6. Click **Create**

### Step 2 — Add Columns

Add each column in order. Use **+ Add column** or **List Settings → Create column**.

#### Classification Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| LenderType | Choice | Yes | Choices: `High Street`, `Specialist`, `Challenger`, `Private`, `Bridging`, `Development` |
| ProductsOffered | Choice (multi-select) | No | Choices: `BTL`, `Bridging`, `Commercial`, `Dev Finance`, `Refurb`, `Auction`. Allow multiple selections |

#### Contact Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| ContactName | Single line of text | No | BDM name |
| ContactEmail | Single line of text | No | BDM email address |
| ContactPhone | Single line of text | No | BDM phone number |
| SubmissionEmail | Single line of text | No | **Used by Flow 12 (Email Docs to Lender)** — this is where application packs are sent |

#### Lending Parameters

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| MaxLTV | Number | No | 0 decimal places. Maximum loan-to-value percentage |
| MinLoanAmount | Currency | No | Minimum loan amount offered |
| MaxLoanAmount | Currency | No | Maximum loan amount offered |
| TypicalProcFeePercent | Number | No | 2 decimal places. Typical procuration fee percentage |

#### Other Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| Notes | Multiple lines of text | No | Plain text — general notes about the lender |
| IsActive | Yes/No | Yes | Default: Yes |
| WebsiteURL | Hyperlink | No | Lender website |

### Step 3 — Configure the Title Column

1. Go to **List Settings** → click **Title** column
2. Keep display name as `Title`
3. Set **Require that this column contains information** = Yes
4. This holds the lender name (e.g. "The Mortgage Lender")

### Step 4 — Create the Default View

1. Go to **List Settings** → **Views** → **Create view**
2. Name: `Active Lenders`
3. Filter: `IsActive is equal to Yes`
4. Columns: `Title`, `LenderType`, `ProductsOffered`, `ContactName`, `SubmissionEmail`, `MaxLTV`, `IsActive`
5. Sort by: `Title` (ascending)

### Step 5 — Populate Initial Data

Add your lender panel. Minimum suggested structure:

| Title | LenderType | ProductsOffered | SubmissionEmail |
|---|---|---|---|
| The Mortgage Lender | Specialist | BTL | submissions@tml.example.com |
| Paragon Banking | Specialist | BTL, Commercial | submit@paragon.example.com |
| Shawbrook Bank | Challenger | BTL, Bridging, Commercial | apps@shawbrook.example.com |
| Precise Mortgages | Specialist | BTL, Bridging | newbiz@precise.example.com |
| Together Money | Specialist | BTL, Bridging, Refurb | apply@together.example.com |

> **Note:** Replace example email addresses with real submission addresses before go-live. Populate the full lender panel as part of UAT.

### Step 6 — Verify

- [ ] List appears in Site Contents as "Lenders"
- [ ] All columns are present with correct types
- [ ] LenderType has 6 choice options
- [ ] ProductsOffered allows multiple selections
- [ ] IsActive defaults to Yes
- [ ] SubmissionEmail column is present (used by Flow 12)
- [ ] Default view filters to active lenders only

---

## Importable Content

See [`imports/lenders-list-columns.csv`](imports/lenders-list-columns.csv) for the complete column definition.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| ProductsOffered not allowing multi-select | Ensure the column is configured as Choice with "Allow multiple selections" enabled |
| Currency columns showing wrong symbol | Set the locale to GBP (£) in column settings |
| SubmissionEmail column confused with ContactEmail | SubmissionEmail is the formal application inbox; ContactEmail is the BDM's personal email |
