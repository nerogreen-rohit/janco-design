# Task P4-02: Configure the Progress Tracker Web Part

**Phase:** 4 — Customer Portal  
**Story:** As a customer, I want to see my application progress with customer-friendly stage names (✅ ▶ ○) so that I know where my case stands  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** P4-01 (portal page created), Phase 3 (Stage Configuration list populated with all stages)

---

## Step-by-Step Guide

### Step 1 — Understand the Stage Display Rules

The progress tracker displays stages from the Stage Configuration list with these rules:

| Condition | Icon | Meaning |
|---|---|---|
| `StageNumber < CurrentStageNumber` | ✅ | Completed |
| `StageNumber = CurrentStageNumber` | ▶ | Current (YOU ARE HERE) |
| `StageNumber > CurrentStageNumber` | ○ | Future |
| `VisibleToCustomer = No` | *(hidden)* | Not shown to customer |

> **Key UX rule:** Customers see `CustomerFriendlyName` values (e.g. "Information Gathering"), NOT internal `StageName` values (e.g. "Fact-Find"). Stage 4 (Product Research, `VisibleToCustomer = No`) is hidden entirely.

### Step 2 — Create a Customer-Visible Stages View

1. Navigate to the **Stage Configuration** list
2. Click **⚙️** → **List Settings** → **Views** → **Create view**
3. Name: `Customer Progress View`
4. Configure the view:
   - **Columns to show:**
     - `StageNumber`
     - `CustomerFriendlyName`
   - **Filter:** `VisibleToCustomer is equal to Yes`
   - **Sort:** `StageNumber` ascending
5. Click **OK** to save the view

### Step 3 — Add JSON Column Formatting for Status Icons

Apply JSON column formatting to show ✅ / ▶ / ○ icons based on the current stage. This uses a calculated approach comparing each stage's number to the case's `CurrentStageNumber`.

1. Navigate to the **Stage Configuration** list
2. Add a calculated column (or use an existing column for display):
   - Name: `ProgressIcon`
   - Type: Single line of text
3. Click the column header → **Column settings** → **Format this column**
4. Paste the following JSON formatting:

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/column-formatting.schema.json",
  "elmType": "div",
  "style": {
    "display": "flex",
    "align-items": "center",
    "padding": "4px 0",
    "font-size": "14px"
  },
  "children": [
    {
      "elmType": "span",
      "style": {
        "margin-right": "8px",
        "font-size": "16px"
      },
      "txtContent": "=if([$StageNumber] < [$CurrentStageNumber], '✅', if([$StageNumber] == [$CurrentStageNumber], '▶', '○'))"
    },
    {
      "elmType": "span",
      "style": {
        "font-weight": "=if([$StageNumber] == [$CurrentStageNumber], 'bold', 'normal')",
        "color": "=if([$StageNumber] == [$CurrentStageNumber], '#0078d4', if([$StageNumber] < [$CurrentStageNumber], '#107c10', '#666666'))"
      },
      "txtContent": "[$CustomerFriendlyName]"
    },
    {
      "elmType": "span",
      "style": {
        "margin-left": "8px",
        "font-size": "11px",
        "color": "#0078d4",
        "font-weight": "bold"
      },
      "txtContent": "=if([$StageNumber] == [$CurrentStageNumber], '← YOU ARE HERE', '')"
    }
  ]
}
```

> **Note:** The `[$CurrentStageNumber]` reference requires a lookup or embedded parameter approach. See Step 5 for the recommended implementation.

### Step 4 — Add a View Formatting JSON (Recommended Approach)

For a cleaner display, apply view formatting to the entire Customer Progress View. This gives full control over the layout.

1. Open the **Customer Progress View** created in Step 2
2. Click the view dropdown → **Format current view**
3. Choose **Advanced mode**
4. Paste the following JSON:

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/row-formatting.schema.json",
  "hideSelection": true,
  "hideColumnHeader": true,
  "rowFormatter": {
    "elmType": "div",
    "style": {
      "display": "flex",
      "align-items": "center",
      "padding": "6px 12px",
      "border-bottom": "1px solid #f0f0f0"
    },
    "children": [
      {
        "elmType": "span",
        "style": {
          "font-size": "18px",
          "margin-right": "12px",
          "min-width": "24px",
          "text-align": "center"
        },
        "txtContent": "=if(Number([$StageNumber]) < Number([$CurrentStageNumber]), '✅', if(Number([$StageNumber]) == Number([$CurrentStageNumber]), '▶️', '○'))"
      },
      {
        "elmType": "span",
        "style": {
          "font-size": "14px",
          "font-weight": "=if(Number([$StageNumber]) == Number([$CurrentStageNumber]), '600', '400')",
          "color": "=if(Number([$StageNumber]) == Number([$CurrentStageNumber]), '#0078d4', if(Number([$StageNumber]) < Number([$CurrentStageNumber]), '#107c10', '#888888'))"
        },
        "txtContent": "[$CustomerFriendlyName]"
      },
      {
        "elmType": "span",
        "style": {
          "margin-left": "8px",
          "font-size": "11px",
          "color": "#0078d4",
          "font-weight": "600",
          "font-style": "italic"
        },
        "txtContent": "=if(Number([$StageNumber]) == Number([$CurrentStageNumber]), '← YOU ARE HERE', '')"
      }
    ]
  }
}
```

### Step 5 — Embed Progress Tracker in the Portal Page

Because the Stage Configuration list does not directly hold `CurrentStageNumber` for each case, use one of these approaches:

**Option A — Static Embed (Manual per case):**

1. On the customer's portal page (P4-01), add an **Embed** web part
2. Use the HTML template from `imports/portal-page-template.html` which renders the progress tracker statically
3. The progress section updates when the case stage changes (requires manual page edit or a future automated flow)

**Option B — Filtered List View (Dynamic):**

1. Add a **List** web part on the portal page
2. Select the **Stage Configuration** list
3. Select the **Customer Progress View** (from Step 2)
4. Apply the view formatting JSON (from Step 4)
5. The `CurrentStageNumber` comparison requires a lookup from the Cases list. Configure the view to cross-reference using:
   - A calculated column on Stage Configuration that reads from a URL parameter: `?CaseStageNumber=7`
   - Or embed a pre-filtered view URL that includes the current stage info

**Option C — Embed with Dynamic Query (Recommended for future):**

1. Use a SharePoint Framework (SPFx) web part or Power Automate triggered page refresh
2. The flow reads `CurrentStageNumber` from the Cases list and renders the progress tracker dynamically
3. This is the cleanest long-term approach but requires SPFx development

> **Recommendation for Phase 4:** Use Option A (static embed) for the initial build. The HTML template includes all stage data and renders immediately. When the case advances, either manually update the page or implement an automated update flow in Phase 5.

### Step 6 — Verify Stage Display

Test with a case at Stage 7 (Application Submitted). Expected display:

```
✅ Application Received
✅ Information Gathering
✅ Terms Agreed
✅ Options Presented
✅ Illustrations Prepared
✅ Initial Decision
▶  Application Submitted  ← YOU ARE HERE
○  Valuation Arranged
○  Lender Review
○  Offer Issued
○  Legal Work
○  Funds Released
```

Verify:
- Stage 4 (Product Research) is NOT shown
- All stage names are `CustomerFriendlyName` values
- Icon colours are correct: green (✅), blue (▶), grey (○)

---

## Verify

- [ ] Customer Progress View exists on the Stage Configuration list
- [ ] View filters out `VisibleToCustomer = No` stages (Stage 4 hidden)
- [ ] View sorts by StageNumber ascending
- [ ] Only CustomerFriendlyName values are shown (no internal names)
- [ ] Completed stages show ✅ icon
- [ ] Current stage shows ▶ icon with "YOU ARE HERE" label
- [ ] Future stages show ○ icon
- [ ] Progress tracker is embedded on the portal page
- [ ] When stage advances, the portal reflects the change

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Stage 4 (Product Research) still visible | Check the Customer Progress View filter: `VisibleToCustomer is equal to Yes` |
| Internal stage names showing | Ensure the view displays `CustomerFriendlyName`, not `StageName` |
| All icons show the same symbol | Verify JSON formatting references `[$StageNumber]` and `[$CurrentStageNumber]` correctly |
| JSON formatting not rendering | Ensure JSON is valid (no trailing commas); test in a standalone view first |
| "YOU ARE HERE" not appearing | Check the equality comparison: `[$StageNumber] == [$CurrentStageNumber]` must use matching number types |
| Progress not updating when stage changes | For Option A (static), page must be manually updated; consider Option B or C for dynamic updates |
