# Task P5-12: Dashboard Screen — Combined Layout

**Phase:** 5 — Commissions & Dashboards  
**Story:** As a staff member, I want a combined Dashboard screen with tabs for Pipeline, Performance, Commission, and Compliance so that all KPIs are in one place  
**Priority:** 🟡 High  
**Estimated effort:** 1–2 hours  
**Depends on:** Staff App (P1-11), Pipeline Dashboard (P5-08), Performance Dashboard (P5-09), Commission Dashboard (P5-10), Compliance Dashboard (P5-11)

---

## Step-by-Step Guide

### Step 1 — Create the Dashboard Screen

1. Open the **Specialist Lending Staff App** in Power Apps Studio
2. Add a new screen: **+ New screen** → **Blank**
3. Name: `scrDashboard`

### Step 2 — Add Screen Header

1. Add a **Rectangle** across the top:
   - Fill: Your brand colour
   - Height: 60
2. Add a **Label** inside:
   - Text: `"Dashboards"`
   - Font size: 24, Bold, White

### Step 3 — Create the Tab Selector

1. Add a **horizontal container** below the header for the tab bar
2. Add 4 tab buttons:

| Button Name | Text | OnSelect |
|---|---|---|
| btnTabPipeline | `"Pipeline"` | `Set(varDashboardTab, "Pipeline")` |
| btnTabPerformance | `"Performance"` | `Set(varDashboardTab, "Performance")` |
| btnTabCommission | `"Commission"` | `Set(varDashboardTab, "Commission")` |
| btnTabCompliance | `"Compliance"` | `Set(varDashboardTab, "Compliance")` |

3. Highlight the active tab. On each button's `Fill` property:

```
If(varDashboardTab = "Pipeline", ColorValue("#0078D4"), ColorValue("#F0F0F0"))
```

(Repeat for each button with the matching tab name.)

4. On each button's `Color` (text colour):

```
If(varDashboardTab = "Pipeline", White, Black)
```

### Step 4 — Set Default Tab

On the screen's `OnVisible`:

```
If(IsBlank(varDashboardTab), Set(varDashboardTab, "Pipeline"))
```

### Step 5 — Add Dashboard Content Containers

Add 4 containers, one for each dashboard. Each container's `Visible` property:

| Container | Visible |
|---|---|
| `conPipelineDashboard` | `varDashboardTab = "Pipeline"` |
| `conPerformanceDashboard` | `varDashboardTab = "Performance"` |
| `conCommissionDashboard` | `varDashboardTab = "Commission"` |
| `conComplianceDashboard` | `varDashboardTab = "Compliance"` |

> The content of each container is built in Tasks P5-08 through P5-11.

### Step 6 — Auto-Refresh on Tab Switch

To ensure data refreshes when switching tabs, update each tab button's `OnSelect` to include a data refresh:

```
// Example for Pipeline tab
Set(varDashboardTab, "Pipeline");
ClearCollect(
    colActiveCases,
    Filter('Specialist Lending Cases', CaseStatus.Value = "Active")
)
```

> Each tab should refresh only the data it needs — see Tasks P5-08 to P5-11 for the specific collections.

### Step 7 — Add a Refresh Button

1. Add a **Button** or **Icon** (🔄) in the header area:
   - Text: `"🔄 Refresh"`
   - OnSelect:

```
// Refresh all data based on current tab
Switch(
    varDashboardTab,
    "Pipeline",
        ClearCollect(colActiveCases, Filter('Specialist Lending Cases', CaseStatus.Value = "Active")),
    "Performance",
        ClearCollect(colAllCases, 'Specialist Lending Cases');
        ClearCollect(colAllLeads, Leads);
        ClearCollect(colStageHistory, 'Case Stage History'),
    "Commission",
        ClearCollect(colAllCommissions, Commissions),
    "Compliance",
        ClearCollect(colComplianceCases, Filter('Specialist Lending Cases', CaseStatus.Value = "Active"));
        ClearCollect(colFactFinds, 'Fact-Find Responses');
        ClearCollect(colDocRequests, 'Document Requests')
);
Notify("Dashboard refreshed.", NotificationType.Success)
```

### Step 8 — Add Dashboard to App Navigation

1. Add "Dashboard" to the app's left navigation menu or top nav bar
2. OnSelect: `Navigate(scrDashboard)`
3. Consider using an icon: 📊

### Step 9 — Full Wireframe Reference

```
╔══════════════════════════════════════════════════════════════════════╗
║  📊 DASHBOARDS                                          [🔄 Refresh]║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                       ║
║  ┌────────────┐┌──────────────┐┌──────────────┐┌──────────────┐     ║
║  │▓ Pipeline ▓││ Performance  ││ Commission   ││ Compliance   │     ║
║  └────────────┘└──────────────┘└──────────────┘└──────────────┘     ║
║                                                                       ║
║  ┌────────────────────────────────────────────────────────────────┐  ║
║  │                                                                │  ║
║  │              [Active dashboard content here]                   │  ║
║  │                                                                │  ║
║  │  Pipeline → See P5-08 wireframe                                │  ║
║  │  Performance → See P5-09 wireframe                             │  ║
║  │  Commission → See P5-10 wireframe                              │  ║
║  │  Compliance → See P5-11 wireframe                              │  ║
║  │                                                                │  ║
║  └────────────────────────────────────────────────────────────────┘  ║
║                                                                       ║
║  ─────────────────────────────────────────────────────────────────── ║
║  Last refreshed: @{Now()} | Tab: @{varDashboardTab}                  ║
╚══════════════════════════════════════════════════════════════════════╝
```

### Step 10 — Verify

- [ ] Dashboard screen is accessible from app navigation
- [ ] All 4 tabs are visible: Pipeline, Performance, Commission, Compliance
- [ ] Clicking a tab changes `varDashboardTab` and shows the correct content
- [ ] Active tab is visually highlighted (different colour/style)
- [ ] Default tab is "Pipeline" on first load
- [ ] Only one dashboard container is visible at a time
- [ ] Data refreshes when switching tabs
- [ ] Refresh button reloads the current tab's data
- [ ] Screen performs well (no long loading times)

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| No tab is highlighted on first load | Ensure `OnVisible` sets `varDashboardTab` to "Pipeline" if blank |
| Multiple containers visible at once | Verify each container's `Visible` checks for exactly one tab value |
| Data stale after switching tabs | Ensure each tab button's `OnSelect` includes the `ClearCollect` for its data |
| Screen loads slowly | Load data only for the active tab, not all tabs at once |
| Tab buttons not aligned | Use a horizontal container with equal-width distribution |
