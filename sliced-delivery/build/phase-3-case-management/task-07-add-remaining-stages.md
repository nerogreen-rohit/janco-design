# Task P3-07: Add Remaining Stages (4–14) to Stage Configuration

**Phase:** 3 — Case Management & Lifecycle  
**Story:** As a developer, I need to add stages 4–14 to the Stage Configuration list so that the full 14-stage lifecycle is available  
**Priority:** 🔴 Critical  
**Estimated effort:** 30 minutes  
**Depends on:** Stage Configuration list (P1-02) with stages 1–3 already populated

---

## Step-by-Step Guide

### Step 1 — Open the Stage Configuration List

1. Navigate to **Site Contents** → open `Stage Configuration`
2. Verify that stages 1–3 already exist from Phase 1:
   - 1 — Customer Enquiry
   - 2 — Fact-Find
   - 3 — Terms of Business & GDPR

### Step 2 — Add Stages 4–14

Add each row using **+ New** or the Quick Edit grid view. Enter the following data:

| Title | ShortName | CustomerFriendlyName | StageNumber | VisibleToCustomer | DefaultSLADays | CanSkip | SortOrder | IsActive |
|---|---|---|---|---|---|---|---|---|
| Product Research | PROD | Under Review | 4 | No | 3 | No | 4 | Yes |
| Present Options & Agree | OPT | Options Presented | 5 | Yes | 3 | Yes | 5 | Yes |
| Produce Illustrations | ILLUS | Illustrations Prepared | 6 | Yes | 3 | Yes | 6 | Yes |
| Decision in Principle | DIP | Initial Decision | 7 | Yes | 5 | Yes | 7 | Yes |
| Full Application | APP | Application Submitted | 8 | Yes | 7 | No | 8 | Yes |
| Valuation | VAL | Valuation Arranged | 9 | Yes | 10 | No | 9 | Yes |
| Underwriting | UW | Lender Review | 10 | Yes | 14 | No | 10 | Yes |
| Loan Offer | OFFER | Offer Issued | 11 | Yes | 5 | No | 11 | Yes |
| Legals | LEGAL | Legal Work | 12 | Yes | 21 | No | 12 | Yes |
| Completion | COMP | Funds Released | 13 | Yes | 1 | No | 13 | Yes |
| Post-Completion & Logging | POST | Completed | 14 | Yes | 3 | No | 14 | Yes |

### Step 3 — Verify the Complete 14-Stage Table

After adding, the full list should contain:

```
 #  | Title                         | Short | CustomerFriendlyName    | Visible | SLA | Skip
----+-------------------------------+-------+-------------------------+---------+-----+------
 1  | Customer Enquiry              | ENQ   | Application Received    | Yes     |  1  | No
 2  | Fact-Find                     | FACT  | Information Gathering   | Yes     |  5  | No
 3  | Terms of Business & GDPR      | TOB   | Terms Agreed            | Yes     |  2  | No
 4  | Product Research              | PROD  | Under Review            | No      |  3  | No
 5  | Present Options & Agree       | OPT   | Options Presented       | Yes     |  3  | Yes
 6  | Produce Illustrations         | ILLUS | Illustrations Prepared  | Yes     |  3  | Yes
 7  | Decision in Principle         | DIP   | Initial Decision        | Yes     |  5  | Yes
 8  | Full Application              | APP   | Application Submitted   | Yes     |  7  | No
 9  | Valuation                     | VAL   | Valuation Arranged      | Yes     | 10  | No
10  | Underwriting                  | UW    | Lender Review           | Yes     | 14  | No
11  | Loan Offer                    | OFFER | Offer Issued            | Yes     |  5  | No
12  | Legals                        | LEGAL | Legal Work              | Yes     | 21  | No
13  | Completion                    | COMP  | Funds Released          | Yes     |  1  | No
14  | Post-Completion & Logging     | POST  | Completed               | Yes     |  3  | No
```

### Step 4 — Stage Applicability Notes

When advancing stages, the `CanSkip` flag determines whether a stage can be bypassed. Product-specific applicability:

| Stage | BTL | Bridging | Commercial | Dev Finance | Refurb | Auction |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| 5. Present Options & Agree | ✅ | ⚪ | ✅ | ✅ | ✅ | ⚪ |
| 6. Produce Illustrations | ✅ | ⚪ | ✅ | ⚪ | ⚪ | ⚪ |
| 7. Decision in Principle | ✅ | ⚪ | ✅ | ⚪ | ⚪ | ⚪ |

✅ = Required  ⚪ = Optional / Skip allowed

> **Note:** All other stages (1–4, 8–14) apply to every product type. The Staff App Overview tab should show a "Skip" option only for stages where `CanSkip = Yes`.

### Step 5 — Verify

- [ ] Stage Configuration list contains exactly 14 items
- [ ] StageNumbers are sequential 1–14 with no gaps
- [ ] Stage 4 (Product Research) has VisibleToCustomer = No
- [ ] Stages 5, 6, 7 have CanSkip = Yes
- [ ] Stages 8–14 have CanSkip = No
- [ ] DefaultSLADays values match the table above
- [ ] All items have IsActive = Yes

---

## Troubleshooting

| Issue | Resolution |
|---|---|
| Duplicate stage numbers | Each StageNumber must be unique — check for typos before saving |
| Stage 4 showing in customer portal | Verify VisibleToCustomer is set to No for stage 4 |
| CanSkip values wrong | Only stages 5, 6, 7 should have CanSkip = Yes — all others = No |
| Existing stages 1–3 missing | These were created in Phase 1 (P1-02) — recreate them if needed using the data above |
