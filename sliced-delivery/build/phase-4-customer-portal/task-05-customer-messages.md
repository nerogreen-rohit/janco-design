# Task P4-05: Configure Customer Message View & Reply

**Phase:** 4 — Customer Portal  
**Story:** As a customer, I want to view messages from my advisor and send replies so that I can communicate without calling  
**Priority:** 🟡 High  
**Estimated effort:** 1–2 hours  
**Depends on:** P4-01 (portal page created), Phase 3 (Messages list operational)

---

## Step-by-Step Guide

### Step 1 — Understand the Messages Model

The Messages list (built in Phase 3) stores all communications for a case. Customers should only see messages directed to them, not internal staff notes.

| Direction Value | Shown to Customer | Description |
|---|---|---|
| `Advisor → Customer` | ✅ Yes | Messages from advisor to customer |
| `Customer → Advisor` | ✅ Yes | Messages from customer to advisor |
| `Internal` | ❌ No | Internal staff notes — hidden from customer |

Messages are filtered by **CaseID** (customer sees only their case) and **Direction ≠ Internal**.

### Step 2 — Create the Customer Messages View

1. Navigate to the **Messages** list
2. Click **⚙️** → **List Settings** → **Views** → **Create view**
3. Name: `Customer Messages`
4. Configure the view:
   - **Columns to show:**
     - `SentDate` (or `Created`)
     - `Direction`
     - `Subject`
     - `MessageBody`
     - `SenderName`
   - **Filter:**
     - `Direction is not equal to Internal`
   - **Sort:** `SentDate` descending (newest first)
5. Click **OK**

> **Note:** The CaseID filter is applied at the web part level on the portal page (Step 4).

### Step 3 — Apply View Formatting

1. Open the **Customer Messages** view
2. Click the view dropdown → **Format current view** → **Advanced mode**
3. Paste the following JSON:

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/row-formatting.schema.json",
  "hideColumnHeader": true,
  "rowFormatter": {
    "elmType": "div",
    "style": {
      "display": "flex",
      "flex-direction": "column",
      "padding": "10px 12px",
      "margin-bottom": "6px",
      "border-left": "=if([$Direction] == 'Advisor → Customer', '4px solid #0078d4', '4px solid #107c10')",
      "background-color": "=if([$Direction] == 'Advisor → Customer', '#f0f6ff', '#f0fff0')",
      "border-radius": "4px"
    },
    "children": [
      {
        "elmType": "div",
        "style": {
          "display": "flex",
          "justify-content": "space-between",
          "margin-bottom": "4px"
        },
        "children": [
          {
            "elmType": "span",
            "style": {
              "font-weight": "600",
              "font-size": "13px",
              "color": "=if([$Direction] == 'Advisor → Customer', '#0078d4', '#107c10')"
            },
            "txtContent": "=if([$Direction] == 'Advisor → Customer', '📨 From your advisor', '📤 Your message')"
          },
          {
            "elmType": "span",
            "style": {
              "font-size": "12px",
              "color": "#666666"
            },
            "txtContent": "=toLocaleDateString([$SentDate])"
          }
        ]
      },
      {
        "elmType": "div",
        "style": {
          "font-weight": "600",
          "font-size": "14px",
          "margin-bottom": "4px"
        },
        "txtContent": "[$Subject]"
      },
      {
        "elmType": "div",
        "style": {
          "font-size": "13px",
          "color": "#333333",
          "line-height": "1.4"
        },
        "txtContent": "[$MessageBody]"
      }
    ]
  }
}
```

### Step 4 — Embed Messages on the Portal Page

1. Open the customer's portal page (P4-01) in **Edit** mode
2. Navigate to the **MESSAGES** section
3. Add a **List** web part
4. Select the **Messages** list
5. Select the **Customer Messages** view
6. Click **Edit web part** (pencil icon) and configure:
   - **Filter:** `CaseID is equal to [the customer's CaseID]`

> **Alternative:** Create a per-case view (e.g. `Messages — BTL-2026-0001`) with the CaseID filter built into the view definition.

### Step 5 — Add "View Messages" Link

Below or above the embedded list, add a prominent link:

1. Add a **Text** web part
2. Add text: `📨 View Messages`
3. Link it to the Messages list filtered view URL:
   ```
   https://[tenant].sharepoint.com/sites/lending/Lists/Messages/Customer%20Messages.aspx?FilterField1=CaseID&FilterValue1=[CaseID]
   ```

### Step 6 — Configure Customer Reply Capability

Customers need to reply to messages via guest access:

1. On the **Messages** list, click **⚙️** → **List Settings** → **Advanced settings**
2. Ensure **Allow items to be created** is enabled for users with Contribute access
3. Share the Messages list with the customer:
   - Click **Share** on the list
   - Enter the customer's email
   - Grant **Can edit** access (so they can create new items / replies)
   - **Important:** Use item-level or view-level sharing to prevent access to other customers' messages

> **Security note:** Guest sharing at the list level may expose other cases' messages. Mitigate by:
> - Using filtered view links only (customer never sees the unfiltered list)
> - Configuring item-level permissions if available
> - See P4-07 for full permission details

### Step 7 — Add Unread Message Indicator (Optional)

To show unread message count on the portal page:

1. Add a **Text** web part above the messages list
2. Manually update it with: `✉ You have [N] unread message(s) from your advisor`
3. **Future enhancement:** Use a Power Automate flow to update a `UnreadCount` column on the Cases list, and display it dynamically on the portal page

> **Note:** SharePoint list views do not natively support unread tracking. For Phase 4, this can be a manual indicator or omitted. A fully automated unread counter is a future enhancement.

### Step 8 — Test Messages

Create test messages for the customer's case:

| Subject | Direction | MessageBody |
|---|---|---|
| Welcome to your application | Advisor → Customer | Hi John, welcome to the lending hub. |
| RE: Welcome | Customer → Advisor | Thanks, I've started the fact-find. |
| Internal: Credit check note | Internal | Customer credit score 720 — proceed. |

Verify:
- "Advisor → Customer" and "Customer → Advisor" messages appear
- "Internal" messages do NOT appear
- Messages are sorted newest first
- Advisor messages show blue styling; customer messages show green styling

---

## Verify

- [ ] Customer Messages view exists on the Messages list
- [ ] View filters exclude `Direction = Internal`
- [ ] View sorts by SentDate descending
- [ ] View formatting renders advisor messages in blue and customer messages in green
- [ ] Messages list web part on portal page is filtered to the customer's CaseID
- [ ] Customer can see messages from their advisor
- [ ] Customer can create a reply message
- [ ] Internal messages are NOT visible to the customer
- [ ] "View Messages" link opens the correctly filtered view

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Customer sees messages from other cases | CaseID filter is missing or incorrect on the web part; verify the filter value matches exactly |
| Internal notes visible to customer | Check the view filter: `Direction is not equal to Internal` |
| Customer cannot reply | Verify guest sharing grants Contribute/Edit access to the Messages list |
| View formatting not rendering | Validate JSON; ensure `[$Direction]` matches the exact column internal name |
| Messages appear unsorted | Confirm the view sort is set to `SentDate` descending |
| Guest cannot create items | Check list advanced settings: "Create items" must be allowed for Contribute access level |
