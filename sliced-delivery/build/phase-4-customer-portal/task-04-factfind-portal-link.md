# Task P4-04: Add Fact-Find Access Button to Portal

**Phase:** 4 — Customer Portal  
**Story:** As a customer, I want a prominent button to open my fact-find form so that I can update my information easily  
**Priority:** 🟡 High  
**Estimated effort:** 30 minutes  
**Depends on:** P4-01 (portal page created), Phase 1 Flow 4 (fact-find guest link generated)

---

## Step-by-Step Guide

### Step 1 — Locate the Fact-Find Guest Link

The guest link to the customer's fact-find item was generated during case creation by Phase 1 Flow 4. To find it:

1. Navigate to the **Specialist Lending Cases** list
2. Find the customer's case record (e.g. `BTL-2026-0001`)
3. Look for the column that stores the fact-find guest link (typically `FactFindGuestLink` or stored in a separate reference)
4. Copy the full URL

> **Example link format:** `https://[tenant].sharepoint.com/:li:/s/lending/[unique-share-id]`

### Step 2 — Add the Fact-Find Button to the Portal Page

1. Open the customer's portal page (P4-01) in **Edit** mode
2. Navigate to the **LINKS** section
3. Add a **Button** web part:
   - Click **+** → search for **Button**
   - Label: `📋 Update Your Information (Fact-Find)`
   - Link: paste the fact-find guest link from Step 1
   - Alignment: **Left** or **Center**

**Alternative — using a Text web part:**

1. Add a **Text** web part in the LINKS section
2. Type: `📋 Update Your Information (Fact-Find)`
3. Select the text → click the **Link** icon (🔗) in the toolbar
4. Paste the fact-find guest link
5. Format the text as bold to make it prominent

**Alternative — using the Embed web part with HTML:**

If using the portal HTML template (`imports/portal-page-template.html`), the fact-find link is already included. Replace the `{{FactFindGuestLink}}` placeholder with the actual guest link URL.

### Step 3 — Style the Button for Prominence

The fact-find button should be visually prominent so customers don't miss it:

- Use a **large button** style or bold link text
- Place it in the LINKS section or as a standalone call-to-action in its own section
- Consider adding instructional text above:
  ```
  Please complete your application information by clicking the button below.
  You can save your progress and return later.
  ```

### Step 4 — Verify the Link Works

1. Copy the portal page URL
2. Open an **InPrivate / Incognito** browser window (to simulate guest access)
3. Paste the fact-find guest link
4. Verify:
   - The fact-find edit form loads
   - The form shows the correct applicant name and case reference
   - The customer can edit and save the form
   - The customer can submit the form

### Step 5 — Verify Customer Experience Flow

Test the full flow from the portal:

1. Open the portal page as a guest customer
2. Click the `📋 Update Your Information (Fact-Find)` button
3. Verify the fact-find form opens in a new tab
4. Fill in some fields and click **Save**
5. Verify changes persist when reloading the form
6. Click **Submit** (if ready)
7. Verify Flow 15 triggers (updates `LastUpdatedByCustomer` timestamp, notifies case owner)

---

## Verify

- [ ] Fact-find button/link is visible on the portal page
- [ ] Button text reads "📋 Update Your Information (Fact-Find)"
- [ ] Clicking the button opens the customer's fact-find edit form
- [ ] The form is pre-populated with the correct case reference and applicant name
- [ ] Customer can edit and save the form
- [ ] Customer can submit the completed form
- [ ] Button opens in a new tab (if configured)
- [ ] The button is visually prominent and easy to find

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Button link shows "Access Denied" | Verify the guest share link is still active; regenerate via Phase 1 Flow 4 if expired |
| Form opens but shows wrong customer | Check the guest link points to the correct Fact-Find Responses item ID |
| Customer cannot save the form | Verify the guest share link grants Edit (not Read) access |
| Link expired | Check guest link expiry settings (P4-08); regenerate the link |
| Button not visible on mobile | Test with SharePoint mobile app; ensure the LINKS section is not collapsed |
