# Task P3-04: Create the Tasks List

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a developer, I need to create the Tasks list so that staff can assign and track internal tasks linked to cases  
**Priority:** 🟡 High  
**Estimated effort:** 1 hour  
**Depends on:** Cases list (P1-01)

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Open your SharePoint site: `https://[tenant].sharepoint.com/sites/lending`
2. Click **⚙️ Settings** → **Site contents**
3. Click **+ New** → **List** → **Blank list**
4. Name: `Tasks`
5. Description: `Internal tasks linked to lending cases — chase items, admin tasks, follow-ups`
6. Click **Create**

### Step 2 — Add Columns

Add each column in order. Use **+ Add column** or **List Settings → Create column**.

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CaseID | Single line of text | No | FK to Cases list (optional — some tasks may be general) |
| CaseName | Single line of text | No | Denormalized |
| AssignedTo | Person or Group | Yes | Allow selection of people only |
| CreatedBy | Person or Group | Yes | Allow selection of people only |
| DueDate | Date and Time | Yes | Date only |
| Priority | Choice | Yes | Choices: `High`, `Medium`, `Low`. Default: `Medium` |
| Status | Choice | Yes | Choices: `Not Started`, `In Progress`, `Completed`, `Cancelled`. Default: `Not Started` |
| Category | Choice | No | Choices: `Chase Customer`, `Chase Lender`, `Chase Solicitor`, `Internal`, `Admin`, `Other` |
| Notes | Multiple lines of text | No | Plain text |
| CompletedDate | Date and Time | No | Date only — set when Status = Completed |
| IsOverdue | Yes/No | No | Default: No. Updated by SLA Reminder flow (Flow 9) |

### Step 3 — Configure the Title Column

1. Go to **List Settings** → click **Title** column
2. Keep display name as `Title`
3. Set **Require that this column contains information** = Yes
4. This holds the task description (e.g. "Chase customer for payslips")

### Step 4 — Create Views

#### My Tasks View

1. Go to **List Settings** → **Views** → **Create view**
2. Name: `My Tasks`
3. Filter: `AssignedTo is equal to [Me] AND Status is not equal to Completed AND Status is not equal to Cancelled`
4. Columns: `Title`, `CaseID`, `Priority`, `Status`, `DueDate`, `Category`, `IsOverdue`
5. Sort by: `DueDate` (ascending)

#### Overdue Tasks View

1. Create another view named `Overdue Tasks`
2. Filter: `IsOverdue is equal to Yes AND Status is not equal to Completed`
3. Columns: `Title`, `CaseID`, `AssignedTo`, `Priority`, `DueDate`, `Category`
4. Sort by: `DueDate` (ascending)

#### By Case View

1. Create another view named `By Case`
2. Columns: `Title`, `AssignedTo`, `Priority`, `Status`, `DueDate`, `Category`
3. Group by: `CaseID`
4. Sort by: `DueDate` (ascending)

### Step 5 — Verify

- [ ] List appears in Site Contents as "Tasks"
- [ ] All columns are present with correct types
- [ ] Priority defaults to Medium
- [ ] Status defaults to Not Started
- [ ] IsOverdue defaults to No
- [ ] CaseID is not required (allows general tasks)
- [ ] All three views are created and configured

---

## Importable Content

See [`imports/tasks-list-columns.csv`](imports/tasks-list-columns.csv) for the complete column definition.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| [Me] filter not working in view | Use `[Me]` (with brackets) in the view filter — this is a SharePoint token for the current user |
| Choice defaults not applying | Edit each Choice column in List Settings and set the default value explicitly |
| IsOverdue not updating | This column is updated by Flow 9 (SLA Reminder) — verify the flow is running |
