# Task P1-13: Customer Fact-Find Access (Guest Sharing)

**Phase:** 1 — Fact-Find  
**Story:** As a customer, I want to open my unique fact-find link without needing an M365 account so that I can complete my application easily  
**Priority:** 🔴 Critical  
**Estimated effort:** 30 minutes (configuration, not development)  
**Depends on:** Fact-Find Responses list (P1-03), Flow 4 generating guest links (P1-07), IT Admin enabling guest sharing

---

## Step-by-Step Guide

### Step 1 — Verify Tenant-Level Guest Sharing

> ⚠️ This step requires **M365 Global Admin** or **SharePoint Admin** access.

1. Go to **SharePoint Admin Center**: `https://[tenant]-admin.sharepoint.com`
2. Click **Policies** → **Sharing**
3. Set **SharePoint** sharing level to: **New and existing guests** (minimum)
4. Set **OneDrive** sharing level to: **New and existing guests**
5. Under **More external sharing settings**:
   - ☑ Allow guests to share items they don't own
   - Set link expiry to **Never** (or long duration — e.g. 365 days)
6. Click **Save**

### Step 2 — Verify Site-Level Guest Sharing

1. In SharePoint Admin Center → **Sites** → **Active sites**
2. Find the lending site: `https://[tenant].sharepoint.com/sites/lending`
3. Click the site → **Sharing** tab
4. Set sharing level to: **New and existing guests**
5. Click **Save**

### Step 3 — Test Guest Access to a Fact-Find Item

1. Create a test case through the Staff App
2. Wait for Flow 4 to create the Fact-Find item and generate the guest link
3. Copy the guest link from the Case record (`FactFindGuestLink` column)
4. Open an **Incognito / Private browser window** (not signed in to M365)
5. Paste the guest link and navigate to it

**Expected behavior:**
- ✅ The Fact-Find item edit form loads
- ✅ No M365 sign-in prompt appears (or a simple guest verification — email code — appears)
- ✅ Customer can edit all fields
- ✅ Customer can save the form
- ✅ Customer can return to the same link later and continue editing

### Step 4 — Test Multi-Save Capability

1. Open the guest link
2. Fill in A1FirstName and A1LastName → Save
3. Close the browser
4. Re-open the link
5. Verify the saved data is still there
6. Fill in more fields → Save again

### Step 5 — Verify Data Isolation

1. Create a second test case for a different customer
2. Open Customer 1's guest link
3. Verify Customer 1 CANNOT see Customer 2's data
4. The guest link points to a **specific item**, not the list

### Step 6 — Document the IT Admin Requirements

Ensure the following are tracked as prerequisites:

| Requirement | Owner | Status |
|---|---|---|
| SharePoint external sharing enabled at tenant level | IT Admin / Global Admin | ☐ |
| Site-level sharing set to "New and existing guests" | IT Admin | ☐ |
| Guest link expiry set to appropriate duration | IT Admin | ☐ |
| Test email deliverability (verification codes may be sent to guests) | Developer | ☐ |

### Step 7 — Verify

- [ ] Guest link opens fact-find form without M365 login
- [ ] Customer can edit and save all sections
- [ ] Customer can return and continue editing (multi-save)
- [ ] Customer can only see their own fact-find item
- [ ] Guest sharing is enabled at both tenant and site level

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Guest link requires M365 sign-in | Tenant sharing level is too restrictive — must be "New and existing guests" or "Anyone" |
| "You need permission to access this item" | Site-level sharing may be more restrictive than tenant level |
| Guest receives verification code email | This is normal for "New and existing guests" mode — guest enters their email and receives a one-time code |
| Guest link expired | Check link expiry policy in SharePoint Admin → Sharing → Link expiry |
| Customer sees the full list instead of their item | The link must be a direct item link, not a list link — verify Flow 4 generates an item-specific share |
