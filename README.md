# BOL × Scan Cross-Checker

A single-file, **fully offline** web tool that cross-checks tracking IDs from
carrier Bills of Lading (BOLs) against the warehouse scan report, and flags any
package that was on a BOL but never got scanned.

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
2. Under **Bills of Lading**, upload the Intelcom and Metro BOLs. Add more
   sources with **+ Add another BOL** if a load has more than two carriers.
3. Under **Warehouse Scan Report**, upload the scan Excel file.
4. For each file the tool auto-picks the sheet and the tracking-ID column. If it
   guesses wrong, just change the dropdown — the "tracking IDs found" count
   updates live so you can confirm it's reading the right column.
5. Click **Cross-check tracking IDs** and review:
   - **Missing (not scanned)** — on a BOL but absent from the scan report. These
     are what you chase.
   - **Scanned, not on any BOL** — scanned but not on any loaded BOL (a possible
     missing BOL, a wrong load, or a typo).
6. Use **Copy email**, **Open in mail app**, or **Copy just the IDs** in the
   Draft email panel.

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

`.xlsx`, `.xls`, `.xlsm`, and `.csv`.
