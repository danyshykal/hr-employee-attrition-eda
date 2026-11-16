# Tableau Public Dashboard Guide

Build an interactive attrition dashboard from `tableau/hr_attrition_tableau.csv` (created by the notebook, section 11) and publish it online with a shareable link.

Time: 3–5 hours. Every sheet has a ✅ checkpoint.

---

## Step 0 — Install
1. Download **Tableau Public** (free) from public.tableau.com → "Create" → download the desktop app.
2. Create a free Tableau Public account (you need it to save, because Tableau Public saves to the web).

---

## Step 1 — Connect the data
1. Open Tableau Public → **Connect → To a File → Text file** → choose `tableau/hr_attrition_tableau.csv`.
2. In the data preview, check:
   - `Left` and `JobLevel` show as numbers (#). If `JobLevel` is a number, right-click → **Convert to Dimension** (it's a category, not something to add up).
   - `StockOptionLevel`: also **Convert to Dimension**.
3. Click **Sheet 1** at the bottom.

✅ 1,470 rows, 25 fields.

---

## Step 2 — Calculated fields
**Analysis → Create Calculated Field**, one at a time:

| Name | Formula |
|---|---|
| `Employees` | `COUNT([Attrition])` |
| `Leavers` | `SUM([Left])` |
| `Attrition Rate` | `SUM([Left]) / COUNT([Left])` |
| `Company Rate` | `{ FIXED : SUM([Left]) / COUNT([Left]) }` |
| `Above Average` | `IF [Attrition Rate] > MIN([Company Rate]) THEN "Above average" ELSE "At or below average" END` |

Right-click **Attrition Rate** and **Company Rate** → **Default Properties → Number Format → Percentage, 1 decimal**.

**Why `Company Rate` uses FIXED:** it ignores the dimensions in the view, so it always shows the overall 16.1% and can be drawn as a reference line on every chart. (Filters on the dashboard won't change it either, unless you add them to context.)

---

## Step 3 — Fix the sort order of bands
Text bands sort alphabetically (`1 yr`, `11+ yrs`, `2 yrs`…), which is wrong.

Right-click **TenureBand** → **Default Properties → Sort → Manual** → order:
`<1 yr`, `1 yr`, `2 yrs`, `3-5 yrs`, `6-10 yrs`, `11+ yrs`

Do the same for:
- **AgeBand:** `18-25`, `26-30`, `31-35`, `36-45`, `46-60`
- **IncomeQuartile:** `Q1 (lowest)`, `Q2`, `Q3`, `Q4 (highest)`
- **JobSatisfaction / EnvironmentSatisfaction:** `Low`, `Medium`, `High`, `Very High`

---

## Step 4 — Build the sheets
Rename each sheet (double-click the tab).

### Sheet 1: `KPI`
- Drag **Employees**, **Leavers**, **Attrition Rate** onto the **Text** mark. Use **Measure Names** on Columns and **Measure Values** on Text if you want them side by side.
- Format as big numbers (font 20+).

✅ 1,470 · 237 · 16.1%

### Sheet 2: `By Role`
- Rows: **JobRole** · Columns: **Attrition Rate** · sort descending.
- Colour: **Above Average** (orange for above, grey for below).
- **Analytics** pane → drag **Reference Line** onto the chart → Table → Value: **Company Rate** (Minimum) → label "Company 16.1%", dashed line.
- Label: show **Attrition Rate** on the bars.

✅ Sales Representative 39.8% at the top, Research Director 2.5% at the bottom.

### Sheet 3: `Overtime x Pay` (highlight table)
- Rows: **OverTime** · Columns: **IncomeQuartile**
- Marks: **Square** · Colour: **Attrition Rate** (orange sequential) · Label: **Attrition Rate** and **Employees**.

✅ OverTime = Yes, Q1 (lowest) = **58.5%** (n = 106). OverTime = No, Q4 = 7.2%.

### Sheet 4: `Tenure`
- Columns: **TenureBand** · Rows: **Attrition Rate** · Marks: **Line** (and turn on markers).
- Add the same **Company Rate** reference line.

✅ <1 yr 36.4% → 1 yr 34.5% → 2 yrs 21.3% → … → 11+ yrs 8.1%.

### Sheet 5: `Age`
- Columns: **AgeBand** · Rows: **Attrition Rate** · Marks: **Bar** · Colour: **Above Average**.

✅ 18-25 = 35.8%, 36-45 = 9.2%.

### Sheet 6: `Satisfaction`
- Columns: **JobSatisfaction** · Rows: **Attrition Rate** · Marks: Bar.
- Optional: duplicate for **EnvironmentSatisfaction** and **WorkLifeBalance**.

✅ Low = 22.8%, Very High = 11.3%.

### Sheet 7: `By Department` (used as a filter)
- Rows: **Department** · Columns: **Attrition Rate** · Label on.

✅ Sales 20.6%, HR 19.0%, R&D 13.8%.

---

## Step 5 — Dashboard
1. **New Dashboard** (bottom tab icon) → Size: **Fixed, 1200 × 800** (or "Automatic" for full width).
2. Layout suggestion:
   ```
   ┌──────────────────────────────────────────────────────────────┐
   │ Title: "Who leaves, when, and why?"    [KPI: 1,470 · 237 · 16.1%] │
   ├─────────────────────┬─────────────────────┬──────────────────┤
   │ By Department       │ By Role             │ Overtime x Pay   │
   ├─────────────────────┴───────┬─────────────┴──────────────────┤
   │ Tenure                      │ Age             │ Satisfaction │
   └─────────────────────────────┴─────────────────┴──────────────┘
   ```
3. Add filters: click a sheet → ▼ menu → **Filters** → **Gender**, **MaritalStatus**, **BusinessTravel** → **Apply to Worksheets → All Using This Data Source**.
4. **Dashboard action:** Dashboard → Actions → Add Action → **Filter** → Source: `By Department` → Targets: all other sheets → Run on: **Select**. Clicking "Sales" now filters the whole dashboard.
5. Add a small text box at the bottom: *"Data: IBM HR Analytics Employee Attrition (fictional dataset created by IBM)."*

✅ Click **Sales** in By Department: By Role shows only Sales Representative 39.8%, Sales Executive 17.5%, Manager 5.4%.

---

## Step 6 — Publish
1. **File → Save to Tableau Public As…** → sign in → name: `HR Employee Attrition Analysis`.
2. Your browser opens the published dashboard. Copy the URL.
3. On the Tableau Public page: click **Edit Details** → add a description and the GitHub repo link.
4. Take a screenshot of the full dashboard → save as `images/08_tableau_dashboard.png`.
5. Paste the Tableau Public URL into the README (the `[Tableau Public link](#)` placeholder).

---

## Design rules (what makes it look professional)
- **One accent colour** (orange) for "above average" only; everything else grey.
- Every chart title states the finding, not the topic: *"Sales Reps leave at 2.5× the average"* beats *"Attrition by Job Role"*.
- The company-average reference line on every rate chart, so readers always have a benchmark.
- No 3-D, no pie charts with more than 3 slices, no gridlines that aren't needed.
