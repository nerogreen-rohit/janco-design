# Task P5-11: Dashboard — Compliance Dashboard

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a compliance officer, I want a Compliance Dashboard showing TOB status, fact-find completion, and overdue documents so that I can identify compliance gaps  
**Priority:** 🟡 High  
**Estimated effort:** 2–3 hours  
**Depends on:** Cases list (P1-01), Fact-Find Responses (P1-03), Document Requests (P3-01), Dashboard Screen Layout (P5-12)

---

## Step-by-Step Guide

### Step 1 — Create the Compliance Dashboard Container

1. Open the **Specialist Lending Staff App** in Power Apps Studio
2. Navigate to the **Dashboard** screen (created in P5-12)
3. Add a container visible when `varDashboardTab = "Compliance"`
4. Name: `conComplianceDashboard`

### Step 2 — Load Data Collections

Add to the Dashboard screen's `OnVisible`:

```
// Active cases
ClearCollect(
    colComplianceCases,
    Filter('Specialist Lending Cases', CaseStatus.Value = "Active")
);

// Fact-Find Responses
ClearCollect(
    colFactFinds,
    'Fact-Find Responses'
);

// Document Requests
ClearCollect(
    colDocRequests,
    'Document Requests'
)
```

### Step 3 — TOB (Terms of Business) Status

Cases at Stage 3 or beyond should have a signed TOB. Calculate status:

```
Set(varTOBSigned,
    CountRows(Filter(colComplianceCases, CurrentStageNumber >= 3 && TOBSigned = true))
);
Set(varTOBPending,
    CountRows(Filter(colComplianceCases, CurrentStageNumber >= 3 && (TOBSigned = false || IsBlank(TOBSigned))))
);
Set(varTOBNotIssued,
    CountRows(Filter(colComplianceCases, CurrentStageNumber < 3))
)
```

> **Note:** If `TOBSigned` is not a column on Cases, adapt to check whether the case has progressed past Stage 3 (which requires TOB).

Display as three metric cards:

| Card | Colour | Value | Description |
|---|---|---|---|
| ✅ TOB Signed | Green | `varTOBSigned` | Signed and compliant |
| ⏳ TOB Pending | Amber | `varTOBPending` | At Stage 3+ but not yet signed |
| ⬜ Not Issued | Grey | `varTOBNotIssued` | Pre-Stage 3 — TOB not yet required |

### Step 4 — Fact-Find Completion Rates

```
Set(varFFComplete,
    CountRows(Filter(colFactFinds, Status.Value = "Complete"))
);
Set(varFFInProgress,
    CountRows(Filter(colFactFinds, Status.Value = "In Progress"))
);
Set(varFFNotStarted,
    CountRows(Filter(colFactFinds, Status.Value = "Not Started" || IsBlank(Status)))
)
```

Display as three metric cards:

| Card | Colour | Value |
|---|---|---|
| ✅ Complete | Green | `varFFComplete` |
| 🔄 In Progress | Blue | `varFFInProgress` |
| ⬜ Not Started | Grey | `varFFNotStarted` |

### Step 5 — Overdue Document Requests

Find document requests that are past their due date and not yet received:

```
ClearCollect(
    colOverdueDocuments,
    Filter(
        colDocRequests,
        Status.Value <> "Received" &&
        Status.Value <> "Not Required" &&
        DueDate < Today()
    )
)
```

1. Add a count metric card:
   - Header: `"Overdue Documents"`
   - Value: `CountRows(colOverdueDocuments)`
   - Colour: Red if > 0, green if 0

2. Add a **Gallery** listing overdue documents:
   - Name: `galOverdueDocuments`
   - Items: `Sort(colOverdueDocuments, DueDate, SortOrder.Ascending)`
   - Template:
     - CaseID
     - Document Name
     - Due Date: `Text(ThisItem.DueDate, "dd/MM/yyyy")`
     - Days Overdue: `DateDiff(ThisItem.DueDate, Today(), TimeUnit.Days) & " days"`

### Step 6 — Cases with Adverse Credit

```
ClearCollect(
    colAdverseCredit,
    Filter(
        colFactFinds,
        AdverseCredit = true || AdverseCredit_Applicant2 = true
    )
)
```

Display:
- Metric card: `CountRows(colAdverseCredit) & " cases with adverse credit flagged"`
- Optional gallery listing case details for review

### Step 7 — Wireframe Reference

```
╔══════════════════════════════════════════════════════════════════════╗
║  DASHBOARDS                                                          ║
║  [Pipeline] [Performance] [Commission] [Compliance]                   ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                       ║
║  TOB STATUS                               FACT-FIND COMPLETION       ║
║  ┌──────────────────────────────┐        ┌─────────────────────┐    ║
║  │ ✅ Signed      28           │        │ ✅ Complete    34    │    ║
║  │ ⏳ Pending      5           │        │ 🔄 In Progress  8    │    ║
║  │ ⬜ Not Issued  14           │        │ ⬜ Not Started  5    │    ║
║  └──────────────────────────────┘        └─────────────────────┘    ║
║                                                                       ║
║  OVERDUE DOCUMENTS (6)                    ADVERSE CREDIT             ║
║  ┌──────────────────────────────────┐    ┌─────────────────────┐    ║
║  │ BTL-0003 │ Bank Stmts │ 5 days  │    │ 3 cases flagged     │    ║
║  │ BTL-0007 │ ID Proof   │ 3 days  │    │                     │    ║
║  │ BRG-0012 │ Valuation  │ 12 days │    │ BTL-0003 (App 1)    │    ║
║  │ COM-0002 │ Accounts   │ 8 days  │    │ BRG-0012 (App 1)    │    ║
║  │ DEV-0001 │ Plans      │ 2 days  │    │ COM-0002 (App 2)    │    ║
║  │ BTL-0015 │ EPC Cert   │ 1 day   │    │                     │    ║
║  └──────────────────────────────────┘    └─────────────────────┘    ║
╚══════════════════════════════════════════════════════════════════════╝
```

### Step 8 — Verify

- [ ] Compliance Dashboard shows TOB status (Signed / Pending / Not Issued) counts
- [ ] TOB counts match the Cases list when filtered manually
- [ ] Fact-Find completion shows Complete / In Progress / Not Started counts
- [ ] Fact-Find counts match the Fact-Find Responses list Status values
- [ ] Overdue Document Requests list shows only documents past due date and not received
- [ ] Days Overdue calculation is correct
- [ ] Adverse Credit count matches Fact-Find Responses with AdverseCredit = true
- [ ] All metrics refresh when navigating to the Compliance tab

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| TOB status shows all "Not Issued" | Verify `CurrentStageNumber` is populated on Case records; check the threshold (Stage 3) is correct |
| Fact-Find counts don't add up | Check Status choice values on the Fact-Find Responses list match exactly: "Complete", "In Progress", "Not Started" |
| Overdue documents list empty when there should be items | Verify `DueDate` is populated and `Status` filter excludes "Received" and "Not Required" correctly |
| Adverse Credit count is 0 | Check the column name — it may be `HasAdverseCredit` or `AdverseCredit`; verify it's a Yes/No field |
| Slow loading | Load only active cases (not all cases); use `ShowColumns` to fetch fewer fields |
