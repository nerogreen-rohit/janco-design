# Task P3-20: Staff App — Admin Screens

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a staff member, I want admin screens for Stage Configuration and Document Types so that I can manage reference data without editing lists directly  
**Priority:** 🟢 Medium  
**Estimated effort:** 2–3 hours  
**Depends on:** Stage Configuration list with all 14 stages (P3-07), Document Types list (P3-02), Staff App created (P1-11)

---

## Step-by-Step Guide

### Step 1 — Create the Admin Navigation

1. In the Staff App, add a new screen: **Admin Screen**
2. Add an admin section to the app's main navigation (e.g. gear icon or "Admin" menu item)
3. Only visible to users with admin/manager role

```
╔══════════════════════════════════════════════════════════════════════════╗
║  ADMIN                                                                  ║
╠══════════════════════════════════════════════════════════════════════════╣
║                                                                         ║
║  [Stage Configuration]       [Document Types]                           ║
║                                                                         ║
╚══════════════════════════════════════════════════════════════════════════╝
```

**Admin visibility (restrict to admins):**
```
// Show admin navigation only for specific users
User().Email in ["admin1@tenant.com", "admin2@tenant.com"]
```

> **Note:** For production, use a SharePoint group or security role instead of hardcoded emails.

### Step 2 — Stage Configuration Admin Screen

```
╔══════════════════════════════════════════════════════════════════════════╗
║  STAGE CONFIGURATION                                     [← Back]      ║
╠══════════════════════════════════════════════════════════════════════════╣
║                                                                         ║
║  #  │ Stage Name                │ Short │ SLA │ Visible │ Skip │ Active║
║  ───┼───────────────────────────┼───────┼─────┼─────────┼──────┼────── ║
║   1 │ Customer Enquiry          │ ENQ   │  1  │  Yes    │  No  │ ✅   ║
║   2 │ Fact-Find                 │ FACT  │  5  │  Yes    │  No  │ ✅   ║
║   3 │ Terms of Business & GDPR  │ TOB   │  2  │  Yes    │  No  │ ✅   ║
║   4 │ Product Research          │ PROD  │  3  │  No     │  No  │ ✅   ║
║   5 │ Present Options & Agree   │ OPT   │  3  │  Yes    │  Yes │ ✅   ║
║   6 │ Produce Illustrations     │ ILLUS │  3  │  Yes    │  Yes │ ✅   ║
║   7 │ Decision in Principle     │ DIP   │  5  │  Yes    │  Yes │ ✅   ║
║   8 │ Full Application          │ APP   │  7  │  Yes    │  No  │ ✅   ║
║   9 │ Valuation                 │ VAL   │ 10  │  Yes    │  No  │ ✅   ║
║  10 │ Underwriting              │ UW    │ 14  │  Yes    │  No  │ ✅   ║
║  11 │ Loan Offer                │ OFFER │  5  │  Yes    │  No  │ ✅   ║
║  12 │ Legals                    │ LEGAL │ 21  │  Yes    │  No  │ ✅   ║
║  13 │ Completion                │ COMP  │  1  │  Yes    │  No  │ ✅   ║
║  14 │ Post-Completion & Logging │ POST  │  3  │  Yes    │  No  │ ✅   ║
║                                                                         ║
║  [Click any row to edit]                                                ║
╚══════════════════════════════════════════════════════════════════════════╝
```

### Step 3 — Stage Configuration Gallery

1. Add a **Gallery** control
2. Set **Items**: `Sort('Stage Configuration', StageNumber, SortOrder.Ascending)`

### Step 4 — Stage Edit Form

When a stage row is clicked, show an edit form:

```
╔════════════════════════════════════════════╗
║  EDIT STAGE: Product Research              ║
║                                            ║
║  Stage Name:        [Product Research    ] ║
║  Short Name:        [PROD               ] ║
║  Customer Name:     [Under Review        ] ║
║  Stage Number:      [4                   ] ║
║  Default SLA Days:  [3                   ] ║
║  Visible to Cust:   [No  ▼]               ║
║  Can Skip:          [No  ▼]               ║
║  Is Active:         [✅]                   ║
║                                            ║
║  [Cancel]                   [Save →]       ║
╚════════════════════════════════════════════╝
```

**[Save →] OnSelect:**
```
Patch(
    'Stage Configuration',
    galStages.Selected,
    {
        Title: txtStageName.Text,
        ShortName: txtShortName.Text,
        CustomerFriendlyName: txtCustomerName.Text,
        DefaultSLADays: Value(txtSLADays.Text),
        VisibleToCustomer: If(ddVisible.Selected.Value = "Yes", true, false),
        CanSkip: If(ddCanSkip.Selected.Value = "Yes", true, false),
        IsActive: tglIsActive.Value
    }
);
Notify("Stage updated.", NotificationType.Success);
UpdateContext({showStageEdit: false})
```

> **Warning:** Changing stage names does NOT affect existing Stage History records (they store text snapshots). However, it will change the display in the progress bar and Stage Configuration for future use.

### Step 5 — IsActive Toggle

The IsActive toggle allows deactivating a stage:

- **Use case:** If a stage is no longer needed, toggle IsActive to No
- **Effect:** The stage will still appear in historical data but won't be available for new stage transitions
- **Caution:** Deactivating core stages (1–4, 8–14) is not recommended

### Step 6 — Document Types Admin Screen

```
╔══════════════════════════════════════════════════════════════════════════╗
║  DOCUMENT TYPES                                          [← Back]      ║
╠══════════════════════════════════════════════════════════════════════════╣
║                                                                         ║
║  [+ Add Document Type]                  Filter: [All Categories ▼]     ║
║                                                                         ║
║  Document             │ Category │ ApplicableTo      │ Required │ Active║
║  ─────────────────────┼──────────┼───────────────────┼──────────┼────── ║
║  Passport             │ Identity │ All Products      │ Yes      │ ✅   ║
║  Driving Licence      │ Identity │ All Products      │ Yes      │ ✅   ║
║  Utility Bill (3 mon) │ Identity │ All Products      │ Yes      │ ✅   ║
║  Council Tax Bill     │ Identity │ All Products      │ No       │ ✅   ║
║  Last 3 Mon Payslips  │ Income   │ All Products      │ Yes      │ ✅   ║
║  ...                  │ ...      │ ...               │ ...      │ ...  ║
╚══════════════════════════════════════════════════════════════════════════╝
```

### Step 7 — Document Types Gallery

1. Add a **Gallery** control
2. Set **Items**:

```
Sort(
    Filter(
        'Document Types',
        ddCategoryFilter.Selected.Value = "All" ||
        Category.Value = ddCategoryFilter.Selected.Value
    ),
    SortOrder,
    SortOrder.Ascending
)
```

**Category filter dropdown Items:**
```
["All", "Identity", "Income", "Property", "Legal", "Lender", "Other"]
```

### Step 8 — Document Type Add/Edit Form

```
╔════════════════════════════════════════════╗
║  ADD / EDIT DOCUMENT TYPE                  ║
║                                            ║
║  Title:        [______________________]    ║
║  Category:     [Identity ▼]                ║
║  ApplicableTo: [☑ All  ☐ BTL  ☐ Bridge]   ║
║  Description:  [______________________]    ║
║                [______________________]    ║
║  Is Required:  [✅]                        ║
║  Sort Order:   [5                    ]     ║
║  Is Active:    [✅]                        ║
║                                            ║
║  [Cancel]                   [Save →]       ║
╚════════════════════════════════════════════╝
```

**[+ Add Document Type] OnSelect:**
```
UpdateContext({
    showDocTypeForm: true,
    editDocType: Defaults('Document Types')
})
```

**[Save →] OnSelect (Add):**
```
Patch(
    'Document Types',
    Defaults('Document Types'),
    {
        Title: txtDocTitle.Text,
        Category: {Value: ddCategory.Selected.Value},
        ApplicableTo: ddApplicableTo.SelectedItems,
        Description: txtDescription.Text,
        IsRequired: tglRequired.Value,
        SortOrder: Value(txtSortOrder.Text),
        IsActive: tglActive.Value
    }
);
Notify("Document type added.", NotificationType.Success);
UpdateContext({showDocTypeForm: false})
```

**[Save →] OnSelect (Edit):**
```
Patch(
    'Document Types',
    galDocTypes.Selected,
    {
        Title: txtDocTitle.Text,
        Category: {Value: ddCategory.Selected.Value},
        ApplicableTo: ddApplicableTo.SelectedItems,
        Description: txtDescription.Text,
        IsRequired: tglRequired.Value,
        SortOrder: Value(txtSortOrder.Text),
        IsActive: tglActive.Value
    }
);
Notify("Document type updated.", NotificationType.Success);
UpdateContext({showDocTypeForm: false})
```

### Step 9 — Deactivate Instead of Delete

Document types should be **deactivated** (IsActive = No), never deleted:

- Existing Document Requests may reference the type
- Historical records must remain intact
- The toggle sets IsActive to No, hiding the type from future use

### Step 10 — Test

1. Navigate to Admin → Stage Configuration
2. Verify:
   - [ ] All 14 stages are displayed in order
   - [ ] Clicking a stage opens the edit form
   - [ ] Editing SLA days and saving persists the change
   - [ ] IsActive toggle works
3. Navigate to Admin → Document Types
4. Verify:
   - [ ] All 25 document types are displayed
   - [ ] Category filter narrows the list
   - [ ] [+ Add Document Type] opens a blank form
   - [ ] Adding a new type creates an item in the list
   - [ ] Clicking a type opens the edit form with pre-filled values
   - [ ] Editing and saving persists changes
   - [ ] IsActive toggle deactivates (does not delete)
   - [ ] Deactivated types don't appear in the [+ Request Document] dropdown (P3-17)
5. Test admin access:
   - [ ] Non-admin users cannot see the Admin navigation

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Admin screens visible to all users | Verify the visibility condition checks user email or security group membership |
| Stage edit not saving | Check Patch formula targets the correct item; verify column internal names |
| Document type appears in requests after deactivation | Verify the Document Request form filters by `IsActive = true` |
| ApplicableTo multi-select not working | Use a `ComboBox` control with `SelectMultiple: true` instead of a Dropdown |
| Sort order conflicts | Assign unique SortOrder values; consider auto-incrementing when adding new types |
