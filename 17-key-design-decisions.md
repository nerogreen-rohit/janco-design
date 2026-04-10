<a>← Back to Index</a>

# 17. Key Design Decisions

This section documents the significant architectural and design choices made during the design of the Specialist Lending Platform, along with the rationale for each decision.

---

## 17.1 No Dataverse — SharePoint Lists Only

| Aspect | Detail |
|---|---|
| **Decision** | Use SharePoint Lists and Libraries exclusively — no Dataverse |
| **Rationale** | Dataverse requires Power Apps Premium licensing (£16.90/user/month). SharePoint Lists are included in M365 Business Standard at no additional cost. For a 2–4 user team, this saves approximately £400–800/year. |
| **Trade-offs** | Dataverse offers relational integrity, advanced security roles, and better performance at scale. SharePoint Lists have row limits (standard view threshold: 5,000 items — manageable for a small firm) and less sophisticated query capabilities. |
| **Mitigation** | Indexed columns, filtered views, and denormalization (storing lookup values as text snapshots) are used throughout to maintain acceptable performance within SharePoint's constraints. |

---

## 17.2 Configurable Stage Lifecycle via Lookup (Not Hardcoded Choices)

| Aspect | Detail |
|---|---|
| **Decision** | Case stage is stored as a Lookup to the Stage Configuration list — not a hardcoded Choice column |
| **Rationale** | A Choice column would require app code changes every time a stage name changes. A Lookup to a configuration list means the system owner can add, rename, or reorder stages by editing a list item — no code changes required. |
| **Trade-offs** | Slightly more complex Power Apps formula (need to resolve LookupId to display values). Denormalized StageNumber and StageName columns added to Cases list to simplify filtering and display. |
| **Key design detail** | Stage History stores **text snapshots** of stage names (not Lookups) to preserve audit integrity even if stage names are later changed. |

---

## 17.3 Fact-Find as SharePoint List Item (Not Microsoft Forms)

| Aspect | Detail |
|---|---|
| **Decision** | Fact-find data is collected as a single SharePoint list item per case — no Microsoft Forms |
| **Rationale** | Microsoft Forms does not support pre-populating fields with case data, editing a previous submission, or sharing a specific form response with a specific customer. A SharePoint list item supports all of these via guest-shared direct links. It is also the single source of truth — no data sync between Forms and SharePoint required. |
| **Trade-offs** | The SharePoint list item edit form is less visually polished than a custom Microsoft Form or Power Apps form. Customers interact with SharePoint's default list item form. |
| **Mitigation** | Section ordering via SortOrder, clear column names, and grouped sections provide adequate UX for the customer. A future enhancement can wrap the fact-find in a Power Apps form on the portal page for a richer experience. |
| **Alternative considered** | Microsoft Forms + Power Automate sync to SharePoint. Rejected because it creates a secondary data copy, loses the direct edit/update capability, and adds complexity. |

---

## 17.4 Customer Portal via SharePoint Guest Sharing (Not Power Apps Portals)

| Aspect | Detail |
|---|---|
| **Decision** | Customer portal is built on SharePoint Pages and shared list items — not Power Apps Portals (now Power Pages) |
| **Rationale** | Power Apps Portals requires a significant additional license (from £1.50/login or £300/month for 100 logins). SharePoint guest sharing is included in M365 Business Standard. For 10–20 customers, SharePoint guest access is free. |
| **Trade-offs** | SharePoint-based portal is less visually customisable. Customer UX is SharePoint's default interface. |
| **Mitigation** | A well-designed SharePoint page with embedded filtered list views and clear instructions provides a functional customer experience. Custom branding via SharePoint site theming can improve appearance. |
| **Constraint** | Requires IT admin to enable external sharing at the Microsoft 365 tenant level. |

---

## 17.5 Single Fact-Find Item per Case (Not Shared Across Cases)

| Aspect | Detail |
|---|---|
| **Decision** | Each case has its own independent Fact-Find Responses item — even for portfolio clients with multiple cases |
| **Rationale** | Each mortgage application is a separate regulatory transaction. The fact-find for a BTL refinance is different to a bridging loan for the same client. Keeping them separate ensures the data is complete and accurate for each individual application, and preserves audit integrity per case. |
| **Trade-offs** | Portfolio clients need to re-enter some personal details (name, DOB, employment) for each new case. |
| **Mitigation** | A future "Clone Fact-Find" flow (v2.1) will copy common personal/income fields from a previous case's fact-find to a new one, with the customer reviewing and confirming the data. |

---

## 17.6 Denormalization Strategy

| Aspect | Detail |
|---|---|
| **Decision** | Certain lookup values are denormalized (stored as text fields) on related list items |
| **Rationale** | SharePoint's Lookup column limits complex multi-column lookups and makes filtering/sorting expensive. Denormalizing key values (e.g. StageName, LenderName, DocTypeName) into text columns enables fast filtering, reliable export, and simpler Power Apps formulas. |
| **Columns denormalized** | CurrentStageNumber, CurrentStageName (on Cases); CaseName, LenderName, DocTypeName (on child lists); StageName, PreviousStageName (on Stage History) |
| **Maintenance** | Denormalized values are updated by Power Automate flows when the source changes. Stage History snapshots are permanent — never updated after creation. |

---

## 17.7 Messages List for Customer-Staff Communication

| Aspect | Detail |
|---|---|
| **Decision** | A dedicated Messages list is used for all case-related communication — not email threads |
| **Rationale** | Email threads are scattered, unsearchable, and not linked to a specific case. A Messages list provides a case-specific, searchable, auditable communication record. Both staff and customers can access it (customers via guest-shared filtered view). |
| **Direction field values** | "Advisor → Customer", "Customer → Advisor", "Internal" — all using **Advisor** not any other term for the case owner in customer-facing context |
| **Trade-offs** | Customers must check the portal to see new messages (they receive an email notification from Power Automate, but must log into the portal to reply). |

---

## 17.8 Commission Tracking — Advisory Fee as AdvFee

| Aspect | Detail |
|---|---|
| **Decision** | Advisory fees charged to customers are recorded as CommissionType = "Advisory Fee (AdvFee)" in all lists, flows, and UI labels |
| **Rationale** | Clear, professional terminology that reflects the advisory relationship with customers. AdvFee is used as the short-form code in flow variable names, column display labels, and dashboard summary cards. |
| **All references use** | "Advisory Fee", "AdvFee" — never any alternative terminology |

---

## 17.9 Stage History as Immutable Audit Log

| Aspect | Detail |
|---|---|
| **Decision** | Stage History records are written once by Power Automate and never modified by the app or staff |
| **Rationale** | An audit trail is only valuable if it is immutable. Staff should be able to add notes at time of stage change, but should not be able to retroactively alter the history. |
| **Implementation** | The Stage History list does not have an edit form exposed in the Staff App. Power Automate creates records; staff read them in the Timeline tab. SharePoint list permissions prevent staff from editing Stage History items directly. |

---

## 17.10 No Power BI in v1.0

| Aspect | Detail |
|---|---|
| **Decision** | Dashboards in v1.0 are built in Power Apps (Canvas) — not Power BI |
| **Rationale** | Power BI Pro is required to share reports with colleagues (£8.60/user/month). For a 2–4 person team starting up, in-app dashboards using Power Apps galleries and calculated counts provide sufficient insight at £0 additional cost. |
| **Migration path** | When the team grows or the system owner requires richer reporting, Power BI reports can be built against the same SharePoint lists and embedded into the Staff App or SharePoint pages with minimal rework. |
