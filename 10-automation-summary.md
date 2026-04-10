[← Back to Index](00-index.md)

# 10. Automation Summary

The platform uses **16 Power Automate cloud flows** to automate routine tasks, generate IDs, manage notifications, and maintain data integrity.

---

## 10.1 Flow Inventory

| # | Flow Name | Trigger | Description |
|---|---|---|---|
| 1 | **Generate Lead ID** | When Lead item created | Reads Counters list, increments LEAD counter, updates Title field with LEAD-XXXX format |
| 2 | **Convert Lead to Case** | Button (Staff App) | Creates Case item, Fact-Find item, case folder structure, generates CaseID, sends welcome email to customer |
| 3 | **Generate Case ID** | When Case item created | Reads Counters list by ProductType, increments appropriate counter, updates Title + CaseID field |
| 4 | **Create Fact-Find Item** | When Case item created | Creates linked Fact-Find Responses item, sets CaseID, generates guest share link, stores link on Case record |
| 5 | **Create Case Folder Structure** | When Case item created | Creates top-level CaseID folder and 13 subfolders in Case Documents library, applies guest sharing to folders 01–04 |
| 6 | **Log Stage Change** | When CurrentStageID changes on Case | Creates Stage History record with text snapshot of stage name, timestamps, duration in previous stage, notifies customer if stage is VisibleToCustomer |
| 7 | **Send Document Request Email** | When Document Request item created | Sends formatted email to customer requesting specific document, includes upload folder link, sets DueDate |
| 8 | **Document Received Notification** | When file uploaded to Case Documents (folders 01–04) | Creates or updates Document Request item as Received, notifies case owner |
| 9 | **SLA Reminder — Cases** | Scheduled (daily, 08:00) | Checks all active cases for SLA breaches; sends Teams notification to case owner for overdue stages; updates IsOverdue on Tasks |
| 10 | **Calculate Advisory Fee** | When Commission item created/modified | Validates Advisory Fee (AdvFee) amount, updates TotalCommissionExpected on Case record, flags if AdvFee not yet received |
| 11 | **Calculate Proc Fee** | When Commission item created/modified | Calculates expected proc fee from LoanAmount × LenderProcFeePercent, creates Commission record if not exists |
| 12 | **Email Docs to Lender** | Button (Staff App — Documents Tab) | Compiles selected files from case folder, sends formatted email to lender submission email, logs event in Stage History and Messages |
| 13 | **Customer Status Update** | When CurrentStageID changes on Case (VisibleToCustomer = Yes) | Sends customer an email update with CustomerFriendlyName of new stage, portal link, and any action required |
| 14 | **Message Notification** | When Message item created (Direction = Advisor → Customer) | Sends email notification to customer that a new message is waiting in their portal |
| 15 | **Fact-Find Updated Notification** | When Fact-Find Responses item modified | Updates LastUpdatedByCustomer timestamp on Fact-Find item, sends notification to case owner in Teams |
| 16 | **Case Completion Wrap-Up** | When CaseStatus set to "Completed" | Sends completion email to customer, marks all outstanding Document Requests as Not Required, triggers commission review reminder to case owner |

---

## 10.2 Flow Architecture Notes

### Concurrency & ID Generation
Flows 1, 3 use SharePoint `Patch` with optimistic locking on the Counters list to prevent duplicate IDs when multiple items are created simultaneously.

### Stage Change Logic
Flow 6 fires on every change to `CurrentStageID`. It:
1. Reads the Stage Config item (via ID) to get `StageName`, `CustomerFriendlyName`, `VisibleToCustomer`
2. Writes a **text snapshot** of the stage name to Stage History (not a lookup)
3. Calculates duration since `StageChangedDate`
4. If `VisibleToCustomer = Yes`, triggers Flow 13 (Customer Status Update)

### Advisory Fee Tracking
Flows 10 and 11 maintain commission totals on the Case record. The Advisory Fee is always referred to as **AdvFee** in flow variable names and Commission Type values. The Commissions list `CommissionType` choice value is "Advisory Fee (AdvFee)".

### Error Handling
All flows include:
- Try/Catch scopes (using parallel branches)
- Error notifications to the system owner email
- Failed items flagged in a Logs list (optional extension)

---

## 10.3 Trigger Summary

| Trigger Type | Count | Flows |
|---|---|---|
| SharePoint item created | 4 | 1, 3, 4, 7 |
| SharePoint item modified | 3 | 6, 10, 15 |
| File created in library | 1 | 8 |
| Scheduled (recurring) | 1 | 9 |
| Button (manual trigger from app) | 2 | 2, 12 |
| Multiple (created + modified) | 2 | 11, 14 |
| Status change | 1 | 16 |
| Stage change | 1 | 13 |
| **Total** | **16** | |
