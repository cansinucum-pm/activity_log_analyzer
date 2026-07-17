# Activity Log Analyzer

`activity_log_analyzer.html` is a single, self-contained HTML file that turns a raw
activity/effort-log export (timesheet, DevOps task log, whatever your tracker produces)
into a KPI dashboard, category/period/resource breakdown, a data-quality/reconciliation
check, a ready-to-send summary email, and a print-to-PDF report — entirely in your browser.

## Purpose

Before invoicing or sending a status update, someone usually has to manually total up hours
by category and by person, sanity-check the numbers against whatever "Total" row is already
in the sheet, and write the same summary email again. This tool automates that pass:

- **KPI tiles** — total hours, total man-days, total amount (at an editable rate), category
  count, resource count.
- **Reconciliation check** — if the uploaded sheet already has its own Total/summary row,
  the tool compares its computed total against that row and flags any mismatch before you
  invoice off the wrong number.
- **Data-quality warnings** — duplicate Activity IDs (the classic failure of a hand-pasted
  cumulative sheet), single entries logging more than 12 hours, and rows with no category
  (bucketed as "Uncategorized" instead of silently dropped).
- **Breakdowns** — by category (table + donut chart), by resource, and — in Cumulative mode
  — by period (table + stacked bar chart).
- **Copy-ready output** — each table has a "Copy table (TSV)" button for pasting straight
  into Excel/Sheets/email, an editable email draft with a "Copy email" button, and a
  "Print / Save as PDF" button that opens a clean, print-formatted version of the report in
  a new tab.

It's fully generic — no company, project, or client names are hard-coded anywhere. The
email draft uses plain `Dear [Name]` / `[period]` placeholders for you to fill in, and
column/category names come entirely from whatever you upload.

## Installing / running it

This is a static HTML file with everything (including the spreadsheet-parsing library)
embedded inside it — there is nothing to install and no build step.

1. **Download the file from GitHub** — don't just use GitHub's file preview (that only
   shows the source code, not the running tool). Either:
   - open `activity_log_analyzer.html` in the repo and click **"Download raw file"**
     (the download icon at the top-right of the file view), or
   - `git clone` the repo and use the file from your local copy.
2. **Open the downloaded file directly in a browser** (double-click it, or drag it into a
   Chrome/Edge/Firefox window).
3. That's it — **no internet connection is required**. Everything, including the Excel
   parser, runs and stays entirely inside your browser; no data is ever uploaded anywhere.

## How to use it

1. **Step 1 — choose a report mode**: *Single period* (one export covering one reporting
   period) or *Cumulative* (one export covering several periods, broken down period by
   period). The tool warns you if your data looks like it doesn't match the mode you picked.
2. **Need the right columns?** Click **"Download Excel template"** to get a spreadsheet with
   the expected header row, an example row, and an "Instructions" sheet — or just use your
   own export if it already has equivalent columns (see below).
3. **Drop your Excel or CSV export** onto the page.
4. Review the KPI tiles, the reconciliation banner, and the breakdown tables/charts.
   Adjust **Unit price** and **Hours per man-day** any time — everything recalculates live.
5. Use **Copy table (TSV)**, **Copy email**, or **Print / Save as PDF** to get the output
   you need.

### What your export needs

The header row is auto-detected (scanned within the first 30 rows) and columns are matched
by name, so most existing exports work without renaming anything:

| Column | Required? | Matches (case-insensitive) |
|---|---|---|
| Activity ID | recommended | header containing "ActivityID" or exactly "ID" |
| Duration | **required** | "Duration (Number)"/"Hours" as a number, or a "Duration" text column (accepts "7.5h", "450 min", etc.) |
| User / resource | **required** | header containing "User", or exactly "Resource" |
| Date | **required** | exactly "Date", or containing "Date"/"Tarih" |
| Category | **required** | header containing "CR Subject" or "Category" — if none is found, the **right-most column** in the sheet is used instead |
| Type | optional | exactly "Type" |
| Comment | optional | header containing "Comment"/"Description" |

Column headers in **Turkish** are recognized too (e.g. "Kaynak" for resource, "Süre" for
duration, "Tarih" for date, "Açıklama" for comment, "Kategori" for category).

Any row that looks like a **summary/total line** (starts with "Total", "Grand total",
"Unit price", "Daily rate", "Toplam", "Amount", "Invoice", …) is automatically excluded from
the totals and — if it contains recognizable numbers — used for the reconciliation check
instead.

## Repo contents

- `activity_log_analyzer.html` — the tool itself (everything you need).
- `Activity_Log_Template.xlsx` — the same import template the app lets you download from
  within the tool, included here for reference.
