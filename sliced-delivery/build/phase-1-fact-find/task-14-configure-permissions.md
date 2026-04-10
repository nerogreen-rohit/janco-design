# Task P1-14: Configure Permissions

**Phase:** 1 — Fact-Find  
**Story:** As a developer, I need to configure permissions so that customers can only access their own data (fact-find item + folders 01–04)  
**Priority:** 🔴 Critical  
**Estimated effort:** 30 minutes  
**Depends on:** All Phase 1 lists and flows operational

---

## Step-by-Step Guide

### Step 1 — Understand the Permission Model

Phase 1 uses **item-level and folder-level guest sharing** (not list-level access). This means:

| Resource | Customer Access | How Enforced |
|---|---|---|
| Their Fact-Find item | Edit (specific item only) | Guest-shared direct item link (Flow 4) |
| Folders 01–04 of their case | Contribute (upload) | Guest-shared folder links (Flow 5) |
| Other customers' Fact-Find items | ❌ No access | Guest link is item-specific |
| Folders 05–13 | ❌ No access | Guest sharing only applied to 01–04 |
| Cases list | ❌ No access | No sharing applied |
| Stage Configuration list | ❌ No access | No sharing applied |
| Counters list | ❌ No access | No sharing applied |

### Step 2 — Verify Fact-Find Item Isolation

1. Create two test cases (Case A for Customer A, Case B for Customer B)
2. Get the guest link for Customer A's Fact-Find item
3. Verify Customer A CAN access their own item
4. Verify Customer A CANNOT:
   - Browse the full Fact-Find Responses list
   - See Customer B's fact-find data
   - Navigate to the Cases list
   - Navigate to any other SharePoint list

### Step 3 — Verify Folder Permission Isolation

1. Get the guest link for Customer A's `01-Identity` folder
2. Verify Customer A CAN:
   - Upload files to 01-Identity
   - Upload files to 02-Income, 03-Property, 04-Bank-Statements (via their respective links)
3. Verify Customer A CANNOT:
   - Navigate to the parent folder (`Case Documents/{CaseID}/`)
   - Access folders 05–13
   - Access any other case's folders

### Step 4 — Staff Permissions

Staff access the site as normal M365 users. Verify:

| Role | Cases List | Fact-Find List | Case Documents | Stage Config |
|---|---|---|---|---|
| Staff (Members) | Full control | Full control | Full control | Read |
| Site Owner | Full control | Full control | Full control | Full control |

To set this up:

1. Go to **Site Settings** → **Site permissions**
2. Add staff users to the **Members** group (Edit permissions)
3. Add admin/system owner to the **Owners** group (Full control)

### Step 5 — Break Inheritance on Sensitive Lists (Optional)

If you want to restrict staff from editing Stage Configuration:

1. Go to **Stage Configuration** → **List Settings** → **Permissions for this list**
2. Click **Stop Inheriting Permissions**
3. Remove the Members group's Edit access
4. Add Members with **Read** access only
5. Keep Owners with Full Control

> This is optional in Phase 1 since there are only 2–4 staff users. It becomes more important as the team grows.

### Step 6 — Permission Verification Checklist

Run through this checklist with a test customer account:

- [ ] Customer opens fact-find guest link → item edit form loads
- [ ] Customer saves changes to fact-find → changes persist
- [ ] Customer navigates to `https://[tenant].sharepoint.com/sites/lending/Lists/Fact-Find%20Responses` → **Access Denied**
- [ ] Customer navigates to `https://[tenant].sharepoint.com/sites/lending/Lists/Specialist%20Lending%20Cases` → **Access Denied**
- [ ] Customer opens folder 01-Identity guest link → can upload files
- [ ] Customer navigates to parent folder → **Access Denied** or no navigation available
- [ ] Customer opens folder 05-Fact-Find directly → **Access Denied**
- [ ] Customer opens another case's folder → **Access Denied**

### Step 7 — Document Permissions for Handover

Record the final permission configuration:

| Actor | Resource | Access Level | Method |
|---|---|---|---|
| Staff (Members group) | All lists | Edit | Site membership |
| Staff (Members group) | Case Documents | Contribute | Site membership |
| System Owner (Owners group) | All lists | Full Control | Site membership |
| Customer | Their Fact-Find item | Edit | Guest-shared direct item link |
| Customer | Folders 01–04 | Contribute | Guest-shared folder links |
| Customer | Everything else | No access | No sharing applied |

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Customer can browse the full list | The share link is a list link, not an item link — regenerate with Flow 4 |
| Customer can't upload to folders | Check folder-level sharing permissions (Contribute role required) |
| Staff can't edit cases | Verify staff are in the Members group with Edit permissions |
| "Access Denied" for staff | Check site permissions; staff need to be invited to the site |
