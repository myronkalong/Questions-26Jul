# Money Manager — Requirement List

How to read this file: **yellow highlight** (`<mark>`) means the text was **added or changed** from the original list. Unhighlighted text is unchanged.

---

## Changelog (vs original)

1. <mark>Main screen: no toggles. Target, Daily, Weekly, and Monthly views sit on one page, side by side.</mark>
2. <mark>Analytics: no interactive filters, slicers, or view-switchers. Each former filter / time-range option is its own independent view on the same page.</mark>
3. <mark>Do not cross every filter × every time range × every chart type (that would explode the page). Time-series charts show all spending. Filter breakdowns are independent scorecards plus independent rolling-14-day charts.</mark>
4. <mark>Income is monthly. Saved $ = all-time income − all-time spent.</mark>
5. <mark>All-time income = monthly income × inclusive calendar months from Target Start Date’s month through the current month. No proration. 0 if today is before Target Start Date.</mark>
6. <mark>Weekly window is Monday–Sunday (was Monday–Friday for Spent $).</mark>
7. <mark>Weekend = Saturday and Sunday.</mark>
8. <mark>Target End Date: Date Projection turns red if the projected date is after Target End Date.</mark>
9. <mark>Weekly / Monthly Spent Projection turns red if it is higher than that view’s Allowance.</mark>
10. <mark>If overspend / save rate ≤ 0, Date Projection shows `Never` (red).</mark>
11. <mark>Rounding is display-only. Calculations keep full precision.</mark>
12. <mark>Category spelling: Food & Dining (was “Dinning”).</mark>
13. <mark>PC clock is the current date. Time Band is a chosen label, not derived from a typed clock time.</mark>
14. <mark>Assume typed / dropdown input only (no extra anti-paste rules). Single-user file. Sheet hyperlinks are enough for navigation.</mark>
15. <mark>Rolling 14-day chart: one independent chart per filter dimension, each with a fixed number of lines (decision for the old “switch which filter to plot” control).</mark>

---

## Main Screen

<mark>One page. Four views side by side. No toggles.</mark>

Layout, left to right on the same screen:

1. <mark>Target View</mark>
2. <mark>Daily View</mark>
3. <mark>Weekly View</mark>
4. <mark>Monthly View</mark>

#### Target View

- Saved $
- Target $
- Saved / Target %
- Date Projection based on Past Saving Rate
  - <mark>If the projected date is after Target End Date, the Date Projection value is shown in red.</mark>
  - <mark>If Saved $ ≤ 0, or days elapsed since Target Start Date = 0, or saving rate ≤ 0: show `Never` in red. Do not show a date.</mark>

#### Daily View

- Spent $
- Allowance $
- Spent / Allowance %
- <mark>No Spent Projection on Daily View.</mark>

#### Weekly View

- Spent $
- Allowance $
- Spent / Allowance %
- Spent Projection based on Past Spending Rate
  - <mark>If Spent Projection > Allowance $ for this Weekly View, the Spent Projection value is shown in red.</mark>

#### Monthly View

- Spent $
- Allowance $
- Spent / Allowance %
- Spent Projection based on Past Spending Rate
  - <mark>If Spent Projection > Allowance $ for this Monthly View, the Spent Projection value is shown in red.</mark>

---

## Analytics Page

<mark>Same page. Scroll is expected. No interactive filter bar, slicers, or dropdowns that switch which view is visible. Every former toggle / filter / time-range option is an independent view on this page.</mark>

#### List

- Spending per transaction
- <mark>One list of all transactions. Not interactively filtered. Do not duplicate the full list once per filter value.</mark>

#### Scorecard <mark>(independent view per filter — not one scorecard driven by a combined filter)</mark>

- Sum of Spent $ <mark>(overall, all time)</mark>
- <mark>Then one independent scorecard block per filter dimension. Each block shows one scorecard per value (sum of Spent $ for that value, all time). Blocks are not combined with each other.</mark>
  - Weekday or Weekend <mark>— 2 scorecards: Weekday, Weekend</mark>
  - Time Band <mark>— 4 scorecards: Morning, Afternoon, Evening, Night</mark>
  - Tag (Essential or Non-Essential) <mark>— 2 scorecards</mark>
  - Tag (Occasion or Reoccurrence) <mark>— 2 scorecards</mark>
  - Category <mark>— 9 scorecards, one per category</mark>

#### Graph <mark>(all spending; time range is not a toggle)</mark>

<mark>For each graph type below, show four independent charts on the same page, side by side:</mark>

- <mark>This Week</mark>
- <mark>This Month</mark> <mark>(was the only default)</mark>
- <mark>This Year</mark>
- <mark>All Time</mark>

<mark>X is still date (day) in every chart. The four charts are different date windows, not a switcher. These charts are not sliced by Weekday/Weekend, Time Band, tags, or Category. Filter breakdowns live in the scorecards above and the rolling-14-day charts below.</mark>

- Spent $ over time (line chart)
    - X: date (day)
    - Y: $
- Cumulative Spent $ over time (line chart)
    - X: date (day)
    - Y: $
    - <mark>Cumulative resets at the start of that chart’s window (week from Monday, month from the 1st, year from 1 Jan, all-time from the all-time start date).</mark>
- Spent $ vs Allowance (2 line chart)
    - X: date (day)
    - Y: $
    - Line 1: Daily Allowance Baseline
    - Line 2: Sum of Spent $

<mark>This Week = current Monday–Sunday (future days in the week plot 0 spent). This Month = days of the current month (future days plot 0 spent). This Year = days of the current calendar year (future days plot 0 spent). All Time = from min(Target Start Date, earliest transaction date) through Current Date (do not extend All Time into the future).</mark>

#### Graph <mark>(not affected by the scorecard groupings; independent chart per filter dimension)</mark>

- Rolling 14 days Spent $ <mark>— five independent charts on the same page, not “by one of the filters” as a switcher</mark>
    - X: date (day)
        - <mark>All Time only</mark> (was default All time, with an implied switch)
    - Y: $
    - <mark>Each point = spend on that day plus the previous 13 days, for that line’s value only.</mark>
    - <mark>Chart 1: Weekday or Weekend — 2 lines (Weekday, Weekend)</mark>
    - <mark>Chart 2: Time Band — 4 lines (Morning, Afternoon, Evening, Night)</mark>
    - <mark>Chart 3: Essential or Non-Essential — 2 lines</mark>
    - <mark>Chart 4: Occasion or Reoccurrence — 2 lines</mark>
    - <mark>Chart 5: Category — 9 lines, one per category</mark>

---

## User Input (In the beginning)

- Income $ <mark>(monthly)</mark>
- Target $
- Target Start Date
- Target End Date <mark>(used for Date Projection red alert when the projected date is after this date)</mark>
- Allowance (Daily) $

---

## User Input (Ongoing - Spending per transaction)

#### Spending per transaction

- Date
- Time Band (HKT) <mark>— choose the band from a list; do not type an exact clock time. Current date comes from the PC clock.</mark>
    - Morning (05:00 - 11:29)
    - Afternoon (11:30 - 16:59)
    - Evening (17:00 - 21:59)
    - Night (22:00 - 04:59)
- Tag (Essential or Non-Essential)
- Tag (Occasion or Reoccurrence)
- Category
    - Food & <mark>Dining</mark>
    - Groceries
    - Transport
    - Shopping & Entertainment
    - Subscriptions
    - Health & Personal Care
    - Travel
    - Gifts & Donations
    - Other
- Spent Amount $

---

## Derived

<mark>All-time Income $ = Income (Monthly) $ × Number of calendar months from Target Start Date’s month through Current Date’s month, inclusive.</mark>

<mark>If Current Date is before Target Start Date, All-time Income $ = 0.</mark>

<mark>Do not prorate partial months. Example: Target Start Date = 15 Jan 2026, Current Date = 13 Aug 2026 → 8 months × Income (Monthly) $.</mark>

<mark>All-time Spent $ = Total Spent Amount $ from all spending per transaction (every date in the log).</mark>

Saved $ = <mark>All-time Income $ − All-time Spent $</mark>

Saved / Target % = Total Saved $ / Target $ in percentage

Date Projection based on Past Saving Rate = Target Start Date + Target $ / ( Saved $ / Number of days between Current Date and Target Start Date )

<mark>If Saved $ ≤ 0, or Number of days between Current Date and Target Start Date ≤ 0, Date Projection = `Never`.</mark>

<mark>If Date Projection is a date and Date Projection > Target End Date, show Date Projection in red.</mark>

<mark>If Date Projection is `Never`, show it in red.</mark>

Spent $ = Total Spent Amount $ from spending per transaction in a given period ( daily = today, weekly = this week from Monday to <mark>Sunday</mark>, monthly = this month )

Allowance $ = Allowance (Daily) $ for Daily View, Allowance (Daily) $ * 7 for Weekly View, Allowance (Daily) $ * Number of days in a given month for Monthly View

Spent / Allowance % = Spent $ / Allowance $ in percentage

Spent Projection based on Past Spending Rate for Weekly View = Total Spent from Monday to today / Number of days from Monday to today * Number of days from Monday to Sunday

Spent Projection based on Past Spending Rate for Monthly View = Total Spent from 1st of the month to today / Number of days from 1st of the month to today * Number of days in the whole month

<mark>Weekly Spent so far = Monday through Current Date only (do not include future days of this week).</mark>

<mark>Monthly Spent so far = 1st of the month through Current Date only (do not include future days of this month).</mark>

---

## Additional Information

1. Display all numbers as integers
2. <mark>Rounding is display-only. Keep full precision in the underlying values. Red / `Never` tests use unrounded values. Do not integer-format dates. Percents display with 0 decimal places.</mark>
3. <mark>Weekend = Saturday and Sunday only.</mark>
4. <mark>Week = Monday–Sunday.</mark>
5. <mark>Current date = PC clock. No timezone engine.</mark>
6. <mark>Assume nobody pastes junk into the log. Dropdowns for Time Band, tags, and Category are enough.</mark>
7. <mark>Single-user local file. Multi-user / mobile app behaviour is out of scope.</mark>
8. <mark>Navigation may be separate sheets with hyperlinks. No app-like page framework required.</mark>
9. <mark>Charts that would have had a variable number of lines (switch which filter to plot) are instead separate charts with a fixed line count each, all visible on the Analytics page.</mark>
10. <mark>Do not build the cartesian product of filter × time range × chart type.</mark>
