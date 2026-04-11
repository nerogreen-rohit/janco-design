# Task P3-02: Create the Document Types List

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a developer, I need to create the Document Types configuration list so that document categories and requirements are centrally managed  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** SharePoint site provisioned

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Open your SharePoint site: `https://[tenant].sharepoint.com/sites/lending`
2. Click **⚙️ Settings** → **Site contents**
3. Click **+ New** → **List** → **Blank list**
4. Name: `Document Types`
5. Description: `Reference/configuration list defining document categories required for lending cases`
6. Click **Create**

### Step 2 — Add Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| Category | Choice | Yes | Choices: `Identity`, `Income`, `Property`, `Legal`, `Lender`, `Other` |
| ApplicableTo | Choice (multi-select) | No | Choices: `All Products`, `BTL`, `Bridging`, `Commercial`, `Dev Finance`, `Refurb`, `Auction`. Allow multiple selections |
| Description | Multiple lines of text | No | Plain text. Shown to customer when requesting documents |
| IsRequired | Yes/No | No | Default: Yes |
| SortOrder | Number | No | 0 decimal places |
| IsActive | Yes/No | No | Default: Yes |

### Step 3 — Rename the Title Column

1. Go to **List Settings** → click **Title** column
2. Keep display name as `Title`
3. Set **Require that this column contains information** = Yes
4. This holds the document type name (e.g. "Passport")

### Step 4 — Populate Initial Data

Add the following 25 document types. Use the import CSV at [`imports/document-types.csv`](imports/document-types.csv) or enter manually:

#### Identity Documents

| Title | Category | ApplicableTo | IsRequired | SortOrder |
|---|---|---|---|---|
| Passport | Identity | All Products | Yes | 1 |
| Driving Licence | Identity | All Products | Yes | 2 |
| Utility Bill (3 months) | Identity | All Products | Yes | 3 |
| Council Tax Bill | Identity | All Products | No | 4 |

#### Income Documents

| Title | Category | ApplicableTo | IsRequired | SortOrder |
|---|---|---|---|---|
| Last 3 Months Payslips | Income | All Products | Yes | 5 |
| Last 2 Years SA302 + Tax Overview | Income | All Products | Yes | 6 |
| Last 2 Years Company Accounts | Income | Commercial, Dev Finance | Yes | 7 |
| 6 Months Business Bank Statements | Income | Commercial, Dev Finance | Yes | 8 |
| 6 Months Personal Bank Statements | Income | All Products | Yes | 9 |

#### Property Documents

| Title | Category | ApplicableTo | IsRequired | SortOrder |
|---|---|---|---|---|
| Mortgage Statement | Property | BTL, Refurb | Yes | 10 |
| Buildings Insurance | Property | All Products | Yes | 11 |
| Tenancy Agreement | Property | BTL, Refurb | Yes | 12 |
| EPC Certificate | Property | BTL, Refurb | Yes | 13 |
| Title Register (OS1) | Property | All Products | No | 14 |

#### Legal Documents

| Title | Category | ApplicableTo | IsRequired | SortOrder |
|---|---|---|---|---|
| Memorandum & Articles | Legal | Commercial, Dev Finance | Yes | 15 |
| Certificate of Incorporation | Legal | Commercial, Dev Finance | Yes | 16 |
| Schedule of Works | Legal | Dev Finance, Refurb | Yes | 17 |

#### Lender Documents

| Title | Category | ApplicableTo | IsRequired | SortOrder |
|---|---|---|---|---|
| ESIS/KFI | Lender | All Products | No | 18 |
| DIP Confirmation | Lender | All Products | No | 19 |
| Formal Loan Offer | Lender | All Products | Yes | 20 |
| Valuation Report | Lender | All Products | No | 21 |

#### Other Documents

| Title | Category | ApplicableTo | IsRequired | SortOrder |
|---|---|---|---|---|
| Power of Attorney | Other | All Products | No | 22 |
| Proof of Deposit | Other | BTL, Bridging, Commercial | Yes | 23 |
| Auction Legal Pack | Other | Auction | Yes | 24 |
| Exit Strategy Evidence | Other | Bridging, Dev Finance | Yes | 25 |

### Step 5 — Create the Default View

1. Go to **List Settings** → **Views** → **Create view**
2. Name: `By Category`
3. Columns to show: `Title`, `Category`, `ApplicableTo`, `IsRequired`, `SortOrder`, `IsActive`
4. Sort by: `SortOrder` (ascending)
5. Group by: `Category`

### Step 6 — Verify

- [ ] List appears in Site Contents as "Document Types"
- [ ] All 6 columns are present with correct types
- [ ] Category choice values are correct (6 options)
- [ ] ApplicableTo allows multiple selections
- [ ] All 25 document types are populated
- [ ] SortOrder values are sequential 1–25
- [ ] IsActive defaults to Yes for all items
- [ ] Default view groups by Category

---

## Importable Content

See [`imports/document-types.csv`](imports/document-types.csv) for the complete dataset.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| ApplicableTo not allowing multi-select | Ensure the column is configured as Choice with "Allow multiple selections" enabled |
| Document types out of order | Verify SortOrder values; ensure the view sorts by SortOrder ascending |
| Missing categories | Verify all 6 Category choice values are defined before adding items |
