# Build Guide — HR Employee Attrition Analysis

Two parts: **(A) EDA notebook in Python** and **(B) Tableau Public dashboard**. Each step has a ✅ checkpoint.
Total time: about 6–8 hours.

---

## Part A — EDA notebook

### A1. Setup
1. Unzip the project, e.g. to `D:\HR_Attrition\hr-attrition`.
2. Open a terminal in that folder and run:
   ```
   pip install -r requirements.txt
   ```
3. Open the folder in VS Code → open `notebooks/hr_attrition_eda.ipynb` → **Select Kernel** → your Python.

The data (`data/emp_attrition.csv`) is already included, so there's nothing to download.

### A2. Run it
Click **Run All**. Takes about 20–30 seconds.

✅ **Checkpoints:**

| Section | Should show |
|---|---|
| 1 | `(1470, 35)` |
| 2 | 0 missing, 0 duplicates; constant columns `EmployeeCount`, `Over18`, `StandardHours`; rate columns correlate ≈ 0 with MonthlyIncome; after dropping: `(1470, 29)` |
| 4 | `237 of 1470 employees left = 16.1% (95% CI 14.3% to 18.1%)` |
| 5 | Sales Representative 39.8%, Overtime Yes 30.5% vs No 10.4% |
| 6 | `43% of all leavers left within their first 2 years` |
| 8 | Top categorical: OverTime (V = 0.244); top numeric: TotalWorkingYears and MonthlyIncome (≈ 0.31) |
| 9 | Overtime + Q1 income = 58%; Overtime + Level 1 + Single = 67.8% (59 people) |
| 11 | `Saved 1470 rows` → creates `tableau/hr_attrition_tableau.csv` |

The 7 charts are saved to `images/`.

### A3. Understand it (important for interviews)
Don't just run it. For each section, make sure you can answer:
- **Section 2:** Why were DailyRate / HourlyRate / MonthlyRate dropped?
- **Section 4:** What does a 95% confidence interval mean, and why does the HR role's interval look so wide?
- **Section 8:** What's the difference between a p-value and an effect size? Why two panels?
- **Section 9:** Why does overtime matter more for low-paid staff?
- **Section 10:** Why is "performance rating doesn't matter" a weak conclusion here?

The study guide (later) covers all of these.

---

## Part B — Tableau Public dashboard
Follow **[TABLEAU_GUIDE.md](TABLEAU_GUIDE.md)**: install, calculated fields, 7 sheets, dashboard, publish.

✅ Final check: your dashboard is live on Tableau Public, and clicking **Sales** filters every chart.

---

## Part C — Finish the README
1. Screenshot the published dashboard → `images/08_tableau_dashboard.png`.
2. In `README.md`, replace `[Tableau Public link](#)` with your real URL.
3. Update the LinkedIn/GitHub links in the footer.

## Part D — GitHub
1. New repo: `hr-employee-attrition-eda` → Public.
2. **Upload what's INSIDE the `hr-attrition` folder** (so `README.md` is at the top level).
3. About → Description: `Exploratory data analysis of employee attrition with statistical tests, risk-factor combinations and a Tableau Public dashboard.`
4. Topics: `eda`, `data-visualization`, `tableau`, `python`, `seaborn`, `hr-analytics`, `employee-attrition`, `statistics`
5. Pin it.
