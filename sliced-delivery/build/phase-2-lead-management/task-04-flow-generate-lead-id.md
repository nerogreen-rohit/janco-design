# Task P2-04: Flow 1 — Generate Lead ID

**Phase:** 2 — Lead Management  
**Story:** As a system, when a new lead is created I need to generate a unique LeadID  
**Priority:** 🔴 Critical  
**Estimated effort:** 30–45 minutes  
**Depends on:** Leads list (P2-01), LEAD counter row (P2-03)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 1 — Generate Lead ID`
3. Trigger: **When an item is created** (SharePoint)
4. Site: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Leads`

### Step 2 — Read the Counter

1. Add **Get items** (SharePoint):
   - List: `Counters`
   - Filter Query: `Title eq 'LEAD'`
   - Top Count: 1

### Step 3 — Increment the Counter

1. Add **Compose** — `NewValue`: `add(first(outputs('Get_items')?['body/value'])?['CurrentValue'], 1)`
2. Add **Update item** (SharePoint):
   - List: `Counters`
   - Id: `first(outputs('Get_items')?['body/value'])?['ID']`
   - Title: `LEAD`
   - CurrentValue: `@{outputs('NewValue')}`
   - Use **If-Match header** with `@odata.etag` for optimistic concurrency

### Step 4 — Generate the LeadID String

1. Add **Compose** — `LeadID`:
```
concat(
  'LEAD-',
  substring(
    concat('0000', string(outputs('NewValue'))),
    sub(length(concat('0000', string(outputs('NewValue')))), 4),
    4
  )
)
```

This produces `LEAD-0001`, `LEAD-0002`, etc.

### Step 5 — Update the Lead Record

1. Add **Update item** (SharePoint):
   - List: `Leads`
   - Id: `triggerOutputs()?['body/ID']`
   - Title: `@{outputs('LeadID')}`
   - LeadID: `@{outputs('LeadID')}`

### Step 6 — Test

1. Create a new Lead in the Leads list
2. Wait up to 30 seconds
3. Verify:
   - [ ] LeadID = `LEAD-0001`
   - [ ] Title = `LEAD-0001`
   - [ ] Counters list LEAD row CurrentValue = 1
4. Create a second lead → verify LEAD-0002

---

## Notes

This flow follows the exact same pattern as Flow 3 (Generate Case ID) from Phase 1. The only differences are the trigger list (Leads vs Cases) and the counter row (LEAD vs product-specific).
