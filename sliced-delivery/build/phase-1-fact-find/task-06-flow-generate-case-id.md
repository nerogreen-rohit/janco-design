# Task P1-06: Flow 3 — Generate Case ID

**Phase:** 1 — Fact-Find  
**Story:** As a system, when a new case is created I need to generate a unique CaseID so that every case has a trackable reference  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** Cases list (P1-01), Counters list (P1-04)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 3 — Generate Case ID`
3. Trigger: **When an item is created** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Specialist Lending Cases`

### Step 2 — Initialize Variables

Add these actions after the trigger:

| Action | Variable Name | Type | Value |
|---|---|---|---|
| Initialize variable | varProductType | String | `triggerOutputs()?['body/ProductType/Value']` |
| Initialize variable | varCounterTitle | String | (set in next step) |

### Step 3 — Map ProductType to Counter Title

Add a **Switch** action on `varProductType`:

| ProductType value | Set varCounterTitle to |
|---|---|
| BTL | `BTL` |
| Bridging | `BRIDGE` |
| Commercial | `COMMERCIAL` |
| Dev Finance | `DEV` |
| Refurb | `REFURB` |
| Auction | `AUCTION` |

### Step 4 — Read the Counter

1. Add **Get items** (SharePoint):
   - List: `Counters`
   - Filter Query: `Title eq '@{variables('varCounterTitle')}'`
   - Top Count: 1

### Step 5 — Increment the Counter (Optimistic Locking)

1. Add **Compose** — `NewValue`: `add(first(outputs('Get_items')?['body/value'])?['CurrentValue'], 1)`
2. Add **Update item** (SharePoint):
   - List: `Counters`
   - Id: `first(outputs('Get_items')?['body/value'])?['ID']`
   - Title: `@{variables('varCounterTitle')}`
   - CurrentValue: `@{outputs('NewValue')}`
   - Use **If-Match header** with `@odata.etag` from the Get items result for optimistic concurrency

### Step 6 — Generate the CaseID String

1. Add **Compose** — `Prefix`: `first(outputs('Get_items')?['body/value'])?['Prefix']`
2. Add **Compose** — `PaddedNumber`:

```
concat(
  outputs('Prefix'),
  substring(
    concat('0000', string(outputs('NewValue'))),
    sub(length(concat('0000', string(outputs('NewValue')))), 4),
    4
  )
)
```

This produces e.g. `BTL-2026-0001`.

### Step 7 — Update the Case Record

1. Add **Update item** (SharePoint):
   - List: `Specialist Lending Cases`
   - Id: `triggerOutputs()?['body/ID']`
   - Title: `@{outputs('PaddedNumber')}`
   - CaseID: `@{outputs('PaddedNumber')}`

### Step 8 — Error Handling

1. Add a **Scope** around Steps 4–7
2. Add a parallel branch after the Scope for **Configure run after** → "has failed"
3. In the failure branch:
   - Wait 5 seconds (to avoid race condition)
   - Re-read the counter (in case another flow incremented it)
   - Retry the increment + update
4. Optionally send a Teams notification to admin if retry also fails

### Step 9 — Test

1. Create a new Case item in the Cases list with ProductType = "BTL"
2. Wait up to 30 seconds
3. Verify:
   - [ ] CaseID field is set (e.g. `BTL-2026-0001`)
   - [ ] Title field matches CaseID
   - [ ] Counters list BTL row shows CurrentValue = 1
4. Create a second BTL case
5. Verify CaseID = `BTL-2026-0002` and counter = 2
6. Create a Bridging case and verify `BRIDGE-2026-0001`

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| CaseID not generated | Check flow run history for errors; verify trigger list name matches |
| Duplicate CaseIDs | Ensure optimistic locking (If-Match header) is configured on Counter update |
| Wrong prefix | Verify Counters list Prefix values include trailing hyphen |
| Flow fails with 412 Precondition Failed | Optimistic locking conflict — the retry logic in Step 8 should handle this |
