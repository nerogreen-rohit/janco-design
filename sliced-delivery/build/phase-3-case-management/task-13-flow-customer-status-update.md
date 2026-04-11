# Task P3-13: Flow 13 — Customer Status Update

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a system, when a case moves to a customer-visible stage I need to send the customer a status update email so that they are kept informed  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** Cases list (P1-01), Stage Configuration list (P3-07), Flow 6 — Log Stage Change (P3-08)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 13 — Customer Status Update`
3. Trigger: **When an HTTP request is received** (if called as a child flow from Flow 6)

   **Alternative:** Use **When an item is created** on Case Stage History list, with a condition that `VisibleToCustomer = Yes` from Stage Configuration.

4. If using HTTP trigger, define the request body schema:

```json
{
    "type": "object",
    "properties": {
        "CaseID": { "type": "string" },
        "CustomerFriendlyName": { "type": "string" },
        "Applicant1Email": { "type": "string" },
        "Applicant1FirstName": { "type": "string" }
    }
}
```

### Step 2 — Get Case Data (if triggered from Stage History)

If using the Stage History trigger approach:

1. Add **Get items** (SharePoint) — Get the Case:
   - List: `Specialist Lending Cases`
   - Filter Query: `CaseID eq '@{triggerOutputs()?['body/CaseID']}'`
   - Top Count: 1

2. Add **Get items** (SharePoint) — Get the Stage Config:
   - List: `Stage Configuration`
   - Filter Query: `Title eq '@{triggerOutputs()?['body/StageName']}'`
   - Top Count: 1

   > **Note:** Use **Get items** (plural) with Top Count = 1, not **Get item** (singular). The "Get item" action requires a list item ID and does not support OData filter queries. Reference the result as `first(outputs('Get_items_-_StageConfig')?['body/value'])`.
   >
   > **Alternative (preferred):** If Flow 6 calls Flow 13 as a child flow, pass the Stage Configuration item ID directly so you can use **Get item** by ID instead of filtering by Title.

3. Add a **Condition**: `first(outputs('Get_items_-_StageConfig')?['body/value'])?['VisibleToCustomer']` equals `true`
   - If **No** → **Terminate** (stage not visible to customer)

### Step 3 — Send Customer Email

1. Add **Send an email (V2)** (Office 365 Outlook):

   - **To:** Customer email (from trigger or Case record)
   - **Subject:** `Your Application Update — @{triggerBody()?['CustomerFriendlyName']}`
   - **Body (HTML):**

```html
<div style="font-family: Arial, sans-serif; max-width: 600px;">
    <h2 style="color: #2c3e50;">Application Update</h2>

    <p>Dear @{triggerBody()?['Applicant1FirstName']},</p>

    <p>We wanted to let you know that your lending application has progressed
       to a new stage:</p>

    <div style="background: #e8f4f8; padding: 20px; border-radius: 8px;
                margin: 20px 0; text-align: center;">
        <p style="font-size: 14px; color: #666; margin: 0 0 5px 0;">
            Current Status
        </p>
        <p style="font-size: 22px; color: #0078d4; font-weight: bold; margin: 0;">
            @{triggerBody()?['CustomerFriendlyName']}
        </p>
    </div>

    <p>Your advisor is managing your application and will be in touch if
       any action is required from you.</p>

    <p style="text-align: center; margin: 20px 0;">
        <a href="https://[tenant].sharepoint.com/sites/lending/portal"
           style="background: #0078d4; color: white; padding: 12px 24px;
                  text-decoration: none; border-radius: 4px;">
            View Your Portal →
        </a>
    </p>

    <p style="color: #666; font-size: 12px;">
        This is an automated update from Janco Finance. If you have any
        questions, please reply to this email or contact your advisor directly.
    </p>
</div>
```

### Step 4 — Error Handling

1. Wrap the email action in a **Scope**
2. Add a parallel failure branch:
   - Send Teams notification to case owner:
     ```
     ⚠️ Flow 13 Failed — Customer Status Update
     Case: @{triggerBody()?['CaseID']}
     Stage: @{triggerBody()?['CustomerFriendlyName']}
     Customer email may not have been sent.
     ```

### Step 5 — Test

1. Advance a test case to a stage where `VisibleToCustomer = Yes` (e.g. Stage 5 — Options Presented)
2. Wait 30–60 seconds for Flow 6 → Flow 13 chain
3. Verify:
   - [ ] Customer receives email with the `CustomerFriendlyName` (e.g. "Options Presented")
   - [ ] Email does NOT use internal stage names (e.g. "Present Options & Agree")
   - [ ] Portal link is included and working
   - [ ] Email renders correctly on desktop and mobile

4. Advance to Stage 4 (Product Research, VisibleToCustomer = No)
5. Verify:
   - [ ] Customer does NOT receive an email for this stage

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Customer gets email for internal stages | Verify the VisibleToCustomer condition is checked before sending; Stage 4 should be No |
| Email shows internal stage name | Ensure the email uses `CustomerFriendlyName` from Stage Configuration, NOT `Title` |
| Flow 13 not triggered | Check Flow 6 is calling Flow 13 correctly; verify HTTP trigger URL or child flow connection |
| Customer email blank | Verify Applicant1Email is populated on the Case record |
