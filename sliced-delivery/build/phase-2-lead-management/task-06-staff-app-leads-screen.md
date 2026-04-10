# Task P2-06: Staff App — Leads Screen

**Phase:** 2 — Lead Management  
**Story:** As a staff member, I want a Leads screen with filtering so that I can view, search, and manage all enquiries  
**Priority:** 🔴 Critical  
**Estimated effort:** 1–2 hours  
**Depends on:** Leads list (P2-01), Flow 1 running (P2-04)

---

## Step-by-Step Guide

### Step 1 — Add the Leads Screen to the Staff App

1. Open the **Specialist Lending Staff App** in Power Apps
2. Add a new screen: **Leads Screen**
3. Connect to data source: `Leads` SharePoint list

### Step 2 — Design the Screen Layout

```
╔══════════════════════════════════════════════════════════════════════╗
║  LEADS                                              [+ New Lead]    ║
║                                                                      ║
║  Filter: [All Status ▼]  [All Products ▼]  [All Assigned ▼]  [🔍]  ║
║                                                                      ║
║  ┌────────────┬──────────────┬──────────┬──────────┬────────────┐   ║
║  │  Lead ID   │  Name        │ Product  │ Status   │ Assigned   │   ║
║  ├────────────┼──────────────┼──────────┼──────────┼────────────┤   ║
║  │ LEAD-0021  │ James Hart   │ BTL      │ New      │ Sarah      │   ║
║  │ LEAD-0022  │ Maria Chen   │ Bridging │ Contacted│ Tom        │   ║
║  │ LEAD-0023  │ Paul Singh   │ Dev      │ Qualified│ Sarah      │   ║
║  └────────────┴──────────────┴──────────┴──────────┴────────────┘   ║
║                                                                      ║
║  [Select lead to view details or convert to case]                    ║
╚══════════════════════════════════════════════════════════════════════╝
```

### Step 3 — Add Filter Controls

| Control | Type | Items |
|---|---|---|
| Status Filter | Dropdown | `["All", "New", "Contacted", "Qualified", "Lost", "Dormant"]` |
| Product Filter | Dropdown | `["All", "BTL", "Bridging", "Commercial", "Dev Finance", "Refurb", "Auction"]` |
| Assigned Filter | Combo Box | Office365Users.SearchUser() |
| Search Box | Text Input | Searches FirstName, LastName, LeadID |

### Step 4 — Gallery (Lead List)

Add a **Gallery** control with Items formula:

```
Filter(
    Leads,
    // Status filter
    (ddStatusFilter.Selected.Value = "All" || LeadStatus.Value = ddStatusFilter.Selected.Value)
    &&
    // Product filter
    (ddProductFilter.Selected.Value = "All" || ProductInterest.Value = ddProductFilter.Selected.Value)
    &&
    // Search filter
    (IsBlank(txtSearch.Text) || 
     StartsWith(FirstName, txtSearch.Text) || 
     StartsWith(LastName, txtSearch.Text) ||
     StartsWith(LeadID, txtSearch.Text))
)
```

Sort: `SortByColumns(..., "Created", SortOrder.Descending)`

### Step 5 — Gallery Row Template

Each row displays:
- **Lead ID** (bold)
- **Name** (FirstName + " " + LastName)
- **Product Interest**
- **Status** (colour-coded badge: New=blue, Contacted=amber, Qualified=green, Lost=red, Dormant=grey)
- **Assigned To** (display name)

### Step 6 — Row Click Navigation

On gallery item select: `Navigate(LeadDetailScreen, ScreenTransition.None, {varSelectedLead: ThisItem})`

### Step 7 — [+ New Lead] Button

On click: `Navigate(NewLeadScreen)` — see Task P2-07.

### Step 8 — Test

- [ ] Gallery shows all leads
- [ ] Status filter correctly hides/shows leads by status
- [ ] Product filter works
- [ ] Search finds leads by name or ID
- [ ] Clicking a lead navigates to the detail screen
- [ ] [+ New Lead] navigates to the new lead form

---

## Default Filter Recommendation

By default, set the Status filter to show **active leads only** (exclude Converted, Lost, Dormant). This matches the business expectation that the Leads screen shows actionable enquiries.

```
Filter(
    Leads,
    Not(LeadStatus.Value in ["Converted", "Lost", "Dormant"])
    && ...
)
```
