# 📊 Power BI Dashboard Guide
## Hospital Patient Analytics

> **Goal:** Build a 3-page interactive dashboard that lets hospital management monitor patient flow, department performance, and satisfaction trends.

---

## Prerequisites
- Power BI Desktop installed (free from microsoft.com/powerbi)
- `healthcare_dataset.xlsx` file from the `data/` folder

---

## Step 1: Load the Data

1. Open **Power BI Desktop**
2. Click **Home → Get Data → Excel Workbook**
3. Browse to `healthcare_dataset.xlsx` and click **Open**
4. In the Navigator panel, tick all 3 sheets:
   - ✅ `Admissions`
   - ✅ `Department_Info`
   - ✅ `Monthly_KPIs`
5. Click **Transform Data** to open Power Query

---

## Step 2: Clean the Data in Power Query

### Admissions table
1. Select the `Admissions` query
2. Click `admission_date` column → **Change Type → Date**
3. Click `discharge_date` column → **Change Type → Date**
4. Click `age` column → **Change Type → Whole Number**
5. Click `wait_time_mins` column → **Change Type → Whole Number**
6. Click `length_of_stay_days` column → **Change Type → Decimal Number**
7. Click `satisfaction_score` column → **Change Type → Decimal Number**

### Add calculated columns in Power Query
1. With `Admissions` selected, click **Add Column → Custom Column**
2. Name it `Age Group`, formula:
```
if [age] < 18 then "Under 18"
else if [age] < 40 then "18-39"
else if [age] < 60 then "40-59"
else if [age] < 80 then "60-79"
else "80+"
```
3. Add another Custom Column named `Wait Time Category`:
```
if [wait_time_mins] <= 15 then "Excellent (≤15 min)"
else if [wait_time_mins] <= 30 then "Good (16-30 min)"
else if [wait_time_mins] <= 60 then "Fair (31-60 min)"
else "Poor (>60 min)"
```
4. Click **Close & Apply**

---

## Step 3: Create Relationships

1. Click the **Model** icon (left sidebar)
2. You'll see the 3 tables — drag to create these relationships:
   - `Admissions[department]` → `Department_Info[department]` (Many to One)
3. The `Monthly_KPIs` table is standalone (pre-aggregated) — no relationship needed

---

## Step 4: Create DAX Measures

Click on the `Admissions` table, then **Home → New Measure** for each:

```dax
-- Total Admissions
Total Admissions = COUNTROWS(Admissions)

-- Average Wait Time
Avg Wait Time (mins) = AVERAGE(Admissions[wait_time_mins])

-- Average Length of Stay
Avg LOS (days) = AVERAGE(Admissions[length_of_stay_days])

-- Average Satisfaction
Avg Satisfaction = AVERAGE(Admissions[satisfaction_score])

-- Readmission Rate
Readmission Rate % = 
DIVIDE(
    COUNTROWS(FILTER(Admissions, Admissions[readmitted_30d] = "Yes")),
    COUNTROWS(Admissions),
    0
) * 100

-- Mortality Rate
Mortality Rate % =
DIVIDE(
    COUNTROWS(FILTER(Admissions, Admissions[outcome] = "Deceased")),
    COUNTROWS(Admissions),
    0
) * 100

-- % Meeting Wait Time Target (under 30 mins)
% Meeting Wait Target =
DIVIDE(
    COUNTROWS(FILTER(Admissions, Admissions[wait_time_mins] <= 30)),
    COUNTROWS(Admissions),
    0
) * 100
```

---

## Step 5: Build Page 1 — Executive Overview

**Rename the page:** Double-click "Page 1" tab → type `Executive Overview`

### KPI Cards (top row)
Add 4 **Card** visuals with these measures:
| Card | Measure |
|------|---------|
| 1 | Total Admissions |
| 2 | Avg Wait Time (mins) |
| 3 | Avg Satisfaction |
| 4 | Readmission Rate % |

Format each card: **Format → Callout value → Font size 28, Bold**

### Monthly Admissions Trend (Line Chart)
1. Insert **Line Chart**
2. X-axis: `Monthly_KPIs[month]`
3. Y-axis: `Monthly_KPIs[total_admissions]`
4. Title: "Monthly Admissions Trend"

### Admissions by Department (Bar Chart)
1. Insert **Clustered Bar Chart**
2. Y-axis: `Admissions[department]`
3. X-axis: `Total Admissions` measure
4. Sort descending by value
5. Title: "Admissions by Department"

### Outcome Breakdown (Donut Chart)
1. Insert **Donut Chart**
2. Legend: `Admissions[outcome]`
3. Values: `Total Admissions`
4. Title: "Patient Outcomes"

### Slicers (filters)
Add 3 **Slicer** visuals:
- `Admissions[admission_date]` → set to **Between** style (date range)
- `Admissions[department]` → set to **Dropdown** style
- `Admissions[admission_type]` → set to **Tile** style

---

## Step 6: Build Page 2 — Department Performance

**Add new page** → rename to `Department Performance`

### Wait Time vs Target (Clustered Column Chart)
1. Insert **Clustered Column Chart**
2. X-axis: `Department_Info[department]`
3. Y-axis (Column 1): `Avg Wait Time (mins)`
4. Y-axis (Column 2): `Department_Info[target_wait_mins]`
5. Title: "Actual vs Target Wait Time by Department"

### Length of Stay vs Target (Same approach)
1. Insert another **Clustered Column Chart**
2. X-axis: `Department_Info[department]`
3. Column 1: `Avg LOS (days)`
4. Column 2: `Department_Info[target_los_days]`
5. Title: "Actual vs Target Length of Stay"

### Bed Occupancy Gauge
1. Insert **Gauge** visual
2. Value: `Total Admissions`
3. Target: `Department_Info[bed_capacity]` (sum)
4. Title: "Overall Bed Utilisation"

### Department Scorecard (Table)
1. Insert **Table** visual
2. Columns: `department`, `Total Admissions`, `Avg Wait Time (mins)`, `Avg LOS (days)`, `Avg Satisfaction`
3. Add **Conditional Formatting** on wait time:
   - Format → Conditional formatting → Background color
   - Green if < 30, Yellow if 30-45, Red if > 45

---

## Step 7: Build Page 3 — Patient Insights

**Add new page** → rename to `Patient Insights`

### Admissions by Age Group (Bar Chart)
1. Insert **Clustered Bar Chart**
2. Y-axis: `Admissions[Age Group]`
3. X-axis: `Total Admissions`
4. Title: "Admissions by Age Group"

### Satisfaction by Department (Column Chart)
1. Insert **Clustered Column Chart**
2. X-axis: `Admissions[department]`
3. Y-axis: `Avg Satisfaction`
4. Add a constant line at 3.8 (average): **Analytics pane → Constant line → 3.8**
5. Title: "Patient Satisfaction by Department"

### Admissions by Region (Map or Bar Chart)
1. Insert **Clustered Bar Chart**
2. Y-axis: `Admissions[region]`
3. X-axis: `Total Admissions`
4. Title: "Admissions by Region"

### Readmission Rate by Department (Column Chart)
1. Insert **Clustered Column Chart**
2. X-axis: `Admissions[department]`
3. Y-axis: `Readmission Rate %`
4. Title: "30-Day Readmission Rate by Department"

### Gender Breakdown (Pie Chart)
1. Insert **Pie Chart**
2. Legend: `Admissions[gender]`
3. Values: `Total Admissions`
4. Title: "Admissions by Gender"

---

## Step 8: Formatting & Theming

### Apply a theme
1. **View → Themes → Browse for themes**
2. Or use built-in: **View → Themes → Executive**

### Consistent colours
Use this colour palette across all visuals:
| Colour | Hex | Use for |
|--------|-----|---------|
| Navy Blue | `#1F4E79` | Headers, primary bars |
| Sky Blue | `#2E75B6` | Secondary elements |
| Orange | `#F97316` | Targets / benchmarks |
| Green | `#16A34A` | Positive metrics |
| Red | `#DC2626` | Alerts / negative metrics |

### Page background
1. Click on blank canvas area
2. **Format → Canvas background → Color → #F0F4F8**

### Add title text boxes to each page
Insert → Text box → type the page title in large bold text

---

## Step 9: Save & Export

1. **File → Save As** → name it `Hospital_Analytics_Dashboard.pbix`
2. To export as PDF: **File → Export → Export to PDF**
3. To publish online: **Home → Publish** (requires Power BI account)

---

## 📸 Screenshot Your Dashboard
Once built, take screenshots of all 3 pages and save them to the `images/` folder in this repo.

---

## 💡 Key Insights to Highlight in Your Portfolio
- Emergency department has the highest wait times — flag as improvement area
- Winter months (June–August) show higher admission volumes
- Oncology has the lowest satisfaction score despite longest average stay
- 12% readmission rate is above the 10% industry benchmark
