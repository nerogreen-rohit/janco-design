# Task P5-01: Create the Commissions SharePoint List

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a developer, I need to create the Commissions SharePoint list so that commission records can be stored per case  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** Cases list (P1-01), Referrers list (P2-01)

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Open your SharePoint site: `https://[tenant].sharepoint.com/sites/lending`
2. Click **⚙️ Settings** → **Site contents**
3. Click **+ New** → **List** → **Blank list**
4. Name: `Commissions`
5. Description: `Commission records for proc fees, advisory fees, introducer fees, and referral fees per case`
6. Click **Create**

### Step 2 — Add Columns

Add each column in order. Use **+ Add column** or **List Settings → Create column**.

#### Identity Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| Title | Single line of text | No | Auto-set to `{CaseID}-{CommissionType}` by flow |
| CaseID | Single line of text | Yes | FK to Cases list |
| CaseName | Single line of text | No | Denormalized — e.g. "John & Jane Doe" |
| LenderName | Single line of text | No | Denormalized from Case or Lenders list |
| ProductType | Single line of text | No | Denormalized from Case record |

#### Commission Detail Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CommissionType | Choice | Yes | Choices: `Proc Fee`, `Advisory Fee (AdvFee)`, `Introducer Fee`, `Referral Fee` |
| ExpectedAmount | Currency | No | Expected commission value |
| ActualAmount | Currency | No | Actual amount received |
| IsReceived | Yes/No | No | Default: No |
| ReceivedDate | Date and Time | No | Date only — when payment was received |
| InvoiceNumber | Single line of text | No | Reference for invoice tracking |
| PaymentMethod | Choice | No | Choices: `BACS`, `Cheque`, `Deducted at Completion`, `Other` |

#### Referral & Split Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| SplitPercentage | Number | No | If shared — percentage to this party (0–100) |
| ReferrerID | Lookup | No | Lookup list: `Referrers`, Lookup column: `Title`. Used for referral/introducer fees |

#### Additional Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| Notes | Multiple lines of text | No | Plain text |
| TaxYear | Single line of text | No | e.g. "2025/26" — for Commission Dashboard filtering |

### Step 3 — Rename the Title Column

1. Go to **List Settings** → click **Title** column
2. Set **Require that this column contains information** = No
3. Title will be auto-set to `{CaseID}-{CommissionType}` by flows

### Step 4 — Create Views

#### View 1: All Commissions (Default)

1. Go to **List Settings** → **Views** → **Create view**
2. Name: `All Commissions`
3. Columns to show: `Title`, `CaseID`, `CaseName`, `CommissionType`, `ExpectedAmount`, `ActualAmount`, `IsReceived`, `ReceivedDate`
4. Sort by: `Modified` (descending)

#### View 2: Outstanding Commissions

1. Create another view
2. Name: `Outstanding`
3. Filter: `IsReceived is equal to No`
4. Columns: `CaseID`, `CaseName`, `CommissionType`, `ExpectedAmount`, `LenderName`
5. Sort by: `Created` (ascending) — oldest outstanding first

#### View 3: By Tax Year

1. Create another view
2. Name: `By Tax Year`
3. Group by: `TaxYear` (ascending)
4. Columns: `CaseID`, `CaseName`, `CommissionType`, `ExpectedAmount`, `ActualAmount`, `IsReceived`
5. Sort by: `CommissionType` (ascending)

### Step 5 — Verify

- [ ] List appears in Site Contents as "Commissions"
- [ ] All 16 columns are present (Title + 15 custom)
- [ ] CommissionType choice values are exactly: `Proc Fee`, `Advisory Fee (AdvFee)`, `Introducer Fee`, `Referral Fee`
- [ ] PaymentMethod choice values are exactly: `BACS`, `Cheque`, `Deducted at Completion`, `Other`
- [ ] ReferrerID lookup points to Referrers list
- [ ] All three views are working (All Commissions, Outstanding, By Tax Year)
- [ ] IsReceived defaults to No

---

## Importable Content

See [`imports/commissions-list-columns.csv`](imports/commissions-list-columns.csv) for the complete column definition that can be referenced during creation.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| ReferrerID lookup not available | Create the Referrers list first (Phase 2, P2-01), then return to add this column |
| CommissionType choice values not matching | Ensure "Advisory Fee (AdvFee)" is entered exactly — including the parentheses. Never use alternatives |
| Currency columns show wrong symbol | Set the currency locale in column settings to GBP (£) |
| TaxYear format inconsistent | Instruct users to use "YYYY/YY" format (e.g. "2025/26"); consider adding column validation |
