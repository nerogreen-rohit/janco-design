# Task P2-03: Add LEAD Counter Row

**Phase:** 2 — Lead Management  
**Story:** As a developer, I need to add the LEAD counter row to the Counters list so that LeadIDs can be auto-generated  
**Priority:** 🔴 Critical  
**Estimated effort:** 5 minutes  
**Depends on:** Counters list created in Phase 1 (P1-04)

---

## Step-by-Step Guide

### Step 1 — Open the Counters List

1. Go to **Site Contents** → **Counters**
2. You should see the 6 existing rows from Phase 1

### Step 2 — Add the LEAD Row

Click **+ New** and enter:

| Title | CurrentValue | Prefix | PaddingLength |
|---|---|---|---|
| LEAD | 0 | LEAD- | 4 |

### Step 3 — Verify

- [ ] Counters list now has 7 rows
- [ ] LEAD row has CurrentValue = 0
- [ ] Prefix = `LEAD-`
- [ ] PaddingLength = 4

This will generate LeadIDs like `LEAD-0001`, `LEAD-0002`, etc.

---

## Importable Content

The row can also be added via PnP PowerShell:

```powershell
Add-PnPListItem -List "Counters" -Values @{
    "Title"         = "LEAD"
    "CurrentValue"  = 0
    "Prefix"        = "LEAD-"
    "PaddingLength" = 4
}
```
