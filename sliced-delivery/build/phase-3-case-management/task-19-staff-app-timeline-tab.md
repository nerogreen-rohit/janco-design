# Task P3-19: Staff App — Timeline Tab

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a staff member, I want a Timeline tab showing the complete stage history so that I can review the full audit trail for a case  
**Priority:** 🟡 High  
**Estimated effort:** 1–2 hours  
**Depends on:** Case Stage History list (P3-01), Flow 6 — Log Stage Change (P3-08), Case Workspace screen (P3-16)

---

## Step-by-Step Guide

### Step 1 — Add the Timeline Tab

1. In the Case Workspace screen, add the Timeline tab content area
2. This tab is visible when the user selects "Timeline" from the tab navigation
3. **All controls on this tab are read-only** — no edit capability

### Step 2 — Screen Layout

```
╔══════════════════════════════════════════════════════════════════════════╗
║  TIMELINE — BTL-2026-0001                                               ║
╠══════════════════════════════════════════════════════════════════════════╣
║                                                                         ║
║  ● Stage 7: Decision in Principle                                       ║
║  │ 22 Apr 2026 10:15  •  Changed by: Alice Brown                      ║
║  │ Time in previous stage: 48.5 hours                                  ║
║  │ Notes: DIP submitted to TML, reference DIP-12345                    ║
║  │                                                                      ║
║  ● Stage 6: Produce Illustrations                ⚠️ SKIPPED            ║
║  │ 20 Apr 2026 14:30  •  Changed by: Alice Brown                      ║
║  │ Time in previous stage: 24.0 hours                                  ║
║  │ Notes: SKIPPED: Bridging product — illustrations not required       ║
║  │                                                                      ║
║  ● Stage 5: Present Options & Agree                                     ║
║  │ 19 Apr 2026 14:30  •  Changed by: Alice Brown                      ║
║  │ Time in previous stage: 36.2 hours                                  ║
║  │ Notes: Customer agreed to TML 5-year fixed at 5.99%                 ║
║  │                                                                      ║
║  ● Stage 4: Product Research                                            ║
║  │ 18 Apr 2026 09:00  •  Changed by: Alice Brown                      ║
║  │ Time in previous stage: 72.0 hours                                  ║
║  │ Notes: Researched 5 lenders, TML best fit                           ║
║  │                                                                      ║
║  ● Stage 3: Terms of Business & GDPR                                    ║
║  │ 15 Apr 2026 09:00  •  Changed by: System                           ║
║  │ Time in previous stage: 120.0 hours                                 ║
║  │                                                                      ║
║  ● Stage 2: Fact-Find                                                   ║
║  │ 10 Apr 2026 09:00  •  Changed by: System                           ║
║  │ Time in previous stage: 0.5 hours                                   ║
║  │                                                                      ║
║  ● Stage 1: Customer Enquiry                                            ║
║    10 Apr 2026 08:45  •  Changed by: Alice Brown                       ║
║                                                                         ║
╚══════════════════════════════════════════════════════════════════════════╝
```

### Step 3 — Timeline Gallery

1. Add a **Gallery** control (vertical, flexible height)
2. Set **Items**:

```
Sort(
    Filter(
        'Case Stage History',
        CaseID = varCurrentCase.CaseID
    ),
    ChangedDate,
    SortOrder.Descending
)
```

### Step 4 — Timeline Entry Template

Inside each gallery item, add the following elements:

| Element | Text Property | DisplayMode |
|---|---|---|
| Stage indicator (●) | `"●"` — circle icon | View |
| Connector line (│) | Vertical line between entries | View |
| Stage label | `"Stage " & ThisItem.StageNumber & ": " & ThisItem.StageName` | View |
| Skipped badge | `If(ThisItem.IsSkipped, "⚠️ SKIPPED", "")` | View |
| Date and author | `Text(ThisItem.ChangedDate, "dd MMM yyyy HH:mm") & "  •  Changed by: " & ThisItem.ChangedBy.DisplayName` | View |
| Duration | `If(ThisItem.TimeInPreviousStageHours > 0, "Time in previous stage: " & Text(ThisItem.TimeInPreviousStageHours, "#,##0.0") & " hours", "")` | View |
| Notes | `ThisItem.Notes` | View |

### Step 5 — Styling

**Stage indicator color:**
```
If(
    ThisItem.IsSkipped,
    RGBA(255, 193, 7, 1),        // Yellow for skipped
    RGBA(0, 120, 212, 1)         // Blue for normal
)
```

**Skipped badge styling:**
- Background: `RGBA(255, 243, 205, 1)` (light yellow)
- Text color: `RGBA(133, 100, 4, 1)` (dark amber)
- Font weight: Bold
- Visible: `ThisItem.IsSkipped`

**Notes area:**
- Visible only when Notes is not blank: `!IsBlank(ThisItem.Notes)`
- Font style: Italic for skipped entries

### Step 6 — Empty State

If no history records exist for the case, show a message:

```
// Visible when gallery is empty
If(
    CountRows(
        Filter(
            'Case Stage History',
            CaseID = varCurrentCase.CaseID
        )
    ) = 0,
    true,
    false
)
```

**Message:** `"No stage history recorded yet. History will appear as the case progresses through stages."`

### Step 7 — Verify Read-Only

Ensure no controls on this tab allow editing:

- [ ] All labels use `DisplayMode.View`
- [ ] No text inputs, dropdowns, or buttons that modify data
- [ ] No "Edit" or "Delete" actions are available
- [ ] Gallery is set to non-selectable if no navigation is needed

> **Design note:** Stage History is an immutable audit log. The Staff App must never expose edit or delete capability for these records.

### Step 8 — Test

1. Open a test case that has been advanced through several stages
2. Navigate to the Timeline tab
3. Verify:
   - [ ] All stage transitions are displayed in reverse chronological order
   - [ ] Stage name matches the text snapshot (not a live lookup)
   - [ ] Changed by shows the correct user name
   - [ ] Time in previous stage shows hours with 1 decimal place
   - [ ] Notes are displayed when present
   - [ ] Skipped entries show ⚠️ SKIPPED badge prominently
   - [ ] Skipped entries show the skip reason in notes
4. Test read-only:
   - [ ] No controls allow editing of any timeline entry
   - [ ] Tapping/clicking entries does not open an edit form
5. Test empty state:
   - [ ] New case with no stage changes shows the empty state message

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Timeline shows no entries | Verify Flow 6 is creating Stage History records when stages change |
| Stage name shows as ID instead of text | Stage History stores text snapshots — verify StageName column is Single line of text (P3-01) |
| Wrong user in ChangedBy | Flow 6 should capture the user who triggered the stage change, not the flow service account |
| Entries out of order | Verify the gallery sorts by ChangedDate descending |
| Duration shows 0 for all entries | Check Flow 6 calculates TimeInPreviousStageHours from StageChangedDate |
