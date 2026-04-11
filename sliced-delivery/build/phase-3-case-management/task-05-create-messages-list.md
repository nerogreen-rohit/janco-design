# Task P3-05: Create the Messages List

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a developer, I need to create the Messages list so that case-level communication between staff and customers is stored  
**Priority:** 🔴 Critical  
**Estimated effort:** 1 hour  
**Depends on:** Cases list (P1-01)

---

## Step-by-Step Guide

### Step 1 — Create the List

1. Open your SharePoint site: `https://[tenant].sharepoint.com/sites/lending`
2. Click **⚙️ Settings** → **Site contents**
3. Click **+ New** → **List** → **Blank list**
4. Name: `Messages`
5. Description: `Case-level threaded communication between staff (advisors) and customers`
6. Click **Create**

### Step 2 — Add Columns

Add each column in order. Use **+ Add column** or **List Settings → Create column**.

| Column Name | Type | Required | Configuration |
|---|---|---|---|
| CaseID | Single line of text | Yes | FK to Cases list |
| CaseName | Single line of text | No | Denormalized |
| MessageBody | Multiple lines of text | Yes | Plain text — message content |
| SentBy | Person or Group | Yes | Sender (staff member) |
| SentByName | Single line of text | No | Denormalized display name |
| Direction | Choice | Yes | Choices: `Advisor → Customer`, `Customer → Advisor`, `Internal` |
| IsRead | Yes/No | No | Default: No. Customer has read |
| IsReadByStaff | Yes/No | No | Default: No. Staff has read |
| SentDate | Date and Time | Yes | Include time |
| AttachmentURL | Hyperlink | No | Optional document link |
| IsSystemMessage | Yes/No | No | Default: No. Auto-generated status notification |

> **Design note:** Direction uses **"Advisor"** (not "Staff" or "Case Owner") in all customer-facing contexts to maintain a professional, personal tone.

### Step 3 — Rename the Title Column

1. Go to **List Settings** → click **Title** column
2. Set **Require that this column contains information** = No
3. Title will be auto-set to `{CaseID}-{timestamp}` by the Staff App or flow

### Step 4 — Create Views

#### By Case View

1. Go to **List Settings** → **Views** → **Create view**
2. Name: `By Case`
3. Columns: `Title`, `CaseID`, `Direction`, `SentByName`, `SentDate`, `IsRead`, `IsReadByStaff`
4. Group by: `CaseID`
5. Sort by: `SentDate` (descending)

#### Unread Messages View

1. Create another view named `Unread — Staff`
2. Filter: `IsReadByStaff is equal to No AND Direction is equal to Customer → Advisor`
3. Columns: `CaseID`, `CaseName`, `SentByName`, `SentDate`, `MessageBody`
4. Sort by: `SentDate` (descending)

### Step 5 — Verify

- [ ] List appears in Site Contents as "Messages"
- [ ] All columns are present with correct types
- [ ] Direction choice values are exactly: `Advisor → Customer`, `Customer → Advisor`, `Internal`
- [ ] IsRead and IsReadByStaff both default to No
- [ ] IsSystemMessage defaults to No
- [ ] Title column is not required
- [ ] Both views are created and configured

---

## Importable Content

See [`imports/messages-list-columns.csv`](imports/messages-list-columns.csv) for the complete column definition.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Direction choice arrow characters not displaying | Use the Unicode arrow `→` (U+2192) — type it or copy/paste from this guide |
| Messages not linked to cases | Verify CaseID is populated when creating items from the Staff App |
| SentDate blank | Ensure the Staff App sets `Now()` when creating a message item |
