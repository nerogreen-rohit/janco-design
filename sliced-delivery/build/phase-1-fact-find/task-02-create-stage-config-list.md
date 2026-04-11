# Task P1-02: Create the Stage Configuration List

**Phase:** 1 — Fact-Find  
**Story:** As a developer, I need to create the Stage Configuration list with stages 1–3 so that case lifecycle tracking is ready  
**Priority:** 🔴 Critical  
**Estimated effort:** 30 minutes  
**Depends on:** SharePoint site provisioned

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Go to **Site Contents** → **+ New** → **List** → **Blank list**
2. Name: `Stage Configuration`
3. Description: `Single source of truth for all lifecycle stages`
4. Click **Create**

### Step 2 — Add Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| ShortName | Single line of text | Yes | Short code e.g. "FACT" |
| CustomerFriendlyName | Single line of text | Yes | Shown to customer on portal |
| StageNumber | Number | Yes | 0 decimal places, min 1, max 14 |
| ProductType | Choice (multi-select) | No | Choices: `All`, `BTL`, `Bridging`, `Commercial`, `Dev Finance`, `Refurb`, `Auction`. Allow multiple selections |
| CanSkip | Yes/No | Yes | Default: No |
| PrerequisiteFields | Multiple lines of text | No | Plain text. Comma-separated list of Case fields |
| DefaultSLADays | Number | Yes | 0 decimal places |
| VisibleToCustomer | Yes/No | Yes | Default: Yes |
| SortOrder | Number | No | 0 decimal places |
| IsActive | Yes/No | Yes | Default: Yes |
| Notes | Multiple lines of text | No | Plain text |

> **Note:** The `Title` column (auto-created) is used for the full stage name (e.g. "Fact-Find").

### Step 3 — Enter Phase 1 Stage Data (Stages 1–3)

| Title | ShortName | CustomerFriendlyName | StageNumber | VisibleToCustomer | DefaultSLADays | CanSkip | ProductType | SortOrder | IsActive |
|---|---|---|---|---|---|---|---|---|---|
| Customer Enquiry | ENQ | Application Received | 1 | Yes | 1 | No | All | 1 | Yes |
| Fact-Find | FACT | Information Gathering | 2 | Yes | 5 | No | All | 2 | Yes |
| Terms of Business & GDPR | TOB | Terms Agreed | 3 | Yes | 2 | No | All | 3 | Yes |

> **Tip:** You can import all 14 stages now using the CSV in [`imports/stage-configuration.csv`](imports/stage-configuration.csv). Only stages 1–3 are functionally required in Phase 1, but adding all 14 does no harm.

### Step 4 — Verify

- [ ] List appears in Site Contents as "Stage Configuration"
- [ ] 3 items (stages 1–3) are present
- [ ] Each item has correct StageNumber, ShortName, and CustomerFriendlyName
- [ ] VisibleToCustomer is Yes for all 3 stages
- [ ] CanSkip is No for all 3 stages

---

## Importable Content

**CSV file:** [`imports/stage-configuration.csv`](imports/stage-configuration.csv) — contains all 14 stages (use rows 1–3 for Phase 1, remaining added in Phase 3).

**PnP PowerShell:**

```powershell
$csv = Import-Csv -Path ".\imports\stage-configuration.csv"
foreach ($row in $csv) {
    Add-PnPListItem -List "Stage Configuration" -Values @{
        "Title"               = $row.Title
        "ShortName"           = $row.ShortName
        "CustomerFriendlyName"= $row.CustomerFriendlyName
        "StageNumber"         = [int]$row.StageNumber
        "VisibleToCustomer"   = ($row.VisibleToCustomer -eq "Yes")
        "DefaultSLADays"      = [int]$row.DefaultSLADays
        "CanSkip"             = ($row.CanSkip -eq "Yes")
        "ProductType"         = $row.ProductType
        "SortOrder"           = [int]$row.SortOrder
        "IsActive"            = ($row.IsActive -eq "Yes")
    }
}
```
