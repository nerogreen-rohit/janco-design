# Task P3-14: Flow 14 — Message Notification

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a system, when a staff member sends a message to a customer I need to email the customer a notification so that they know to check the portal  
**Priority:** 🟡 High  
**Estimated effort:** 1 hour  
**Depends on:** Messages list (P3-05), Cases list (P1-01)

---

## Step-by-Step Guide

### Step 1 — Create the Flow

1. Go to **Power Automate** → **+ Create** → **Automated cloud flow**
2. Name: `Flow 14 — Message Notification`
3. Trigger: **When an item is created** (SharePoint)
4. Site Address: `https://[tenant].sharepoint.com/sites/lending`
5. List Name: `Messages`

### Step 2 — Filter by Direction

Only send email when the message is from advisor to customer:

1. Add a **Condition**: `triggerOutputs()?['body/Direction/Value']` is equal to `Advisor → Customer`
2. If **No** → **Terminate** (no notification needed for internal or customer→advisor messages)

### Step 3 — Get the Case Record

1. Add **Get items** (SharePoint):
   - List: `Specialist Lending Cases`
   - Filter Query: `CaseID eq '@{triggerOutputs()?['body/CaseID']}'`
   - Top Count: 1

### Step 4 — Send Notification Email

1. Add **Send an email (V2)** (Office 365 Outlook):

   - **To:** `first(outputs('Get_Case')?['body/value'])?['Applicant1Email']`
   - **Subject:** `New Message from Your Advisor — Case @{triggerOutputs()?['body/CaseID']}`
   - **Body (HTML):**

```html
<div style="font-family: Arial, sans-serif; max-width: 600px;">
    <h2 style="color: #2c3e50;">New Message from Your Advisor</h2>

    <p>Dear @{first(outputs('Get_Case')?['body/value'])?['Applicant1FirstName']},</p>

    <p>Your advisor has sent you a new message regarding your lending
       application <strong>@{triggerOutputs()?['body/CaseID']}</strong>.</p>

    <div style="background: #f8f9fa; padding: 15px; border-left: 4px solid #0078d4;
                margin: 20px 0;">
        <p style="color: #666; font-size: 12px; margin: 0 0 5px 0;">
            @{triggerOutputs()?['body/SentByName']} —
            @{formatDateTime(triggerOutputs()?['body/SentDate'], 'dd MMM yyyy HH:mm')}
        </p>
        <p style="margin: 0;">
            @{triggerOutputs()?['body/MessageBody']}
        </p>
    </div>

    <p style="text-align: center; margin: 20px 0;">
        <a href="https://[tenant].sharepoint.com/sites/lending/portal"
           style="background: #0078d4; color: white; padding: 12px 24px;
                  text-decoration: none; border-radius: 4px;">
            View in Portal →
        </a>
    </p>

    <p style="color: #666; font-size: 12px;">
        You can reply to this message through your customer portal or by
        contacting your advisor directly.
    </p>
</div>
```

### Step 5 — Skip System Messages

Add an additional condition before sending the email:

1. Add a **Condition**: `triggerOutputs()?['body/IsSystemMessage']` is equal to `true`
2. If **Yes** → **Terminate** (system messages don't need a separate email — they are logged for audit only)

> **Note:** Place this check before the email action. The flow should only send emails for human-written advisor messages.

### Step 6 — Error Handling

1. Wrap the email action in a **Scope**
2. Add a parallel failure branch:
   - Send Teams notification to the message sender:
     ```
     ⚠️ Flow 14 Failed — Message Notification
     Case: @{triggerOutputs()?['body/CaseID']}
     Customer may not have received email notification.
     ```

### Step 7 — Test

1. Create a new Messages item with:
   - CaseID: a valid test case
   - Direction: `Advisor → Customer`
   - MessageBody: "Please upload your passport at your earliest convenience."
   - IsSystemMessage: No
2. Wait 30–60 seconds
3. Verify:
   - [ ] Customer receives email with the message content
   - [ ] Advisor name and timestamp are displayed
   - [ ] Portal link is included
   - [ ] Email renders correctly

4. Create a message with Direction = `Internal`
5. Verify:
   - [ ] No email is sent to the customer

6. Create a message with IsSystemMessage = Yes
7. Verify:
   - [ ] No email is sent to the customer

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Email sent for internal messages | Verify the Direction condition checks for exactly `Advisor → Customer` (with Unicode arrow) |
| System messages trigger emails | Ensure the IsSystemMessage check is placed before the Send email action |
| Customer email missing | Verify the Case record has Applicant1Email populated |
| Message body truncated | Multiple lines of text may contain HTML characters — consider using `replace()` to convert newlines to `<br>` |
| Unicode arrow not matching | Ensure Direction choice value uses `→` (U+2192) consistently |
