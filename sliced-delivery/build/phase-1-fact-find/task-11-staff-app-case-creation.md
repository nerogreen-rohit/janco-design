# Task P1-11: Staff App — Case Creation Screen

**Phase:** 1 — Fact-Find  
**Story:** As a staff member, I want a simple case creation form so that I can create a new case and trigger the automated setup  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** Cases list (P1-01), all 3 flows operational (P1-06, P1-07, P1-08)

---

## Step-by-Step Guide

### Step 1 — Create the Power App (or Use SharePoint Custom Form)

**Option A — Power Apps (recommended for Staff App):**

1. Open **Power Apps** → **+ Create** → **Canvas app** → Tablet layout
2. Name: `Specialist Lending Staff App`
3. Connect to data source: SharePoint → `Specialist Lending Cases` list

**Option B — SharePoint List Form Customization:**

1. Open the Cases list → click **Integrate** → **Power Apps** → **Customize forms**
2. This opens the default SharePoint list form in Power Apps for customization

### Step 2 — Design the Case Creation Form

Create a new screen or form with the following fields:

#### Required Fields

| Field | Control Type | Data Source Column | Default Value |
|---|---|---|---|
| Product Type | Dropdown | ProductType | — (must select) |
| Applicant 1 First Name | Text Input | Applicant1FirstName | — |
| Applicant 1 Last Name | Text Input | Applicant1LastName | — |
| Applicant 1 Email | Text Input | Applicant1Email | — |
| Applicant 1 Phone | Text Input | Applicant1Phone | — |
| Property Address | Text Input (multiline) | PropertyAddress | — |
| Property Postcode | Text Input | PropertyPostcode | — |
| Loan Amount Required | Text Input (number) | LoanAmountRequired | — |
| Case Owner | Person Picker / Combo Box | CaseOwner | `User()` (current user) |
| Enquiry Date | Date Picker | EnquiryDate | `Today()` |

#### Optional Fields

| Field | Control Type | Data Source Column |
|---|---|---|
| Applicant 2 First Name | Text Input | Applicant2FirstName |
| Applicant 2 Last Name | Text Input | Applicant2LastName |
| Applicant 2 Email | Text Input | Applicant2Email |
| Property Type | Dropdown | PropertyType |
| Estimated Value | Text Input (number) | EstimatedValue |

### Step 3 — Form Layout

```
╔══════════════════════════════════════════════════════════════════╗
║  CREATE NEW CASE                                                  ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  Product Type:      [BTL ▼]                                        ║
║                                                                    ║
║  ── APPLICANT 1 ──────────────────────────────────────────────── ║
║  First Name:        [____________]                                 ║
║  Last Name:         [____________]                                 ║
║  Email:             [____________]                                 ║
║  Phone:             [____________]                                 ║
║                                                                    ║
║  ── APPLICANT 2 (Optional) ──────────────────────────────────── ║
║  First Name:        [____________]                                 ║
║  Last Name:         [____________]                                 ║
║  Email:             [____________]                                 ║
║                                                                    ║
║  ── PROPERTY ─────────────────────────────────────────────────── ║
║  Address:           [____________]                                 ║
║  Postcode:          [____________]                                 ║
║  Property Type:     [Residential ▼]                                ║
║                                                                    ║
║  ── LOAN ──────────────────────────────────────────────────────  ║
║  Loan Amount:       [£___________]                                 ║
║                                                                    ║
║  ── ASSIGNMENT ─────────────────────────────────────────────── ║
║  Case Owner:        [Current User ▼]                               ║
║  Enquiry Date:      [Today        ]                                ║
║                                                                    ║
║  [Cancel]                                         [Create Case →] ║
╚══════════════════════════════════════════════════════════════════╝
```

### Step 4 — Submit Action

On the **[Create Case →]** button's `OnSelect` property:

```
// Validate required fields
If(
    IsBlank(ddProductType.Selected.Value) ||
    IsBlank(txtApplicant1FirstName.Text) ||
    IsBlank(txtApplicant1LastName.Text) ||
    IsBlank(txtApplicant1Email.Text) ||
    IsBlank(txtPropertyAddress.Text) ||
    IsBlank(txtPropertyPostcode.Text) ||
    IsBlank(txtLoanAmount.Text),
    
    // Show validation error
    Notify("Please complete all required fields.", NotificationType.Error),
    
    // Create the case
    Patch(
        'Specialist Lending Cases',
        Defaults('Specialist Lending Cases'),
        {
            ProductType: {Value: ddProductType.Selected.Value},
            Applicant1FirstName: txtApplicant1FirstName.Text,
            Applicant1LastName: txtApplicant1LastName.Text,
            Applicant1Email: txtApplicant1Email.Text,
            Applicant1Phone: txtApplicant1Phone.Text,
            Applicant2FirstName: txtApplicant2FirstName.Text,
            Applicant2LastName: txtApplicant2LastName.Text,
            Applicant2Email: txtApplicant2Email.Text,
            PropertyAddress: txtPropertyAddress.Text,
            PropertyPostcode: txtPropertyPostcode.Text,
            LoanAmountRequired: Value(txtLoanAmount.Text),
            CaseOwner: cmbCaseOwner.Selected,
            EnquiryDate: dpEnquiryDate.SelectedDate,
            CaseStatus: {Value: "Active"}
        }
    );
    
    // Show success and navigate
    Notify("Case created! CaseID will be generated automatically.", NotificationType.Success);
    Navigate(CaseListScreen)
)
```

### Step 5 — Test

1. Open the Staff App
2. Click **Create New Case**
3. Fill in all required fields and click **[Create Case →]**
4. Verify:
   - [ ] Case record appears in the Cases list
   - [ ] CaseID is generated within 30 seconds (Flow 3)
   - [ ] Fact-Find item is created (Flow 4)
   - [ ] Folder structure is created (Flow 5)
   - [ ] Welcome email is sent to customer (Flow 5)
   - [ ] Validation prevents submission with empty required fields

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Person picker doesn't show users | Ensure the data source is connected; use `Office365Users.SearchUser()` for the combo box |
| Patch fails with delegation error | Ensure the list is within delegation limits (2000 items default) |
| Flows don't trigger after case creation | Verify the Patch creates the item in the correct list; check flow trigger conditions |
