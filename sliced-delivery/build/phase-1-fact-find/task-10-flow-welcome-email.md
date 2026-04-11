# Task P1-10: Welcome Email Template

**Phase:** 1 — Fact-Find  
**Story:** As a system, once all case setup flows complete I need to send a welcome email to the customer with their fact-find link and document upload links  
**Priority:** 🔴 Critical  
**Estimated effort:** 30 minutes  
**Depends on:** Flow 4 (P1-07) and Flow 5 (P1-08) complete — fact-find link + folder links available

---

## Step-by-Step Guide

### Step 1 — Determine Where to Send the Email

The welcome email is sent at the end of **Flow 5** (after folder links are generated), since Flow 5 is the last setup flow to complete and has all the required links.

See [task-08-flow-create-folders.md](task-08-flow-create-folders.md) Step 8.

### Step 2 — Add the Email Action in Flow 5

1. At the end of Flow 5 (after folder links are stored on the Case record), add:
2. **Send an email (V2)** (Office 365 Outlook):
   - **To:** `@{triggerOutputs()?['body/Applicant1Email']}`
   - **Subject:** `Welcome to Your Lending Application — @{variables('varCaseID')}`
   - **Importance:** Normal

### Step 3 — Email HTML Template

Use the following HTML body. Replace the placeholder variables with dynamic content from the flow.

```html
<html>
<body style="font-family: Arial, sans-serif; color: #333; max-width: 600px; margin: 0 auto;">
  
  <div style="background-color: #1a365d; color: white; padding: 20px; text-align: center;">
    <h1 style="margin: 0; font-size: 22px;">🏦 Your Lending Application</h1>
    <p style="margin: 5px 0 0; font-size: 14px;">Specialist Lending Hub</p>
  </div>

  <div style="padding: 20px;">
    <p>Dear <strong>@{triggerOutputs()?['body/Applicant1FirstName']}</strong>,</p>

    <p>Thank you for your enquiry. Your case reference is <strong>@{variables('varCaseID')}</strong>.</p>

    <p>To progress your application, we need you to:</p>

    <h3 style="color: #1a365d;">📋 1. Complete Your Fact-Find</h3>
    <p>Please click the link below to provide your personal, employment, property, and loan details:</p>
    <p><a href="@{variables('varFactFindGuestLink')}" 
          style="background-color: #2b6cb0; color: white; padding: 10px 20px; text-decoration: none; border-radius: 5px; display: inline-block;">
          Complete Your Fact-Find →
       </a></p>
    <p style="font-size: 12px; color: #666;">You can save your progress and return to complete it later. No account is required.</p>

    <h3 style="color: #1a365d;">📁 2. Upload Your Documents</h3>
    <p>Please upload the following documents to the secure folders below:</p>
    <ul>
      <li><a href="@{variables('varFolderLink01')}">📂 Identity Documents</a> — Passport, driving licence, proof of address</li>
      <li><a href="@{variables('varFolderLink02')}">📂 Income Documents</a> — Payslips, SA302, accounts</li>
      <li><a href="@{variables('varFolderLink03')}">📂 Property Documents</a> — Mortgage statement, EPC, tenancy agreement</li>
      <li><a href="@{variables('varFolderLink04')}">📂 Bank Statements</a> — Last 6 months personal and/or business</li>
    </ul>

    <hr style="border: none; border-top: 1px solid #e2e8f0; margin: 20px 0;">

    <h3 style="color: #1a365d;">📞 Your Advisor</h3>
    <p>If you have any questions, please contact your advisor:</p>
    <p>
      <strong>@{first(outputs('Get_CaseOwner'))?['DisplayName']}</strong><br>
      📧 <a href="mailto:@{first(outputs('Get_CaseOwner'))?['Email']}">@{first(outputs('Get_CaseOwner'))?['Email']}</a>
    </p>

    <hr style="border: none; border-top: 1px solid #e2e8f0; margin: 20px 0;">

    <p style="font-size: 12px; color: #999;">
      This is an automated message from the Specialist Lending Hub. 
      Your data is stored securely on Microsoft 365. 
      Case reference: @{variables('varCaseID')}.
    </p>
  </div>

</body>
</html>
```

### Step 4 — Dynamic Content Mapping

| Placeholder | Source |
|---|---|
| `Applicant1FirstName` | Case item trigger: `Applicant1FirstName` |
| `varCaseID` | Variable set from Flow 3 result |
| `varFactFindGuestLink` | From Flow 4 output or Case record column `FactFindGuestLink` |
| `varFolderLink01` | Guest share link for 01-Identity (from Step 6 of Flow 5) |
| `varFolderLink02` | Guest share link for 02-Income |
| `varFolderLink03` | Guest share link for 03-Property |
| `varFolderLink04` | Guest share link for 04-Bank-Statements |
| `Get_CaseOwner` | Get item on CaseOwner person field |

### Step 5 — Test

1. Create a new Case item
2. Wait for all flows to complete (up to 90 seconds)
3. Check the customer email inbox:
   - [ ] Email received with subject containing CaseID
   - [ ] Customer name is correct
   - [ ] Fact-find link opens the correct edit form (no M365 login required)
   - [ ] All 4 document upload folder links work
   - [ ] Advisor name and email are correct
4. Check spam/junk folder if not in inbox

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Email not received | Check Applicant1Email is populated; check spam folder; check flow run history |
| Links don't work | Verify guest sharing is enabled at tenant + site level |
| Advisor info missing | Ensure CaseOwner person field is populated on the Case record |
| Email looks broken | Test HTML in an email preview tool; some email clients strip styles |
