# Task P1-04: Create the Counters List

**Phase:** 1 — Fact-Find  
**Story:** As a developer, I need to create the Counters list with all product type rows so that CaseIDs can be auto-generated  
**Priority:** 🔴 Critical  
**Estimated effort:** 15 minutes  
**Depends on:** SharePoint site provisioned

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Go to **Site Contents** → **+ New** → **List** → **Blank list**
2. Name: `Counters`
3. Description: `Sequential ID generation counters for cases and leads`
4. Click **Create**

### Step 2 — Add Columns

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CurrentValue | Number | Yes | 0 decimal places, default: 0 |
| Prefix | Single line of text | Yes | |
| PaddingLength | Number | Yes | 0 decimal places, default: 4 |

> The `Title` column (auto-created) stores the counter key (e.g. "BTL", "BRIDGE").

### Step 3 — Enter Counter Rows

Add the following 6 rows:

| Title | CurrentValue | Prefix | PaddingLength |
|---|---|---|---|
| BTL | 0 | BTL-2026- | 4 |
| BRIDGE | 0 | BRIDGE-2026- | 4 |
| COMMERCIAL | 0 | COMM-2026- | 4 |
| DEV | 0 | DEV-2026- | 4 |
| REFURB | 0 | REFURB-2026- | 4 |
| AUCTION | 0 | AUCT-2026- | 4 |

> **Note:** The LEAD counter row is added in Phase 2.

### Step 4 — Verify

- [ ] List appears in Site Contents as "Counters"
- [ ] 6 rows present (BTL, BRIDGE, COMMERCIAL, DEV, REFURB, AUCTION)
- [ ] All CurrentValue fields are 0
- [ ] Prefix values include year and trailing hyphen (e.g. `BTL-2026-`)
- [ ] PaddingLength is 4 for all rows

---

## Importable Content

**CSV file:** [`imports/counters.csv`](imports/counters.csv)

**PnP PowerShell:**

```powershell
$csv = Import-Csv -Path ".\imports\counters.csv"
foreach ($row in $csv) {
    Add-PnPListItem -List "Counters" -Values @{
        "Title"         = $row.Title
        "CurrentValue"  = [int]$row.CurrentValue
        "Prefix"        = $row.Prefix
        "PaddingLength" = [int]$row.PaddingLength
    }
}
```

---

## How Flow 3 Uses This List

When a Case is created with `ProductType = "BTL"`:

1. Flow reads the Counters list, filtering by `Title = "BTL"`
2. Reads `CurrentValue` (e.g. 0)
3. Increments to 1
4. Updates the Counters item: `CurrentValue = 1`
5. Generates CaseID: `BTL-2026-` + `0001` (padded to 4 digits) = `BTL-2026-0001`
6. Sets `Title` and `CaseID` on the Case record

> **Concurrency:** Flow uses optimistic locking (check version before update) to prevent duplicate IDs when two cases are created simultaneously.
