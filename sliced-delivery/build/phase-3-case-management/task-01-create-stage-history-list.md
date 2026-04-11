# Task P3-01: Create the Case Stage History List

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a developer, I need to create the Case Stage History list so that every stage change is recorded as an immutable audit log  
**Priority:** 🔴 Critical  
**Estimated effort:** 1 hour  
**Depends on:** Cases list (P1-01), Stage Configuration list (P1-02)

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Open your SharePoint site: `https://[tenant].sharepoint.com/sites/lending`
2. Click **⚙️ Settings** → **Site contents**
3. Click **+ New** → **List** → **Blank list**
4. Name: `Case Stage History`
5. Description: `Immutable audit log of every stage transition per case. Created by Flow 6 only — never edited by staff.`
6. Click **Create**

### Step 2 — Add Columns

Add each column in order. Use **+ Add column** or **List Settings → Create column**.

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CaseID | Single line of text | Yes | FK to Cases list |
| CaseName | Single line of text | No | Denormalized case display name |
| StageNumber | Number | Yes | 0 decimal places |
| StageName | Single line of text | Yes | **Text snapshot — NOT a lookup** |
| PreviousStageNumber | Number | No | 0 decimal places |
| PreviousStageName | Single line of text | No | Text snapshot |
| ChangedBy | Person or Group | Yes | Allow selection of people only |
| ChangedDate | Date and Time | Yes | Include time |
| TimeInPreviousStageHours | Number | No | 2 decimal places |
| Notes | Multiple lines of text | No | Plain text |
| IsSkipped | Yes/No | No | Default: No |

> **Key design decision:** StageName and PreviousStageName are stored as **plain text snapshots**, not lookups to Stage Configuration. This ensures audit integrity — even if a stage is renamed later, historical records remain accurate.

### Step 3 — Rename the Title Column

1. Go to **List Settings** → click **Title** column
2. Set **Require that this column contains information** = No
3. Title will be auto-set to `{CaseID}-Stage{N}` by Flow 6

### Step 4 — Create the Default View

1. Go to **List Settings** → **Views** → **Create view**
2. Name: `By Case`
3. Columns to show: `Title`, `CaseID`, `StageNumber`, `StageName`, `ChangedBy`, `ChangedDate`, `TimeInPreviousStageHours`, `IsSkipped`
4. Sort by: `ChangedDate` (descending)
5. Group by: `CaseID`

### Step 5 — Remove Edit Permissions for Staff

This list is **read-only for staff**. Only Flow 6 creates items.

1. Go to **List Settings** → **Permissions for this list**
2. Click **Stop Inheriting Permissions** (this breaks inheritance from the site)
3. Select the **Members** group (e.g. "Lending Site Members") → click **Edit User Permissions**
4. Change permission level from **Edit** (or Contribute) to **Read** only
5. Keep the **Owners** group and the **flow service account / app registration** at Edit or Full Control so Flow 6 can write to this list

> **Why break inheritance?** SharePoint list Advanced Settings does not provide a "Create and Edit access = None" option. Breaking permission inheritance and removing Contribute/Edit from the Members group is the correct way to prevent staff from directly creating or editing Stage History items.

> **Fallback:** If breaking inheritance is not feasible (e.g. governance constraints), ensure the Staff App Timeline tab uses `DisplayMode.View` on all controls and does not expose any edit or new-item form for this list. This is a UI-level safeguard, not a list-level permission control.

### Step 6 — Verify

- [ ] List appears in Site Contents as "Case Stage History"
- [ ] All columns are present with correct types
- [ ] StageName and PreviousStageName are **Single line of text** (NOT lookup)
- [ ] IsSkipped defaults to No
- [ ] Title column is not required
- [ ] Default view groups by CaseID

---

## Importable Content

See [`imports/stage-history-columns.csv`](imports/stage-history-columns.csv) for the complete column definition.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Stage name shows as lookup instead of text | Delete the column and recreate as Single line of text — this is critical for audit integrity |
| Staff can edit items directly | Verify list permissions; ensure Staff App Timeline tab uses DisplayMode.View on all controls |
| Title column left blank | Flow 6 sets Title automatically — verify flow is running after stage change |
