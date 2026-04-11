# Task P4-08: Verify IT Admin Prerequisites Before Go-Live

**Phase:** 4 — Customer Portal  
**Story:** As a developer, I need to verify all IT Admin prerequisites before go-live so that guest sharing and email delivery work correctly  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours (coordination with IT Admin)  
**Depends on:** IT Admin access to M365 tenant settings

---

## Step-by-Step Guide

### Prerequisite 1 — SharePoint Guest Sharing Enabled at M365 Tenant Level

**Owner:** IT Admin  
**Risk:** Medium — requires tenant admin access. Raise at start of project.

#### Steps

1. IT Admin signs in to the **Microsoft 365 Admin Center**: `https://admin.microsoft.com`
2. Navigate to **Settings** → **Org settings** → **SharePoint**
   - Or go directly to the **SharePoint Admin Center**: `https://[tenant]-admin.sharepoint.com`
3. Click **Policies** → **Sharing**
4. Under **External sharing**, verify the SharePoint slider is set to at least **"New and existing guests"**
   - **Most permissive:** "Anyone" (anonymous links — not recommended for security)
   - **Recommended:** "New and existing guests" (requires sign-in)
   - **Too restrictive:** "Existing guests only" or "Only people in your organization" — these will block customer access
5. Click **Save** if changes were made

#### Verify

- [ ] SharePoint external sharing is set to "New and existing guests" or more permissive
- [ ] OneDrive external sharing matches or is more permissive than SharePoint setting

> **Important:** This is a tenant-wide setting. If the organisation has security policies that restrict external sharing, escalate early. This requirement should be raised in Week 1.

---

### Prerequisite 2 — "New and Existing Guests" Sharing at Site Level

**Owner:** IT Admin  
**Risk:** Low — once tenant setting is enabled, site-level is straightforward.

#### Steps

1. Open the **SharePoint Admin Center**: `https://[tenant]-admin.sharepoint.com`
2. Click **Sites** → **Active sites**
3. Find the lending site: `https://[tenant].sharepoint.com/sites/lending`
4. Click the site name → **Policies** tab
5. Under **External sharing**, verify the setting is **"New and existing guests"**
   - If it shows "Only people in your organization", click **Edit** and change it
6. Click **Save**

**Alternative (from the site itself):**

1. Navigate to `https://[tenant].sharepoint.com/sites/lending`
2. Click **⚙️ Settings** → **Site permissions** → **Change how members can share**
3. Ensure "Site owners and members can share files, folders, and the site. People with Edit permissions can share files and folders." is selected
4. Ensure "Allow sharing with external users" is enabled

#### Verify

- [ ] Lending site external sharing is set to "New and existing guests"
- [ ] A test guest share link can be generated for a list item on the site

---

### Prerequisite 3 — Guest Link Expiry Set to "Never" or Long Duration

**Owner:** IT Admin  
**Risk:** Low — must be set before customer links are generated.

#### Steps

1. Open the **SharePoint Admin Center**: `https://[tenant]-admin.sharepoint.com`
2. Click **Policies** → **Sharing**
3. Under **Choose expiration and permissions options for guest links**:
   - **Guest access links:** Set to **"Never"** or a long duration (e.g. 365 days)
   - If set to a short duration (e.g. 30 days), customer links will expire and need regeneration
4. Under **Choose permissions for sharing links by default:**
   - Files: **View** (or Edit for fact-find)
   - Folders: **View and edit** (for upload capability)
5. Click **Save**

#### Important Considerations

| Setting | Recommended | Why |
|---|---|---|
| Guest link expiry | Never (or 365+ days) | Customer links must remain active for the duration of their case |
| Default sharing permission | View | Prevents accidental over-sharing; specific links override this |
| Require guests to sign in | Yes | Provides audit trail and prevents anonymous access |

> **Security note:** If the organisation requires link expiry, implement a Power Automate flow (future enhancement) to regenerate and email new links to customers before expiry.

#### Verify

- [ ] Guest link expiry is set to "Never" or 365+ days
- [ ] A test guest link generated 7+ days ago still works
- [ ] Default sharing permission is set appropriately

---

### Prerequisite 4 — Automated Email Deliverability Tested

**Owner:** Developer  
**Risk:** Medium — automated emails may hit spam filters.

#### Steps

1. **Test SharePoint sharing emails:**
   - Share a test item with an external email address (e.g. a personal Gmail/Outlook.com account)
   - Verify the sharing invitation email arrives in the recipient's inbox (not spam/junk)
   - Test with multiple email providers: Gmail, Outlook.com, Yahoo, and at least one corporate email

2. **Test Power Automate notification emails:**
   - Trigger a test flow that sends a notification email (e.g. Flow 6 welcome email from Phase 1)
   - Verify the email arrives at the customer's address
   - Check spam/junk folders

3. **If emails land in spam:**
   - Check if the tenant has **SPF**, **DKIM**, and **DMARC** records configured:
     - SPF: `v=spf1 include:spf.protection.outlook.com -all`
     - DKIM: Enabled in Exchange Online admin
     - DMARC: `v=DMARC1; p=quarantine; rua=mailto:dmarc@[domain]`
   - Ask IT Admin to verify these DNS records
   - Consider using a custom "From" address in Power Automate via a shared mailbox

4. **Test the welcome email flow:**
   - Create a test case with a real external email address
   - Verify the welcome email arrives with:
     - Working fact-find link
     - Working document upload folder links
     - Working portal page link
   - Click each link to confirm they work from the email

#### Verify

- [ ] SharePoint sharing invitation emails arrive in inbox (not spam)
- [ ] Power Automate notification emails arrive in inbox (not spam)
- [ ] Emails tested with at least 3 different email providers
- [ ] Welcome email contains all working links
- [ ] SPF, DKIM, and DMARC records are configured on the tenant domain

---

## Pre-Go-Live Checklist Summary

| # | Prerequisite | Owner | Risk | Status |
|---|---|---|---|---|
| 1 | SharePoint guest sharing enabled at M365 tenant level | IT Admin | Medium | ☐ |
| 2 | "New and existing guests" sharing at site level | IT Admin | Low | ☐ |
| 3 | Guest link expiry set to "Never" or long duration | IT Admin | Low | ☐ |
| 4 | Automated email deliverability tested | Developer | Medium | ☐ |

> **When to raise:** Prerequisites 1–3 should be raised with IT Admin at the **start of the project** (Week 1), not at Phase 4. This allows time for any approvals or policy discussions.

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| "External sharing is turned off" error when sharing | Prerequisite 1 not met — IT Admin must enable tenant-level sharing |
| Can share from other sites but not the lending site | Prerequisite 2 not met — site-level sharing must be enabled |
| Guest links expire after 30 days | Prerequisite 3 not met — increase expiry to "Never" or regenerate links |
| Customer never receives the sharing email | Check spam/junk; verify email address; check email deliverability (Prerequisite 4) |
| Email arrives but links don't work | Links may have expired (Prerequisite 3) or sharing was revoked |
| IT Admin won't enable guest sharing | Escalate: guest sharing is a core requirement for the customer portal. Without it, Phase 4 cannot be delivered. Explore alternatives (e.g. Microsoft Forms for fact-find) as a fallback |
| Corporate firewall blocks SharePoint guest access | Customer's employer may block external SharePoint sites — advise customer to use a personal device or contact their IT department |
