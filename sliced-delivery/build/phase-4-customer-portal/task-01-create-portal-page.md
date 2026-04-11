# Task P4-01: Create SharePoint Portal Page Template

**Phase:** 4 — Customer Portal  
**Story:** As a developer, I need to create a SharePoint portal page template for each customer case so that customers have a branded landing page  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** SharePoint site provisioned, Phase 1 complete (guest links available)

---

## Step-by-Step Guide

### Step 1 — Understand the Portal Page Structure

Each customer case gets its own SharePoint page. The page layout follows this wireframe:

```
╔══════════════════════════════════════════════════════════════════════╗
║                                                                      ║
║         🏦  YOUR LENDING APPLICATION                                 ║
║         Specialist Lending Hub — Customer Portal                     ║
║                                                                      ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  Welcome, [Applicant Name]                                           ║
║  Case Reference: [CaseID]                                            ║
║                                                                      ║
║  APPLICATION PROGRESS                                                ║
║  ─────────────────────────────────────────────────────────────────  ║
║   ✅ Application Received                                            ║
║   ✅ Information Gathering                                           ║
║   ✅ Terms Agreed                                                    ║
║   ✅ Options Presented                                               ║
║   ✅ Illustrations Prepared                                          ║
║   ✅ Initial Decision                                                ║
║   ▶  Application Submitted  ← YOU ARE HERE                          ║
║   ○  Valuation Arranged                                              ║
║   ○  Lender Review                                                   ║
║   ○  Offer Issued                                                    ║
║   ○  Legal Work                                                      ║
║   ○  Funds Released                                                  ║
║                                                                      ║
║  WHAT YOU NEED TO DO                                                 ║
║  ─────────────────────────────────────────────────────────────────  ║
║  ⚠  Please upload your bank statements (last 6 months)              ║
║  ⚠  Your EPC certificate was rejected — please re-upload            ║
║  [📤 Upload Documents]                                               ║
║                                                                      ║
║  MESSAGES                                                            ║
║  ─────────────────────────────────────────────────────────────────  ║
║  ✉  You have 1 unread message from your advisor                      ║
║  [📨 View Messages]                                                  ║
║                                                                      ║
║  LINKS                                                               ║
║  ─────────────────────────────────────────────────────────────────  ║
║  [📋 Update Your Information (Fact-Find)]                            ║
║  [📁 View Your Documents]                                            ║
║                                                                      ║
║  Questions? Contact your advisor: sarah@[firmname].co.uk             ║
╚══════════════════════════════════════════════════════════════════════╝
```

### Step 2 — Create the First Portal Page

1. Navigate to `https://[tenant].sharepoint.com/sites/lending`
2. Click **+ New** → **Page** → **Blank** template
3. Set the page title: `YOUR LENDING APPLICATION`
4. Click the page title area and set it to a clean heading style

### Step 3 — Add the Header Section

1. Click **+** to add a new section → choose **One column** layout
2. Add a **Text** web part
3. Enter the header content:
   ```
   🏦 YOUR LENDING APPLICATION
   Specialist Lending Hub — Customer Portal
   ```
4. Format "YOUR LENDING APPLICATION" as **Heading 1**
5. Format "Specialist Lending Hub — Customer Portal" as subtitle text

### Step 4 — Add the Welcome & Case Reference Section

1. Add another **Text** web part below the header
2. Enter:
   ```
   Welcome, [Applicant Name]
   Case Reference: [CaseID]
   ```
3. Replace `[Applicant Name]` and `[CaseID]` with the actual customer name and case reference for this case

> **Tip:** For this version, manually create each page. In a future enhancement, a Power Automate flow can auto-generate pages.

### Step 5 — Add the Application Progress Section

1. Add a **Text** web part with the heading: `APPLICATION PROGRESS`
2. Below it, add a **List** web part
3. Select the **Stage Configuration** list
4. Configure the view to show only `VisibleToCustomer = Yes` stages
5. See Task P4-02 for full progress tracker configuration

> **Alternative:** Use the **Embed** web part with the HTML template from [`imports/portal-page-template.html`](imports/portal-page-template.html) for a styled custom layout.

### Step 6 — Add the "What You Need To Do" Section

1. Add a section divider or **Text** web part heading: `WHAT YOU NEED TO DO`
2. Add a **List** web part
3. Select the **Document Requests** list
4. Filter by the customer's CaseID where Status = "Requested" or "Rejected"
5. See Task P4-03 for full action panel configuration

### Step 7 — Add the Messages Section

1. Add a **Text** web part heading: `MESSAGES`
2. Add a **List** web part
3. Select the **Messages** list
4. Filter to the customer's CaseID, Direction ≠ "Internal"
5. See Task P4-05 for full messages configuration

### Step 8 — Add the Links Section

1. Add a **Text** web part heading: `LINKS`
2. Add a **Button** web part or formatted link:
   - `📋 Update Your Information (Fact-Find)` → link to the guest-shared fact-find URL (from Phase 1 Flow 4)
   - `📁 View Your Documents` → link to the document upload folders
3. See Tasks P4-04 and P4-06 for link configuration

### Step 9 — Add Advisor Contact Information

1. Add a **Text** web part at the bottom
2. Enter: `Questions? Contact your advisor: [advisor email]`
3. Replace with the actual case owner's email address

### Step 10 — Add the Embed Web Part (Recommended)

For a polished, branded look, use the **Embed** web part instead of individual web parts:

1. Click **+** → **Embed**
2. Open [`imports/portal-page-template.html`](imports/portal-page-template.html)
3. Replace all `{{placeholder}}` variables with the customer's actual values:
   - `{{CaseID}}` → e.g. `BTL-2026-0001`
   - `{{ApplicantName}}` → e.g. `John & Jane Doe`
   - `{{FactFindGuestLink}}` → the guest link from Phase 1 Flow 4
   - `{{Folder01Link}}` → guest link for 01-Identity folder
   - `{{Folder02Link}}` → guest link for 02-Income folder
   - `{{Folder03Link}}` → guest link for 03-Property folder
   - `{{Folder04Link}}` → guest link for 04-Bank-Statements folder
   - `{{AdvisorName}}` → case owner's name
   - `{{AdvisorEmail}}` → case owner's email
4. Paste the completed HTML into the Embed web part
5. Click **Publish**

### Step 11 — Configure Page Sharing

1. Click **Share** on the published page
2. Choose **Specific people** → enter the customer's email address
3. Set permission to **Can view**
4. Send the sharing invitation

### Step 12 — Repeat for Each Case

For each new customer case:

1. Duplicate the portal page template (click **⋯** on the page → **Copy**)
2. Update all placeholder values with the new customer's details
3. Share with the new customer

> **Future enhancement:** Automate page creation with Power Automate. The flow would duplicate a template page and populate placeholders on case creation.

---

## Verify

- [ ] Portal page loads with correct branding (🏦 heading, subtitle)
- [ ] Customer name and Case Reference are displayed correctly
- [ ] Application Progress section is visible (configured in P4-02)
- [ ] "What You Need To Do" section is visible (configured in P4-03)
- [ ] Messages section is visible (configured in P4-05)
- [ ] Links section contains Fact-Find and Documents links
- [ ] Advisor contact information is displayed at the bottom
- [ ] Page is accessible via the shared guest link
- [ ] Page renders correctly on mobile devices

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Customer cannot access the page | Verify the page was shared with the customer's email address; check guest sharing is enabled (P4-08) |
| Embed web part shows "content blocked" | SharePoint may block certain HTML; simplify the template or use standard web parts |
| Page layout looks broken on mobile | Test with the SharePoint mobile app; use responsive sections in the page layout |
| List web parts show "No items" | Verify CaseID filter matches exactly; check the list view configuration |
| Customer can see other pages | Ensure pages are shared individually, not via site-level access |
