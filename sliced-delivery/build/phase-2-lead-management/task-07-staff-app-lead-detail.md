# Task P2-07: Staff App — Lead Detail & Convert to Case

**Phase:** 2 — Lead Management  
**Story:** As a staff member, I want a Lead detail/edit form and a [Convert to Case] button so that I can manage leads and convert qualified ones  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** Leads list (P2-01), Flow 2 (P2-05), Leads screen (P2-06)

---

## Step-by-Step Guide

### Step 1 — Create Lead Detail Screen

Add a new screen in the Staff App: **Lead Detail Screen**

Receives: `varSelectedLead` from the Leads gallery.

### Step 2 — Edit Form

Add an **Edit Form** connected to the Leads list, with Item = `varSelectedLead`.

#### Form Fields

| Field | Mode | Notes |
|---|---|---|
| LeadID | View (read-only) | Auto-generated |
| FirstName | Edit | |
| LastName | Edit | |
| Email | Edit | |
| Phone | Edit | |
| ProductInterest | Edit | Dropdown |
| LoanAmountRequired | Edit | |
| PropertyAddress | Edit | Multiline |
| LeadSource | Edit | Dropdown |
| ReferrerID | Edit | Lookup dropdown |
| LeadStatus | Edit | Dropdown |
| AssignedTo | Edit | Person picker |
| Notes | Edit | Multiline |
| ConvertedToCaseID | View (read-only) | Only visible if converted |
| ConvertedDate | View (read-only) | Only visible if converted |

### Step 3 — Save Button

```
// [Save Changes] button OnSelect
SubmitForm(frmLeadDetail);
Notify("Lead saved.", NotificationType.Success);
```

### Step 4 — Status Pipeline Buttons (Optional UX Enhancement)

Add quick-action buttons to move the lead through the pipeline:

```
[Mark Contacted]  [Mark Qualified]  [Mark Lost]  [Mark Dormant]
```

Each button patches the LeadStatus:

```
// [Mark Contacted] OnSelect
Patch(Leads, varSelectedLead, {LeadStatus: {Value: "Contacted"}});
Set(varSelectedLead, LookUp(Leads, ID = varSelectedLead.ID));
Notify("Lead marked as Contacted.", NotificationType.Success);
```

### Step 5 — [Convert to Case] Button

This is the key button. It should:
- **Only be visible** when `LeadStatus = "Qualified"`
- **Trigger Flow 2** (Convert Lead to Case)

```
// Visibility
Visible = varSelectedLead.LeadStatus.Value = "Qualified"
```

```
// OnSelect
Set(varIsConverting, true);

// Run Flow 2 with Lead ID parameter
Set(varConvertResult, 
    'Flow2-ConvertLeadToCase'.Run(varSelectedLead.ID)
);

Set(varIsConverting, false);

If(
    varConvertResult.success,
    Notify(
        "Lead converted to case " & varConvertResult.caseid & "!",
        NotificationType.Success
    );
    Navigate(CaseDetailScreen, ScreenTransition.None, 
        {varCurrentCase: LookUp('Specialist Lending Cases', CaseID = varConvertResult.caseid)}
    ),
    Notify("Conversion failed: " & varConvertResult.message, NotificationType.Error)
);
```

### Step 6 — Loading Indicator

While converting, show a loading spinner:

```
// Spinner visibility
Visible = varIsConverting
```

Display text: "Converting lead to case... This may take up to 60 seconds."

### Step 7 — New Lead Form

Create a **New Lead Screen** with a blank form:

```
// [+ New Lead] button navigates here
// On Submit:
SubmitForm(frmNewLead);
Notify("Lead created! LeadID will be generated automatically.", NotificationType.Success);
Navigate(LeadsScreen);
```

Required fields for new lead:
- FirstName, LastName, Email
- ProductInterest
- AssignedTo (default: current user)

### Step 8 — Test

1. Create a new lead from the [+ New Lead] form
2. Verify LeadID is generated
3. Open the lead detail → update status to "Contacted" → save
4. Update status to "Qualified" → [Convert to Case] button appears
5. Click [Convert to Case]
6. Verify:
   - [ ] Loading indicator shows during conversion
   - [ ] Case record created with lead data
   - [ ] Navigates to the new case detail screen
   - [ ] Lead shows ConvertedToCaseID and Status = "Converted"
   - [ ] Lead no longer visible in Active Leads filter
   - [ ] Customer receives welcome email

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| [Convert to Case] doesn't appear | Verify LeadStatus is exactly "Qualified" (case-sensitive) |
| Flow run fails | Check Power Automate run history; ensure Flow 2 has correct permissions |
| Conversion takes too long | Flow 2 includes a 60-second wait for downstream flows — this is expected |
| Navigation to case fails | The case may not have CaseID yet — add retry logic or delay before navigation |
