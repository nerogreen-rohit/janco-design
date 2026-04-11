# Task P1-12: Staff App — Fact-Find Tab (Read-Only Review)

**Phase:** 1 — Fact-Find  
**Story:** As a staff member, I want a Fact-Find tab in the Case Workspace showing a read-only view of the customer's fact-find so that I can review their information  
**Priority:** 🔴 Critical  
**Estimated effort:** 2–3 hours  
**Depends on:** Staff App created (P1-11), Fact-Find Responses list (P1-03), Flow 4 running (P1-07)

---

## Step-by-Step Guide

### Step 1 — Add the Fact-Find Tab to Case Workspace

In the Staff App (Power Apps canvas):

1. Open the Case Detail / Workspace screen
2. Add a **Tab control** (or use a navigation gallery) with tabs:
   - Overview | **Fact-Find** | Documents (future) | Messages (future) | Timeline (future)
3. Create the content area for the Fact-Find tab

### Step 2 — Load Fact-Find Data

When the Fact-Find tab is selected, load the linked Fact-Find Responses item:

```
// Set a context variable with the fact-find record
Set(
    varFactFind,
    LookUp(
        'Fact-Find Responses',
        CaseID = varCurrentCase.CaseID
    )
)
```

### Step 3 — Display Header

```
╔══════════════════════════════════════════════════════════════════╗
║  FACT-FIND                              Status: In Progress      ║
║  Last updated by customer: 09 Apr 2026 14:32                     ║
╚══════════════════════════════════════════════════════════════════╝
```

| Element | Source | Display |
|---|---|---|
| Status badge | `varFactFind.Status` | Color-coded: Not Started (grey), In Progress (amber), Submitted (blue), Reviewed (green) |
| Last updated | `varFactFind.LastUpdatedByCustomer` | Formatted date/time |

### Step 4 — Section Selector

Add a **Dropdown** to switch between fact-find sections:

- Choices: `Personal Details`, `Employment & Income`, `Property Details`, `Portfolio`, `Company / SPV`, `Loan Requirements`, `Bridging / Development`, `Additional Info`
- Default: `Personal Details`

### Step 5 — Section Display (Read-Only)

For each section, display fields in a two-column layout (Applicant 1 | Applicant 2). All fields are **read-only** (DisplayMode.View).

#### Personal Details Section

```
╔══════════════════════════════════════════════════════════════════╗
║  APPLICANT 1                     APPLICANT 2                     ║
║  ─────────────────────────       ─────────────────────────       ║
║  Title:    Mr                    Title:    Mrs                    ║
║  Name:     John Doe              Name:     Jane Doe              ║
║  DOB:      12/03/1978            DOB:      04/09/1981            ║
║  NI:       AB123456C             NI:       CD789012E             ║
║  Marital:  Married               Marital:  Married               ║
║  Status:   Owner Occupier        Status:   Owner Occupier        ║
║  Address:  12 High Street        Address:  12 High Street        ║
║            London SW1A 1AA                 London SW1A 1AA       ║
║  Phone:    07700 900123          Phone:    07700 900456          ║
║  Email:    john@email.com        Email:    jane@email.com        ║
╚══════════════════════════════════════════════════════════════════╝
```

Use Label controls bound to `varFactFind.A1FirstName`, `varFactFind.A1LastName`, etc.

#### Employment & Income Section

Display: Employment status, employer, job title, salary, self-employment details — for both applicants.

#### Property Details Section

Display: Address, type, tenure, bedrooms, bathrooms, value, rental income, EPC, HMO flag.

#### Portfolio Section

Display: IsPortfolioLandlord, total properties/value/mortgage/rental, details.

#### Company / SPV Section

Display: Company name, number, type, incorporation date, directors.

#### Loan Requirements Section

Display: Amount, purpose, repayment type, term, adverse credit flag.

#### Bridging / Development Section

Display: Exit strategy, auction date, works details, build cost, GDV.

#### Additional Info Section

Display: Solicitor details, declaration status, special circumstances.

### Step 6 — Action Buttons

Add buttons at the bottom of the Fact-Find tab:

| Button | Action | Visibility |
|---|---|---|
| **[Send Reminder to Customer]** | Triggers a reminder email with the fact-find link | Always visible |
| **[Copy Portal Link]** | Copies the guest-shared fact-find link to clipboard | Always visible |
| **[Mark Reviewed]** | Sets Status = "Reviewed" on the Fact-Find item | Only when Status ≠ "Reviewed" |

#### Mark Reviewed Button

```
// OnSelect for [Mark Reviewed]
Patch(
    'Fact-Find Responses',
    varFactFind,
    {
        Status: {Value: "Reviewed"}
    }
);
Notify("Fact-find marked as Reviewed.", NotificationType.Success);
Set(varFactFind, LookUp('Fact-Find Responses', CaseID = varCurrentCase.CaseID))
```

#### Copy Portal Link Button

```
// OnSelect for [Copy Portal Link]
Copy(varCurrentCase.FactFindGuestLink);
Notify("Fact-find link copied to clipboard.", NotificationType.Information)
```

### Step 7 — Test

1. Create a case and have the "customer" fill in some fact-find fields
2. Open the case in the Staff App → Fact-Find tab
3. Verify:
   - [ ] Status badge shows correct status (In Progress / Submitted)
   - [ ] Last updated timestamp is correct
   - [ ] All sections display data correctly
   - [ ] Section dropdown switches between sections
   - [ ] Fields are read-only (staff cannot edit customer data)
   - [ ] [Mark Reviewed] sets Status = "Reviewed"
   - [ ] [Copy Portal Link] copies the correct URL
   - [ ] [Send Reminder] sends a reminder email to the customer

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Fact-Find data not loading | Verify CaseID matches between Cases and Fact-Find Responses lists |
| Fields show blank despite customer data | Check column internal names match the reference in the formula |
| Mark Reviewed doesn't persist | Verify Patch is targeting the correct item; refresh the variable after Patch |
