# Task P3-18: Staff App — Messages Tab

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a staff member, I want a Messages tab so that I can send and view case communications with the customer  
**Priority:** 🟡 High  
**Estimated effort:** 2 hours  
**Depends on:** Messages list (P3-05), Cases list (P1-01), Flow 14 — Message Notification (P3-14), Case Workspace screen (P3-16)

---

## Step-by-Step Guide

### Step 1 — Add the Messages Tab

1. In the Case Workspace screen, add the Messages tab content area
2. This tab is visible when the user selects "Messages" from the tab navigation

### Step 2 — Screen Layout

```
╔══════════════════════════════════════════════════════════════════════════╗
║  MESSAGES — BTL-2026-0001                                               ║
╠══════════════════════════════════════════════════════════════════════════╣
║  Direction: [All ▼]                                                     ║
╠══════════════════════════════════════════════════════════════════════════╣
║                                                                         ║
║  ┌─────────────────────────────────────────────────────────────────┐   ║
║  │ 📤 Advisor → Customer          Alice Brown  •  15 Apr 14:32    │   ║
║  │                                                                 │   ║
║  │ Hi John, please upload your payslips at your earliest           │   ║
║  │ convenience. The deadline is 22 April.                          │   ║
║  └─────────────────────────────────────────────────────────────────┘   ║
║                                                                         ║
║  ┌─────────────────────────────────────────────────────────────────┐   ║
║  │ 📥 Customer → Advisor           John Doe  •  16 Apr 09:15      │   ║
║  │                                                                 │   ║
║  │ Hi Alice, I've uploaded the payslips. Could you also let me     │   ║
║  │ know what the next steps are?                                   │   ║
║  └─────────────────────────────────────────────────────────────────┘   ║
║                                                                         ║
║  ┌─────────────────────────────────────────────────────────────────┐   ║
║  │ 🔒 Internal                     Alice Brown  •  16 Apr 09:30   │   ║
║  │                                                                 │   ║
║  │ Payslips look fine. Ready to proceed to DIP.                    │   ║
║  └─────────────────────────────────────────────────────────────────┘   ║
║                                                                         ║
╠══════════════════════════════════════════════════════════════════════════╣
║  ── COMPOSE MESSAGE ────────────────────────────────────────────────── ║
║                                                                         ║
║  Direction: [Advisor → Customer ▼]                                      ║
║  Message:   [_________________________________________________]        ║
║             [_________________________________________________]        ║
║                                                  [Send Message →]       ║
╚══════════════════════════════════════════════════════════════════════════╝
```

### Step 3 — Direction Filter Dropdown

1. Add a **Dropdown** control at the top:

```
// Items
["All", "Advisor → Customer", "Customer → Advisor", "Internal"]
```

### Step 4 — Messages Gallery

1. Add a **Gallery** control (vertical, flexible height)
2. Set **Items**:

```
Sort(
    Filter(
        Messages,
        CaseID = varCurrentCase.CaseID &&
        (ddDirectionFilter.Selected.Value = "All" ||
         Direction.Value = ddDirectionFilter.Selected.Value)
    ),
    SentDate,
    SortOrder.Descending
)
```

### Step 5 — Message Card Template

Inside the gallery, add a card-style layout:

| Element | Text Property |
|---|---|
| Direction icon | `Switch(ThisItem.Direction.Value, "Advisor → Customer", "📤", "Customer → Advisor", "📥", "Internal", "🔒")` |
| Direction label | `ThisItem.Direction.Value` |
| Sender name | `ThisItem.SentByName` |
| Timestamp | `Text(ThisItem.SentDate, "dd MMM HH:mm")` |
| Message body | `ThisItem.MessageBody` |

**Card background color by direction:**
```
Switch(
    ThisItem.Direction.Value,
    "Advisor → Customer", RGBA(232, 244, 248, 1),    // Light blue
    "Customer → Advisor", RGBA(255, 248, 232, 1),    // Light yellow
    "Internal",           RGBA(245, 245, 245, 1)      // Light gray
)
```

**Unread indicator (for customer→advisor messages):**
```
// Show a bold dot if IsReadByStaff = false
If(
    ThisItem.Direction.Value = "Customer → Advisor" &&
    ThisItem.IsReadByStaff = false,
    "● ",
    ""
)
```

### Step 6 — Mark as Read on View

When a customer→advisor message is displayed, mark it as read:

```
// Gallery OnVisible or OnSelect
ForAll(
    Filter(
        Messages,
        CaseID = varCurrentCase.CaseID &&
        Direction.Value = "Customer → Advisor" &&
        IsReadByStaff = false
    ),
    Patch(
        Messages,
        ThisRecord,
        {IsReadByStaff: true}
    )
)
```

> **Note:** Consider batching this or running on tab selection to avoid excessive API calls.

### Step 7 — Compose Area

Add controls for composing a new message:

| Control | Type | Configuration |
|---|---|---|
| ddSendDirection | Dropdown | Items: `["Advisor → Customer", "Internal"]`. Default: `"Advisor → Customer"` |
| txtMessageBody | Text Input | Mode: MultiLine, PlaceholderText: `"Type your message..."` |

> **Note:** "Customer → Advisor" is NOT available in the compose dropdown — customers send messages through the portal.

### Step 8 — Send Message Button

**[Send Message →]** button `OnSelect`:

```
// Validate
If(
    IsBlank(txtMessageBody.Text),
    Notify("Please enter a message.", NotificationType.Error),

    // Create the message
    Patch(
        Messages,
        Defaults(Messages),
        {
            Title: varCurrentCase.CaseID & "-" &
                   Text(Now(), "yyyyMMddHHmmss"),
            CaseID: varCurrentCase.CaseID,
            CaseName: varCurrentCase.Title,
            MessageBody: txtMessageBody.Text,
            SentBy: User(),
            SentByName: User().FullName,
            Direction: {Value: ddSendDirection.Selected.Value},
            IsRead: false,
            IsReadByStaff: true,
            SentDate: Now(),
            IsSystemMessage: false
        }
    );

    // Clear the compose area
    Reset(txtMessageBody);

    // Flow 14 triggers automatically for Advisor → Customer messages
    Notify("Message sent.", NotificationType.Success)
)
```

### Step 9 — Test

1. Open a test case → Messages tab
2. Test message display:
   - [ ] All messages for the case are shown, sorted by date (newest first)
   - [ ] Direction filter works (All, Advisor→Customer, Customer→Advisor, Internal)
   - [ ] Direction icons and colors display correctly
   - [ ] Unread customer messages show bold indicator
3. Test sending a message:
   - [ ] Select "Advisor → Customer" and type a message
   - [ ] Click [Send Message →]
   - [ ] Message appears in the gallery immediately
   - [ ] Flow 14 sends email notification to customer
4. Test internal messages:
   - [ ] Select "Internal" direction
   - [ ] Send a message — verify it does NOT trigger customer notification
5. Test read tracking:
   - [ ] Customer→Advisor messages are marked as read when the tab is viewed

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Messages gallery empty | Verify CaseID filter matches current case; check list connection |
| Direction dropdown not showing arrows | Use Unicode `→` (U+2192) in the choices |
| Flow 14 not triggering | Verify the Messages list item is created with Direction = "Advisor → Customer" |
| Read status not updating | Check that the ForAll/Patch for IsReadByStaff runs on tab selection |
| Compose area doesn't clear | Verify `Reset(txtMessageBody)` is called after successful Patch |
