# Task P1-05: Create the Case Documents Library

**Phase:** 1 — Fact-Find  
**Story:** As a developer, I need to create the Case Documents library so that case files have a structured home  
**Priority:** 🔴 Critical  
**Estimated effort:** 30 minutes  
**Depends on:** SharePoint site provisioned

---

## Step-by-Step Guide

### Step 1 — Create the Document Library

1. Go to **Site Contents** → **+ New** → **Document library**
2. Name: `Case Documents`
3. Description: `Stores all case-related documents in a structured folder hierarchy per case`
4. Click **Create**

### Step 2 — Understand the Folder Structure

Each case will have 13 subfolders created automatically by Flow 5. The structure per case is:

```
Case Documents/
└── {CaseID}/                        ← Created by Flow 5
    ├── 01-Identity/                 ← Customer upload (guest link, Phase 1)
    ├── 02-Income/                   ← Customer upload (guest link, Phase 1)
    ├── 03-Property/                 ← Customer upload (guest link, Phase 1)
    ├── 04-Bank-Statements/          ← Customer upload (guest link, Phase 1)
    ├── 05-Fact-Find/                ← Auto PDF (future v1.1)
    ├── 06-Terms-of-Business/        ← Staff only (Phase 3)
    ├── 07-Illustrations/            ← Staff only (Phase 3)
    ├── 08-DIP/                      ← Staff only (Phase 3)
    ├── 09-Application/              ← Staff only (Phase 3)
    ├── 10-Valuation/                ← Staff only (Phase 3)
    ├── 11-Offer/                    ← Staff only (Phase 3)
    ├── 12-Legal/                    ← Staff only (Phase 3)
    └── 13-Completion/               ← Staff only (Phase 3)
```

### Step 3 — Configure Library Settings (Optional)

1. Go to **Library Settings** → **Advanced settings**
2. Set **Allow management of content types** = No (keep simple)
3. Set **Opening Documents in the Browser** = Use the server default
4. Set **New Folder command** = Yes (staff may create ad-hoc subfolders)

### Step 4 — Create a Test Folder Manually (Verification Only)

To test the structure before Flow 5 is built:

1. Click **+ New** → **Folder** → Name: `TEST-BTL-0000`
2. Navigate into `TEST-BTL-0000`
3. Create subfolders: `01-Identity`, `02-Income`, `03-Property`, `04-Bank-Statements`
4. Upload a test file to `01-Identity`
5. Verify the file appears correctly
6. Delete the `TEST-BTL-0000` folder after verification

### Step 5 — Verify

- [ ] Library appears in Site Contents as "Case Documents"
- [ ] New folders can be created manually
- [ ] Files can be uploaded to subfolders
- [ ] Library URL is accessible: `https://[tenant].sharepoint.com/sites/lending/Case Documents/`

---

## Subfolder Names Reference (for Flow 5)

```json
[
  "01-Identity",
  "02-Income",
  "03-Property",
  "04-Bank-Statements",
  "05-Fact-Find",
  "06-Terms-of-Business",
  "07-Illustrations",
  "08-DIP",
  "09-Application",
  "10-Valuation",
  "11-Offer",
  "12-Legal",
  "13-Completion"
]
```

This JSON array is used by Flow 5 to create all 13 subfolders programmatically. See [task-08-flow-create-folders.md](task-08-flow-create-folders.md).
