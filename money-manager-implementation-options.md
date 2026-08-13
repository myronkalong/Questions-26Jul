# Money Manager — Implementation Options

This note evaluates **how to implement the original requirement list** (toggles on the main screen, combined filters on Analytics). It is not a build. Four options, then a comparison.

---

## 1. What you are actually building

Two jobs, one product:

| Job | Frequency | What must feel fast |
|---|---|---|
| Log a spend | Several times a day | Date, 4 dropdowns, amount, save |
| Steer money | Daily glance + weekly review | Target progress, allowance vs spend, “am I on track?” |

Everything else is a **derived engine** on top of two tables:

1. **Settings** (entered once, edited rarely): Income, Target, Target Start, Target End, Daily Allowance.
2. **Transactions** (ongoing): Date, Time Band, Essential/Non-Essential, Occasion/Reoccurrence, Category, Amount.

There is no account feed, no login, no multi-user, and no need to store exact clock time if Time Band is a chosen label.

### Main screen (original)

Two independent panels:

- **Target View** — Saved, Target, Saved/Target %, date projection from past saving rate.
- **Period View** — Daily / Weekly / Monthly toggle: Spent, Allowance, Spent/Allowance %, plus a spend projection on Weekly and Monthly only.

### Analytics (original)

One **combined filter** (Weekday/Weekend × Time Band × Essential × Occasion/Reoccurrence × Category) applied to:

- the transaction list
- the Spent $ scorecard
- three time-series charts (daily spend, cumulative spend, spend vs daily-allowance baseline)

and **not** applied to:

- a rolling-14-day chart that splits spend by **one** chosen dimension (one line per value of that dimension).

Chart windows: Weekly / Monthly (default) / Yearly / All time. X is calendar day.

### Display

Show money and percents as integers. Keep full precision in the engine; round only for display.

---

## 2. Shared data model (all options)

Use this model even in a spreadsheet. Names below are logical, not a stack.

```
Settings (1 row)
  income
  target
  target_start
  target_end
  daily_allowance

Transaction
  id
  date              // calendar date in HKT
  time_band         // morning | afternoon | evening | night
  essential         // essential | non_essential
  recurrence        // occasion | reoccurrence
  category          // 9 closed values
  amount            // number, not rounded
```

Derived columns (do not type these; compute them):

```
weekday_type = weekend if Saturday/Sunday else weekday
saved        = income_window − spent_window     // see spec gaps below
```

Closed lists:

| Field | Values |
|---|---|
| Time Band | Morning `05:00–11:29`, Afternoon `11:30–16:59`, Evening `17:00–21:59`, Night `22:00–04:59` |
| Essential | Essential, Non-Essential |
| Recurrence | Occasion, Reoccurrence |
| Category | Food & Dining, Groceries, Transport, Shopping & Entertainment, Subscriptions, Health & Personal Care, Travel, Gifts & Donations, Other |

Night wraps midnight. That only matters if you later derive the band from a clock time. If the user **picks** the band, store the label and stop.

---

## 3. Spec gaps that change the build (lock these first)

These are in the original list. Every option has to pick an answer. They are product decisions, not tech decisions.

| Gap | Why it matters | Sensible default |
|---|---|---|
| **What is Income $?** One lump, monthly, or total over the target window? | `Saved = Income − Spent` is wrong if Income is monthly and Spent is all-time. | Monthly income. All-time income = monthly × inclusive calendar months from start month through current month (no proration). Saved = all-time income − all-time spent. |
| **Weekly spent is Mon–Fri, projection is Mon–Sun** | Dashboard week and projection week disagree. | One week: Monday–Sunday. Spent so far = Mon through today. Projection scales to 7 days. |
| **Target End Date is collected but unused** | Dead input unless it drives an alert. | Keep the field. If projected date is after Target End, show the date in red. If save rate ≤ 0, show `Never` in red. |
| **Date projection formula** | `Start + Target / (Saved / days)` is “when Target would be hit at the historical average daily save rate, measured from Start” — not “today + remaining / rate”. Same answer only while Saved grew linearly from Start. Day 0 and Saved ≤ 0 divide by zero. | Keep the written formula. On day 0, Saved ≤ 0, or save rate ≤ 0 → `Never`. |
| **“Days between Current Date and Target Start Date”** | Inclusive vs exclusive. On the start date this is 0. | `current_date − target_start` in whole days (0 on start date). |
| **Spent vs Allowance chart, line 2 = “Sum of Spent $”** | Daily total or running total? | Daily spent that day vs a **horizontal** daily-allowance line. Cumulative belongs on the other chart. |
| **Filter combination** | Empty vs “all values”. Can the user pick Food **and** Transport? | Multi-select per dimension, AND across dimensions. Empty dimension = no restriction. |
| **Rolling 14-day “by one of the filter”** | Picker vs five charts. | One chart + a dimension picker is the original. Five always-on charts is the Excel-friendly version. |
| **Integer display** | `ROUND`, floor, or banker’s rounding. Percents: `67%` or `67`. | Display-only: money `#,##0`, percents `0%`. Tests and alerts use unrounded values. Do not integer-format dates. |
| **Current date / HKT** | Server TZ vs phone TZ vs PC clock. | Current **date** = the device clock. No timezone engine unless you store exact times. |
| **Weekend** | Fri night? HK public holidays? | Saturday and Sunday only. |

Until those are locked, “implement the formulas” will fork.

---

## 4. Option A — Excel workbook (local `.xlsx`)

**Idea:** Settings + Transactions table + formula Dashboard + formula Analytics. No macros.

**How the original UX maps:**

| Original control | Excel reality |
|---|---|
| Target / Daily / Weekly / Monthly toggles | Four panels side by side. Toggles are possible (form controls / `CHOOSE`) but worse than showing all four. |
| Combined Analytics filter | PivotTables + slicers, **or** `SUMIFS` scorecards with dropdowns. Slicers want to filter the whole sheet. |
| Charts that **ignore** the combined filter (rolling 14-day) | Need a **second** data cache (helper sheet / second PivotCache). This is the painful part. |
| Chart window Weekly / Monthly / Yearly / All time | Four chart copies, or one chart + a window dropdown feeding helper ranges. |

**Workbook shape:**

- `Dashboard` — settings row; Target / Daily / Weekly / Monthly metrics.
- `Transactions` — Excel Table, data-validation dropdowns.
- `Analytics` — list (or link to Transactions), scorecards, charts.
- Hidden `_Calc` — one row per calendar day: daily spend, cumulative, allowance baseline, rolling-14 by dimension.

**Fits well**

- The derived formulas are spreadsheet-native (`SUMIFS`, `TODAY()`, `WEEKDAY`).
- You can audit every number by clicking the cell.
- Zero hosting, zero login, file is the backup.
- Matches how this repo has already been used (Excel models).

**Fits poorly**

- The original Analytics page wants **two filter scopes on one screen**. Excel slicers do not. You either flatten the page (independent scorecards + independent charts, no slicers) or you fight PivotCaches.
- Phone logging is clumsy.
- All-time daily series for charts can get long; keep helper rows bounded (e.g. from min(start, first txn) through today).
- Easy to break a named range or table expansion.

**When to pick it:** personal tool, desktop-first, you want it this week, and you can live without combined slicers.

---

## 5. Option B — Google Sheets (optional Google Form for capture)

**Idea:** Same model as Excel, in the cloud. Phone can append a row. Optional Form: Date, Time Band, tags, Category, Amount.

**How the original UX maps:** same as Excel, with slightly better mobile entry and worse chart control.

**Fits well**

- Logging from a phone browser without installing an app.
- Automatic backup / multi-device of the **file**.
- `QUERY` / `FILTER` / `SPARKLINE` cover list + scorecard.
- Sharing later (partner, accountant) is a checkbox.

**Fits poorly**

- Charts and “filter does not apply to this graph” are weaker than Excel.
- `TODAY()` follows the spreadsheet locale/timezone, not “HKT” unless you set the file TZ to `Asia/Hong_Kong`.
- Offline logging is unreliable.
- Apps Script can fake a sidebar UI; that is a small web app in a worse runtime.

**When to pick it:** you want Option A, but capture must work on a phone, and you accept weaker Analytics.

---

## 6. Option C — Local-first web app (PWA)

**Idea:** One web app that installs on phone and laptop. Data stays on the device (IndexedDB). Optional later: file export / iCloud-style sync. No server required for v1.

**How the original UX maps:** this is the only option that implements the written UI without flattening it.

```
Settings (1)
Transactions (N)
        │
        ▼
  derived engine (pure functions)
        │
        ├── Main: Target panel + period toggle (D/W/M)
        └── Analytics
              ├── filter state  → list, scorecard, 3 charts
              └── group-by dim  → rolling 14-day (filters ignored)
```

**Suggested stack (illustrative, not mandatory):**

| Layer | Choice | Why |
|---|---|---|
| UI | React or Vue, one CSS kit | Toggles, filter chips, two pages |
| Charts | ECharts / Chart.js / uPlot | Several line series, daily X |
| Storage | IndexedDB (Dexie) | Transactions grow; localStorage is a poor fit |
| Engine | Pure TS/JS module + unit tests | Same formulas as the spec; rounding only in the view |
| Shell | Vite PWA | Add-to-home-screen, offline log |

**Screens:** `Dashboard` · `Add spend` (one thumb-friendly form) · `Analytics` · `Settings`.

**Fits well**

- Toggle 1 / Toggle 2 and combined filters are ordinary React state.
- List, scorecard, and the three charts share one filter object; the rolling chart uses a second object. That dual scope is a 20-line concern here and a structural fight in Excel.
- Integer display, red alerts, `Never` are trivial.
- Daily logging on a phone is a first-class screen.
- Formulas can be unit-tested (Excel cannot).
- Privacy: nothing leaves the device until you export.

**Fits poorly**

- You must design the UI (spreadsheet gives you a grid for free).
- A cleared browser profile can wipe data unless you add export (JSON/CSV) or a backup file.
- Charts need a date-range helper (one point per day, including $0 days).
- More moving parts than a workbook.

**When to pick it:** you want the requirement list as written, including combined filters and a rolling chart that ignores them.

---

## 7. Option D — Native mobile app

**Idea:** iOS/Android (SwiftUI, Kotlin, or Flutter) with SQLite. Dashboard + quick-add + Analytics.

**How the original UX maps:** capture is excellent; Analytics on a phone needs a different layout (filters as a sheet, charts swipeable, not one long desktop page).

**Fits well**

- Fastest path to “I paid $42 for lunch, log it in 10 seconds.”
- OS backup (iCloud / Google) if you opt in.
- HKT date/time from the system calendar.
- Notifications (daily allowance warning) if you ever want them.

**Fits poorly**

- The Analytics page as specified is a **desktop** dashboard (many charts, long list, many scorecards). You will redesign it, not port it.
- Two platforms, store listing, and release process — none of which the spec needs.
- Harder to audit formulas than a spreadsheet or a tested JS module you can run on a laptop.

**When to pick it:** the habit of logging is the product, and you already live on your phone. Build Analytics as a secondary, simplified surface — or pair a native logger with Option C’s Analytics later.

---

## 8. What each option does with each requirement

| Requirement | A Excel | B Sheets | C PWA | D Native |
|---|---|---|---|---|
| Target metrics | Cells | Cells | Panel | Panel |
| Daily / Weekly / Monthly | Four panels, or a toggle cell | Same as Excel | Toggle (as written) | Segmented control |
| Weekly / Monthly spend projection | Formulas | Formulas | Engine | Engine |
| Log spend: date + 4 tags + amount | Table + dropdowns | Table / Form | Form | Form |
| Combined filter → list | Slicer / `FILTER` | `FILTER` / views | Client filter | Client filter |
| Combined filter → scorecard | `SUMIFS` / Pivot | `SUMIFS` / `QUERY` | Reduce in engine | Same |
| Combined filter → 3 time-series charts | Helper ranges or PivotCharts | Weaker | Shared filter state | Same, cramped |
| Rolling 14-day **ignores** that filter | Second cache / no slicers | Awkward | Separate query | Separate query |
| Chart window W/M/Y/All | Four charts or a dropdown | Same | Dropdown (as written) | Dropdown |
| Integer display | Number formats | Number formats | Format in the view | Format in the view |
| HKT time bands as labels | Validation list | Validation list | Enum | Enum |
| Offline | File on disk | Weak | Yes (PWA) | Yes |
| Phone logging | Poor | OK (Form) | Good | Best |

---

## 9. Comparison

Scores are relative to **this spec**, for a single person, not a startup. 5 = strong fit.

| Criterion | A Excel | B Sheets | C PWA | D Native |
|---|---|---|---|---|
| Faithfulness to original toggles + dual filter scopes | 2 | 2 | 5 | 4 |
| Correct, auditable formulas | 5 | 4 | 5 (if tested) | 3 |
| Time to a usable v1 | 5 | 5 | 3 | 2 |
| Daily logging friction | 2 | 3 | 4 | 5 |
| Analytics density (many charts on one page) | 4 | 3 | 5 | 2 |
| Phone + laptop without a server | 2 | 4 | 5 | 3 (phone-first) |
| Data you control / privacy | 5 | 3 | 5 | 4 |
| Risk of “one wrong formula” in production | 3 | 3 | 2 | 3 |
| Cost to run | 5 (you already have Excel) | 5 | 5 (static host or local) | 3 (dev accounts) |
| Later: bank CSV import, extra categories | 3 | 3 | 5 | 4 |
| Later: second user / sync | 1 | 4 | 3 | 3 |

**Read the table as two clusters:**

- **Spreadsheet cluster (A, B)** — fastest path to the **numbers**. Weak on the **original Analytics interaction model**. You will flatten toggles and filters (four dashboard panels; independent scorecards; independent charts). That is a product change, not a small UI compromise.
- **App cluster (C, D)** — can ship the **written** UI. C keeps desktop Analytics. D wins at capture and loses at the dashboard-as-specified.

---

## 10. Recommendation

**If the goal is “I want this money manager in my life”:** start with **Option A (Excel)**. Put Settings, Transactions, and the four metric panels on one Dashboard; put list + scorecards + charts on Analytics; do **not** attempt combined slicers plus unfiltered rolling charts on the same Pivot cache. Flatten Analytics: overall + per-dimension scorecards, four time windows as separate charts, five rolling-14-day charts (one per dimension). That is the honest spreadsheet product.

**If the goal is “implement this requirement list as written”:** choose **Option C (local-first PWA)**. The dual filter scope, period toggle, and rolling-14 group-by picker are ordinary application state. Put every formula in one pure module and test the spec gaps in section 3 as fixtures (start-date day 0, Saved ≤ 0, night band, Mon–Sun week, integer display vs unrounded alerts).

**Do not start with Option D** unless logging-on-phone is the only thing you care about. **Do not start with a hosted full-stack app** (accounts, Postgres, cloud charts). Nothing in the list needs it.

### Practical sequence

1. Lock the table in section 3 (income window, week definition, end-date alert, chart line 2, rounding).
2. Write the derived engine as a one-page formula spec (or a 50-line TS module). Same math for every option.
3. Pick A or C from the recommendation above.
4. v1 scope: Settings, transaction log, Dashboard metrics, one unfiltered spend-over-time chart. Then add combined filters. Then add rolling 14-day. Do not start with twelve charts.

### What not to build in v1

- Bank / FPS / Alipay import
- Per-category budgets
- Exact clock time (unless you drop the Time Band picker)
- Multi-currency
- Auth, sync, sharing
- Excel VBA / Apps Script UI pretending to be Option C
