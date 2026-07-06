# BOL × Scan Cross-Checker

A single-file, **fully offline** web tool that cross-checks tracking IDs from
carrier Bills of Lading (BOLs) against the warehouse scan report, and flags any
package that was on a BOL but never got scanned — so one person on the Ops team
can do the weekly cross-reference in seconds instead of by hand.

No install, no server, no internet. Everything runs in your browser — your
files never leave your machine.

## The workflow it supports

1. **Intelcom** and **Metro** drop packages at the warehouse, each with a BOL
   listing the tracking IDs they delivered.
2. Ops (Pasquale) scans every package into one scan report.
3. Load the BOLs (**data set A** — one source per carrier) and the scan report
   (**data set B** — a single Excel file) into this tool.
4. Click **Cross-check**. It confirms whether every BOL tracking ID appears in
   the scan report.
5. For anything missing, it auto-drafts an email — grouped by carrier — that you
   can copy or open straight in your mail app.

## How to use it

1. Open **`index.html`** in any modern browser (double-click it, or right-click →
   Open with). It works from a local file — no web host needed.
2. Under **Bills of Lading**, drop in each carrier's BOL. Files can be **Excel,
   CSV, or Word (.docx)** — BOLs often arrive as Word documents, and the tool
   pulls the tracking numbers straight out of them. Add more carriers with
   **+ Add another BOL** if needed.
3. Under **Warehouse Scan Report**, drop in the scan file (Excel or CSV).
4. The tool auto-finds the tracking-ID column (for Excel/CSV) or the tracking
   numbers (for Word). The count updates live so you can confirm it read the
   right thing:
   - For Excel/CSV, if it picked the wrong column just change the dropdown.
   - For Word, the pulled numbers show in an editable box — delete any stray
     line before checking.
5. Click **Cross-check tracking IDs** and review:
   - **Missing (not scanned)** — on a BOL but absent from the scan report. These
     are what you chase.
   - **Scanned, not on any BOL** — scanned but not on any loaded BOL (a possible
     missing BOL, a wrong load, or a typo).
6. Scroll to **Email to send** — a ready-to-paste message listing the missing
   numbers by carrier. Hit **Copy email** (or **Copy just the tracking
   numbers**) and paste it into your mail app.

## Matching rules

- Comparison is **case-insensitive** and ignores surrounding and internal whitespace, so
  `int100002`, `INT100002`, and ` INT100002 ` all match.
- Long numeric IDs are read as full digit strings (no scientific-notation
  mangling).
- Duplicate rows within a single BOL are counted once and noted.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The entire app (open this). |
| `vendor/xlsx.full.min.js` | [SheetJS](https://sheetjs.com/) Excel parser, bundled locally so the tool works offline. |
| `samples/` | Example Intelcom BOL, Metro BOL, and scan report you can load to try it. In the sample, `INT100003` and `MET900004` are the two missing IDs. |

## Supported input formats

- **BOLs:** `.xlsx`, `.xls`, `.xlsm`, `.csv`, and `.docx` (Word).
- **Scan report:** `.xlsx`, `.xls`, `.xlsm`, `.csv`.

Word `.docx` reading uses the browser's built-in decompression — no extra
library needed. (Old-style binary `.doc` files aren't supported; re-save them as
`.docx` first.)
