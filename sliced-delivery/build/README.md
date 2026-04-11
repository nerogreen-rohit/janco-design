# Build Guides — Specialist Lending Platform

This folder contains **step-by-step build guides** for each delivery phase. Each phase has:

| File | Purpose |
|---|---|
| `stories.md` | User stories for every task in the phase, grouped by feature area |
| `task-*.md` | Detailed step-by-step implementation guide for each task |
| `imports/` | Importable CSV / JSON content for SharePoint lists and configuration data |

---

## Phase Index

| Phase | Folder | Duration | Key Deliverable |
|---|---|---|---|
| **1** | [`phase-1-fact-find/`](phase-1-fact-find/) | Weeks 1–2 | Customer fact-find online; staff reviews in app |
| **2** | [`phase-2-lead-management/`](phase-2-lead-management/) | Week 3 | Structured lead capture, qualification, convert to case |
| **3** | [`phase-3-case-management/`](phase-3-case-management/) | Weeks 4–5 | Full 14-stage lifecycle, documents, messages, SLA alerts |
| **4** | [`phase-4-customer-portal/`](phase-4-customer-portal/) | Week 6 | Customer portal — progress, messages, uploads |
| **5** | [`phase-5-commissions-dashboards/`](phase-5-commissions-dashboards/) | Week 7 | Commission tracking + 4 KPI dashboards |
| **6** | [`phase-6-uat-golive/`](phase-6-uat-golive/) | Week 8 | End-to-end testing, training, go-live |

---

## How to Use These Guides

1. **Start with `stories.md`** in the phase folder to see the full backlog of user stories.
2. **Open the linked `task-*.md`** for step-by-step implementation instructions.
3. **Import CSV/JSON files** from the `imports/` subfolder directly into SharePoint using the built-in import feature or PnP PowerShell.
4. Complete tasks in order — each task builds on the previous one within a phase.
5. Test acceptance criteria listed at the end of each task before moving on.

---

## Import Instructions

### CSV Import to SharePoint

1. Navigate to the target SharePoint list
2. Click **⚙️ Settings** → **List Settings** → **Create column** (or use the CSV to create the list)
3. Alternatively, use the SharePoint **"Import from CSV"** feature:
   - Go to **Site Contents** → **+ New** → **List** → **From CSV**
   - Upload the CSV file from the `imports/` folder
   - Map columns and confirm

### PnP PowerShell Import

```powershell
# Connect to your SharePoint site
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/lending" -Interactive

# Import list items from CSV
$csv = Import-Csv -Path ".\imports\stage-configuration.csv"
foreach ($row in $csv) {
    Add-PnPListItem -List "Stage Configuration" -Values @{
        "Title"               = $row.Title
        "ShortName"           = $row.ShortName
        "CustomerFriendlyName"= $row.CustomerFriendlyName
        "StageNumber"         = [int]$row.StageNumber
        "VisibleToCustomer"   = ($row.VisibleToCustomer -eq "Yes")
        "DefaultSLADays"      = [int]$row.DefaultSLADays
        "CanSkip"             = ($row.CanSkip -eq "Yes")
    }
}
```
