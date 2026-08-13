# How to use this prompt

Product spec with highlighted edits vs the original list: [`money-manager-requirements.md`](money-manager-requirements.md).

Copy everything from the line **BEGIN PROMPT** through **END PROMPT** and paste it into Claude as a single instruction. Claude should produce a working `.xlsx` (Excel 365 / Excel 2021+). Do not add extra product decisions; every toggle, filter, and spec gap is already resolved below.

---

**BEGIN PROMPT**

# Build a personal Money Manager as an Excel workbook

You are an expert Excel workbook designer. Build a complete, formula-driven Money Manager `.xlsx` file. Do not ask clarifying questions. Implement exactly what this prompt specifies. If something is unspecified, choose the simplest Excel-native approach that preserves the formulas and layout below.

## Goal

A two-page personal money manager:

1. **Dashboard** — settings + four views on one screen, side by side: Target, Daily, Weekly, Monthly.
2. **Analytics** — transaction list, scorecards, and charts. No dropdowns, slicers, or toggles that switch which view is visible. Every former “toggle / filter / time-range option” is its own independent view on the same sheet.

Also include a **Transactions** sheet for ongoing data entry.

## Hard constraints

- Target Excel: Microsoft Excel 365 / 2021+ (Windows or Mac). File format: `.xlsx` (no macros, no `.xlsm`, no VBA, no Power Query, no Power Pivot required).
- All metrics are Excel formulas that recalculate on open. Do not bake KPI results in as static numbers.
- `TODAY()` is the current date (PC clock). That is acceptable. There is no timezone engine. Time Band is a user-chosen label, not derived from a clock time.
- Assume users type or pick from dropdowns. Do not build anti-paste validation beyond data validation lists and a numeric amount field.
- Single-user, local file. No sharing, no mobile app UX, no sheet-protection passwords.
- **No UI toggles, option buttons, form controls, slicers, or “view mode” dropdowns.** Independent views instead.
- Charts that would have had a variable number of series: give each breakdown its own chart with a **fixed** series count. Use helper ranges on a hidden `_Calc` sheet + native Excel line charts (not PivotCharts).
- Display rounding is **display-only**. Keep full precision in the stored formula values. Use number formats for money and percents. Conditional formatting and “over allowance / after deadline” tests must compare **unrounded** values.
- Category name is **Food & Dining** (not “Dinning”).
- Week = **Monday–Sunday**. Weekend = **Saturday and Sunday** (`WEEKDAY(date, 2)` → 6 or 7).
- Do not implement refunds, budgets-per-category, or extra features not listed here.
- Include a modest set of **sample transactions** (~35–50 rows over the last 6–8 weeks) so charts are not empty. Sample rows must use the real dropdown values and look plausible (mix of weekday/weekend, all four time bands, both tag pairs, several categories). The user can delete them.

Deliverable: `Money_Manager.xlsx` plus a short `README` in the same folder explaining how to use the three visible sheets.

---

## Locked product decisions (do not reopen)

| Topic | Decision |
|---|---|
| Income | **Monthly** income. One number. |
| Saved $ | **All-time income − all-time spent** |
| All-time income | `MonthlyIncome ×` number of calendar months from Target Start Date’s month through today’s month, **inclusive**. If `TODAY() < Target Start Date`, all-time income = 0. Do **not** prorate partial months. Example: start 15 Jan 2026, today 13 Aug 2026 → 8 months. |
| All-time spent | `SUM` of **every** amount in the Transactions table (entire log). |
| Weekly window | Monday–Sunday. Spent so far this week = Monday through `TODAY()` (not future days). |
| Target End Date | Used only for alert colour: if Date Projection is a real date **and** that date > Target End Date → **red font**. |
| Spent projection alert | If Weekly or Monthly spent projection **>** that view’s Allowance → **red font**. |
| Overspend / no positive save rate | Date Projection shows the text `Never` (red font). |
| Rounding | Display only (`#,##0` money, `0%` percent). Dates stay dates (`dd-mmm-yyyy`). |
| Analytics filters | Not interactive. Each filter dimension is a separate block of scorecards (one scorecard per value). |
| Chart time ranges | Not a dropdown. Each of This Week / This Month / This Year / All Time is its own chart. |
| Rolling 14-day split | Not a dropdown. Five separate charts, one per dimension, fixed series. |
| Time Band input | Dropdown label only. Bands: Morning `05:00–11:29`, Afternoon `11:30–16:59`, Evening `17:00–21:59`, Night `22:00–04:59`. Do not collect exact clock time. |
| Navigation | Two working sheets + Transactions. Hyperlinks between them are OK. |

Do **not** create the cartesian product of (filter × time range × chart type). That would be dozens of duplicate charts. Follow the Analytics layout below exactly.

---

## Workbook structure

Visible sheets (left to right):

1. `Dashboard`
2. `Analytics`
3. `Transactions`

Hidden sheets:

4. `_Lists` — source ranges for data validation.
5. `_Calc` — daily series for every chart and rolling window. Hide this sheet (`veryHidden` if the library supports it, else hidden).

Named cells on `Dashboard` (or a clearly labelled Settings block at the top of Dashboard — not a fourth visible “settings app” page):

| Name | Cell meaning |
|---|---|
| `MonthlyIncome` | Income $ per month |
| `TargetAmount` | Target $ |
| `TargetStart` | Target Start Date |
| `TargetEnd` | Target End Date |
| `DailyAllowance` | Allowance (Daily) $ |

Use defined names so Analytics and `_Calc` formulas stay readable.

---

## Settings (top of Dashboard)

One horizontal input row, labelled, with a light fill so it is obviously editable:

- Income $ (monthly)
- Target $
- Target Start Date
- Target End Date
- Allowance (Daily) $

Defaults so the file opens in a demo-ready state:

- Income: `20000`
- Target: `50000`
- Target Start Date: first day of the month that is 3 months before `TODAY()` (formula is OK; or a fixed date consistent with sample txns)
- Target End Date: 12 months after Target Start Date
- Allowance (Daily): `400`

Add a one-line note under settings: “Income is monthly. Saved = (monthly income × months since target start, inclusive) − all spending in Transactions.”

---

## Transactions sheet (ongoing input)

Excel Table named `TblSpend`, starting at row 1 with headers. Columns in this order:

| Header | Type | Rules |
|---|---|---|
| Date | Excel date | Required. Default new row: user types a date. |
| Time Band | text | Dropdown from `_Lists`: `Morning (05:00 - 11:29)`, `Afternoon (11:30 - 16:59)`, `Evening (17:00 - 21:59)`, `Night (22:00 - 04:59)` |
| Essential | text | Dropdown: `Essential`, `Non-Essential` |
| Recurrence | text | Dropdown: `Occasion`, `Reoccurrence` |
| Category | text | Dropdown exactly: `Food & Dining`, `Groceries`, `Transport`, `Shopping & Entertainment`, `Subscriptions`, `Health & Personal Care`, `Travel`, `Gifts & Donations`, `Other` |
| Amount | number | Spending amount. Format `#,##0`. Allow 0+; no currency symbol required in the cell (use number format `$#,##0` if it still displays as integer dollars). |

Helper columns **to the right of the table** (or inside the table if cleaner), hidden if they clutter data entry, all formulas:

- `WeekdayType`: `=IF(WEEKDAY([@Date],2)>=6,"Weekend","Weekday")`
- `TimeBandKey`: short key Morning / Afternoon / Evening / Night extracted from the dropdown label (e.g. text before ` (` ) so `_Calc` matching is simple
- `ISOYear`, `ISOWeek`, `MonthStart` as needed

The Table must auto-expand when the user types in the next blank row. Data validation must apply to the whole table column.

Do not use exact-time input. Do not add extra categories.

---

## Derived formulas (canonical)

Define these once (Dashboard named cells and/or `_Calc`) and reference them everywhere. Use `IF`/`IFERROR` so the dashboard never shows `#DIV/0!` or `#VALUE!`.

Let `Today` = `TODAY()`.

```
DaysElapsed = IF(Today < TargetStart, 0, Today - TargetStart)
MonthsElapsed = IF(Today < TargetStart, 0,
    (YEAR(Today)-YEAR(TargetStart))*12 + MONTH(Today)-MONTH(TargetStart) + 1)
AllTimeIncome = MonthlyIncome * MonthsElapsed
AllTimeSpent = SUM(TblSpend[Amount])
Saved = AllTimeIncome - AllTimeSpent
SavedPct = IF(TargetAmount=0, 0, Saved / TargetAmount)

DailySaveRate = IF(DaysElapsed<=0, 0, Saved / DaysElapsed)
DateProjection = IF(DailySaveRate<=0, "Never", TargetStart + TargetAmount / DailySaveRate)
```

`DateProjection` must be the literal spec:

`Target Start Date + Target $ / (Saved $ / Number of days between Current Date and Target Start Date)`

which is `TargetStart + TargetAmount / DailySaveRate`. This is **not** remaining-to-target from today.

If `DateProjection` is `"Never"` → red font.  
If `DateProjection` is a date **and** `DateProjection > TargetEnd` → red font.  
If the date is in the past (already would have hit target at this rate) → still show the date; do not turn red unless it is after Target End.

### Period bounds

```
WeekStart = Today - WEEKDAY(Today, 2) + 1          // Monday
WeekEnd   = WeekStart + 6                           // Sunday
MonthStart = DATE(YEAR(Today), MONTH(Today), 1)
DaysInMonth = DAY(EOMONTH(Today, 0))
YearStart = DATE(YEAR(Today), 1, 1)
YearEnd = DATE(YEAR(Today), 12, 31)
```

### Spent / allowance / projection

```
DailySpent = SUMIFS(TblSpend[Amount], TblSpend[Date], Today)
DailyAllowanceView = DailyAllowance
DailyPct = IF(DailyAllowanceView=0, 0, DailySpent / DailyAllowanceView)
// Daily view has NO spent projection

WeeklySpent = SUMIFS(TblSpend[Amount], TblSpend[Date], ">="&WeekStart, TblSpend[Date], "<="&Today)
WeeklyAllowance = DailyAllowance * 7
WeeklyPct = IF(WeeklyAllowance=0, 0, WeeklySpent / WeeklyAllowance)
DaysMonToToday = WEEKDAY(Today, 2)                 // 1..7
WeeklyProjection = IF(DaysMonToToday=0, 0, WeeklySpent / DaysMonToToday * 7)

MonthlySpent = SUMIFS(TblSpend[Amount], TblSpend[Date], ">="&MonthStart, TblSpend[Date], "<="&Today)
MonthlyAllowance = DailyAllowance * DaysInMonth
MonthlyPct = IF(MonthlyAllowance=0, 0, MonthlySpent / MonthlyAllowance)
Days1ToToday = DAY(Today)
MonthlyProjection = IF(Days1ToToday=0, 0, MonthlySpent / Days1ToToday * DaysInMonth)
```

Weekly projection = total spent Monday→today / days Monday→today × **7** (Mon–Sun).  
Monthly projection = total spent 1st→today / days 1st→today × days in the month.

Red font on `WeeklyProjection` if `WeeklyProjection > WeeklyAllowance`.  
Red font on `MonthlyProjection` if `MonthlyProjection > MonthlyAllowance`.

---

## Dashboard layout

Title: **Money Manager**. Settings row under the title. Then **four panels in one row, side by side**, visually equal width, with a clear header on each panel. Do not stack these four views. Do not hide any of them.

Suggested columns (adjust if needed, keep them side by side on typical 13–15" laptop width by using compact metric blocks, not four huge tables):

### Panel 1 — Target View

| Label | Value |
|---|---|
| Saved $ | `Saved` integer money |
| Target $ | `TargetAmount` integer money |
| Saved / Target % | `SavedPct` as `0%` |
| Date Projection | `DateProjection` date or `Never` |

### Panel 2 — Daily View

| Label | Value |
|---|---|
| Spent $ | `DailySpent` |
| Allowance $ | `DailyAllowanceView` |
| Spent / Allowance % | `DailyPct` |

No projection row on Daily.

### Panel 3 — Weekly View

| Label | Value |
|---|---|
| Spent $ | `WeeklySpent` |
| Allowance $ | `WeeklyAllowance` |
| Spent / Allowance % | `WeeklyPct` |
| Spent Projection | `WeeklyProjection` |

Show the week range under the header, e.g. `13-Aug-2026 to 19-Aug-2026 (Mon–Sun)`.

### Panel 4 — Monthly View

| Label | Value |
|---|---|
| Spent $ | `MonthlySpent` |
| Allowance $ | `MonthlyAllowance` |
| Spent / Allowance % | `MonthlyPct` |
| Spent Projection | `MonthlyProjection` |

Show the month name/year under the header.

Formatting:

- All money and percents: integers (number format, not `ROUND` in the formula).
- Panel headers bold; values large enough to read (14–18pt for the main numbers).
- Light distinct panel fills (neutral, professional — e.g. pale grey / pale blue / pale green / pale amber). Not neon.
- Hyperlinks: `Add spending → Transactions`, `Analytics → Analytics`.

---

## Analytics sheet

Top: title **Analytics**, hyperlink back to Dashboard, and a one-line table of contents with internal hyperlinks to each section.

Then these sections **in order**, all on this same sheet. Scroll is expected. Do not put sections on extra sheets. Do not add slicers.

### Section A — Transaction list

A readable range that shows **all** spending rows (you may use a formula view of `TblSpend`, or tell the user the list lives on Transactions and embed a filtered-looking copy via formulas). Prefer showing a **formula-driven copy** of Date, Time Band, Essential, Recurrence, Category, Amount sorted by Date descending so Analytics is self-contained. If a full live spill copy is fragile in your generator, put a prominent note + hyperlink to `Transactions` and skip the duplicate list — but only as fallback.

Do **not** duplicate the full transaction log once per filter value.

### Section B — Scorecards (independent view per filter)

One **overall** scorecard: **Sum of Spent $** = `AllTimeSpent`.

Then five independent blocks. Each block has a heading and one scorecard per value (side by side). Each scorecard = `SUMIFS` of Amount for that value (all-time, not filtered by the other dimensions).

1. **Weekday or Weekend** — 2 scorecards: Weekday, Weekend  
2. **Time Band** — 4 scorecards: Morning, Afternoon, Evening, Night  
3. **Tag: Essential or Non-Essential** — 2 scorecards  
4. **Tag: Occasion or Reoccurrence** — 2 scorecards  
5. **Category** — 9 scorecards, one per category, wrap to two rows if needed  

Under each scorecard, optional small subtitle: transaction count (`COUNTIFS`). Keep it compact.

### Section C — Time-series charts (filters do not apply; time range is not a toggle)

Unfiltered spend (all transactions). **X axis is calendar day.** Four independent charts per type, in a 4-column row, headers: **This Week** | **This Month** | **This Year** | **All Time**.

**Row C1 — Spent $ over time** (line)

- Y: $ spent that day  
- Week chart: 7 points, Monday–Sunday of the current week (future days in the week = 0)  
- Month chart: day 1..DaysInMonth of the current month (future days = 0)  
- Year chart: each day of the current year through 31 Dec (future days = 0)  
- All Time: each day from `MIN(TargetStart, MIN(TblSpend[Date]))` through `TODAY()` (do not extend All Time into the future)

**Row C2 — Cumulative Spent $ over time** (line)

- Same four time windows  
- Cumulative **resets at the start of that chart’s window** (week cumulative starts at 0 on Monday; month on the 1st; year on 1 Jan; all-time from the all-time start date)

**Row C3 — Spent $ vs Allowance** (two lines)

- Same four time windows  
- Line 1: **Daily Allowance Baseline** — plot `DailyAllowance` on every day in the window (horizontal line)  
- Line 2: **Sum of Spent $** that day  
- Legend required  

Chart styling: no 3D, thin lines, markers off or small, integer Y axis (`$#,##0`), date X axis, chart title includes the metric + window, height ~8–10 rows, width ~10 columns. Hide `_Calc` gridlines from the user; charts live on Analytics.

If the year/all-time charts are visually dense, that is acceptable — do not downsample to weeks unless a daily year chart exceeds Excel series limits, in which case All Time / Year may use daily points still (Excel handles hundreds of points).

### Section D — Rolling 14-day charts (page filters do not apply)

Heading: **Rolling 14 days Spent $**  
Subtitle: “Each point = sum of spend on that day plus the previous 13 days. All time. Not affected by the scorecard groupings.”

X: date (day), **All Time** window only (same all-time start as above through `TODAY()`).  
Y: $.  
Five independent charts, stacked vertically (full width). **Fixed series, never rebuilt:**

1. **By Weekday or Weekend** — 2 lines: Weekday, Weekend  
2. **By Time Band** — 4 lines: Morning, Afternoon, Evening, Night  
3. **By Essential or Non-Essential** — 2 lines  
4. **By Occasion or Reoccurrence** — 2 lines  
5. **By Category** — 9 lines, one per category, legend visible  

Rolling definition for date `D` and series value `V`:

`SUMIFS` Amount where Date is between `D-13` and `D` inclusive **and** the dimension equals `V`.

A transaction on a Saturday counts only toward the Weekend line, not Weekday, etc.

---

## `_Calc` sheet (hidden)

Build explicit date grids (formulas, not Python-computed values):

- `WeekDays`: 7 rows, `WeekStart + 0..6`  
- `MonthDays`: 31 rows, hide/NA after `DaysInMonth`  
- `YearDays`: 1 Jan .. 31 Dec of current year (366 rows max; Feb 29 blank/`NA()` if not leap)  
- `AllDays`: start date through Today, plus enough extra rows (e.g. 800) with `IF` blanks so the file keeps working as time passes for ~2 years. Blank rows must not plot (`NA()` not `0` for dates that do not exist). For real dates in the week/month/year window that are in the future, spent = 0 (plot zeros). For All Time, stop at Today.

For each date row compute daily spent total and daily spent by each dimension value via `SUMIFS` against `TblSpend`. Compute rolling 14 from those daily columns (`SUM` of the current and previous 13 daily cells, or `SUMIFS` on the daily grid). Compute cumulative with a running sum that starts at the first row of that grid.

Charts on Analytics must reference these `_Calc` ranges only.

---

## Formatting and UX

- Font: Calibri or Aptos, consistent.  
- Display all **numbers** as integers. Percents with 0 decimal places. Do not integer-format dates.  
- Zero money shows as `0`, not blank.  
- `Never` is text; do not force it into a date format.  
- Conditional formatting rules as specified (red font). No other traffic-light systems required.  
- Freeze Dashboard title/settings rows. Freeze Analytics title/TOC rows. Freeze Transactions header.  
- Print layout is not important.  
- No VBA, no buttons that run macros, no ActiveX. Hyperlinks only.

---

## How to generate the file

Prefer Python 3 + `openpyxl` to construct `Money_Manager.xlsx` so formulas, table, data validation, defined names, conditional formatting, and LineCharts are all present when the user opens the file in Excel.

Requirements for the generator:

- Write **Excel formulas** into KPI cells, helper columns, and `_Calc` (use `TODAY()`, `SUMIFS`, structured table references where openpyxl allows; if structured refs fail in names, use equivalent cell ranges that the Table will expand — then document that the user should not move the table).
- After creating `TblSpend`, make sure `_Calc` SUMIFS cover the table’s data body range. Include extra empty table rows (e.g. 200 blank rows already inside the table, Amount blank) **or** document that the user must resize — prefer a Table that already includes ~200 empty rows with validation so new entries are in-range without VBA.
- Create all 12 Section C charts + 5 Section D charts.
- Do not leave `#REF!`.
- Smoke-check: open the workbook with a library that evaluates formulas if available; otherwise unit-test the same math in Python against the sample data and assert Dashboard numbers would match. Fix mismatches before finishing.

---

## Acceptance checklist

The workbook is done only if all of the following are true:

- [ ] `.xlsx` with no macros  
- [ ] Dashboard shows Target, Daily, Weekly, Monthly **side by side**, plus settings inputs  
- [ ] No view-mode dropdowns, slicers, or toggles  
- [ ] Saved $ uses monthly income × inclusive months − all-time spend  
- [ ] Weekly is Mon–Sun; weekend scorecards are Sat+Sun only  
- [ ] Date Projection formula matches the spec; `Never` when save rate ≤ 0; red if after Target End  
- [ ] Weekly/Monthly spent projections present; red if above that period’s allowance; Daily has no projection  
- [ ] Numbers display as integers; calculation precision kept  
- [ ] Transactions: date + time band dropdown + two tags + 9 categories + amount  
- [ ] Analytics: overall spent scorecard + independent scorecards for every filter value  
- [ ] 4×3 unfiltered time-series charts (week/month/year/all-time × spent / cumulative / vs allowance)  
- [ ] 5 rolling-14-day charts with fixed series (2 / 4 / 2 / 2 / 9 lines)  
- [ ] Sample transactions included so charts have shapes  
- [ ] README explains where to type settings, where to add spend, and that Analytics is scroll-down not a filter UI  

Build the file now.

**END PROMPT**
