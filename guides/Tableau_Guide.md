# 📊 Tableau Dashboard Guide
## Hospital Patient Analytics

> **Goal:** Build a 3-dashboard Tableau workbook that mirrors the Power BI project, demonstrating cross-tool proficiency.

---

## Prerequisites
- Tableau Desktop (paid) OR **Tableau Public** (free from public.tableau.com)
- `healthcare_dataset.xlsx` from the `data/` folder

> 💡 Tableau Public is free and lets you publish dashboards online — great for your portfolio!

---

## Step 1: Connect to the Data

1. Open Tableau → click **Microsoft Excel** under Connect
2. Browse to `healthcare_dataset.xlsx` → click **Open**
3. You'll see the sheets on the left panel
4. Drag **Admissions** onto the canvas
5. Then drag **Department_Info** → Tableau will ask to create a join:
   - Join type: **Inner Join**
   - Join condition: `Admissions[Department] = Department_Info[Department]`
6. Click the **Sheet 1** tab at the bottom to start building

---

## Step 2: Organise Your Fields

In the left Data panel, rename fields for clarity (right-click → Rename):
- `Wait Time Mins` → `Wait Time (Mins)`
- `Length Of Stay Days` → `Length of Stay (Days)`
- `Satisfaction Score` → `Satisfaction Score (1-5)`

### Create Calculated Fields
Right-click anywhere in the Data panel → **Create Calculated Field**

```
-- Readmission Rate
Name: Readmission Rate %
Formula: SUM(IF [Readmitted 30d] = "Yes" THEN 1 ELSE 0 END) / COUNT([Patient Id]) * 100

-- Mortality Rate
Name: Mortality Rate %
Formula: SUM(IF [Outcome] = "Deceased" THEN 1 ELSE 0 END) / COUNT([Patient Id]) * 100

-- Wait Time Category
Name: Wait Time Category
Formula:
IF [Wait Time Mins] <= 15 THEN "Excellent (≤15 min)"
ELSEIF [Wait Time Mins] <= 30 THEN "Good (16-30 min)"
ELSEIF [Wait Time Mins] <= 60 THEN "Fair (31-60 min)"
ELSE "Poor (>60 min)"
END

-- Age Group
Name: Age Group
Formula:
IF [Age] < 18 THEN "Under 18"
ELSEIF [Age] < 40 THEN "18-39"
ELSEIF [Age] < 60 THEN "40-59"
ELSEIF [Age] < 80 THEN "60-79"
ELSE "80+"
END

-- Met Wait Target (Flag)
Name: Met Wait Target
Formula: IF [Wait Time Mins] <= 30 THEN "Met" ELSE "Missed" END
```

---

## Step 3: Build Sheet 1 — Monthly Admissions Trend

1. Rename Sheet 1 to `Monthly Admissions`
2. Drag `Admission Date` to **Columns** → right-click → select **Month (continuous)**
3. Drag `Patient Id` to **Rows** → right-click → **Measure → Count**
4. Click **Show Me** → select **Line chart**
5. Drag `Admission Type` to **Color** (Marks card)
6. Right-click Y-axis → **Edit Axis** → Title: "Total Admissions"
7. Add a **Reference Line**: Analytics pane → drag "Average" onto the view → Line

**Format:**
- Right-click chart → **Format** → change line colour to `#2E75B6`
- Title: "Monthly Patient Admissions by Type"

---

## Step 4: Build Sheet 2 — Department Wait Times

1. Add new sheet → rename `Wait Time Analysis`
2. Drag `Department` to **Rows**
3. Drag `Wait Time (Mins)` to **Columns** → it becomes AVG automatically
4. Click **Show Me → Horizontal Bar Chart**
5. Drag `Met Wait Target` to **Color**:
   - Right-click `Met` → Edit Color → Green `#16A34A`
   - Right-click `Missed` → Edit Color → Red `#DC2626`
6. Add a **Reference Line** at 30 mins:
   - Analytics pane → drag "Constant Line" → value: 30
   - Label: "30 min target"
7. Sort bars by descending wait time

---

## Step 5: Build Sheet 3 — Satisfaction by Department

1. New sheet → rename `Satisfaction`
2. Drag `Department` to **Columns**
3. Drag `Satisfaction Score (1-5)` to **Rows** (AVG)
4. **Show Me → Bar Chart**
5. Drag `Satisfaction Score (1-5)` to **Color** (Marks card)
6. Edit colors: **Edit Colors → Red-Green Diverging** → center at 3.5
7. Add Reference Line at 3.8 (overall average):
   - Analytics → Constant Line → 3.8 → label "Avg: 3.8"
8. Add **Labels**: Marks card → Label → tick "Show mark labels"

---

## Step 6: Build Sheet 4 — Outcome Breakdown

1. New sheet → rename `Patient Outcomes`
2. Drag `Outcome` to **Rows**
3. Drag `Patient Id` to **Columns** → Count
4. **Show Me → Treemap** (or Pie Chart)
5. Drag `Outcome` to **Color**
6. Drag `Patient Id` (Count) to **Label**
7. Add percentage labels:
   - Right-click label → **Add Table Calculation → Percent of Total**

---

## Step 7: Build Sheet 5 — Age Group Analysis

1. New sheet → rename `Age Analysis`
2. Drag `Age Group` to **Rows**
3. Drag `Patient Id` to **Columns** → Count
4. **Show Me → Horizontal Bar Chart**
5. Drag `Gender` to **Color**
6. Sort by: right-click Y-axis → Sort → Descending by Count
7. Title: "Admissions by Age Group and Gender"

---

## Step 8: Build Sheet 6 — Department Scorecard

1. New sheet → rename `Department Scorecard`
2. Drag `Department` to **Rows**
3. Drag these to **Columns** one by one:
   - `Patient Id` (Count) → rename to "Admissions"
   - `Wait Time (Mins)` (AVG)
   - `Length of Stay (Days)` (AVG)
   - `Satisfaction Score (1-5)` (AVG)
   - `Readmission Rate %`
4. **Show Me → Text Table**
5. Apply conditional formatting:
   - Click `Wait Time (Mins)` pill → **Add Table Calculation**
   - Or: right-click measure → **Format → Pane → Color by value**

---

## Step 9: Build Dashboard 1 — Executive Overview

1. Click **New Dashboard** icon at the bottom
2. Rename to `Executive Overview`
3. Set size: **Fixed Size → 1200 x 800**
4. Drag these sheets onto the canvas:
   - `Monthly Admissions` (top, full width)
   - `Patient Outcomes` (bottom left)
   - `Wait Time Analysis` (bottom right)
5. Add **KPI Text** boxes (Dashboard → Objects → Text):
   - Total Admissions: use a calculated sheet or text box
6. Add Filters that apply to all sheets:
   - Click `Monthly Admissions` sheet on dashboard → More Options (▼) → Filters → `Department`
   - Right-click filter → **Apply to Worksheets → All Using This Data Source**

---

## Step 10: Build Dashboard 2 — Department Performance

1. New Dashboard → rename `Department Performance`
2. Drag onto canvas:
   - `Wait Time Analysis` (top left)
   - `Satisfaction` (top right)
   - `Department Scorecard` (bottom, full width)
3. Add filter: `Admission Date` range filter
4. Add a **Title** object at top: "Department Performance Dashboard"

---

## Step 11: Build Dashboard 3 — Patient Insights

1. New Dashboard → rename `Patient Insights`
2. Drag onto canvas:
   - `Age Analysis` (left)
   - `Patient Outcomes` (top right)
   - `Satisfaction` (bottom right)
3. Add filters: `Gender`, `Region`, `Admission Type`
4. Make filters interactive: click each filter → **Single Value (Dropdown)**

---

## Step 12: Formatting & Polish

### Consistent colour palette
| Colour | Hex | Use |
|--------|-----|-----|
| Navy | `#1F4E79` | Dashboard titles |
| Blue | `#2E75B6` | Primary bars/lines |
| Orange | `#F97316` | Targets |
| Green | `#16A34A` | Positive/met |
| Red | `#DC2626` | Alerts/missed |

### Dashboard titles
Dashboard → Objects → Text → drag to top → paste:
```
🏥 Hospital Patient Analytics | 2022–2023
```

### Background colour
Dashboard → Format → Dashboard → Shading → `#F0F4F8`

### Remove gridlines
Worksheet → Format → Lines → set all to None

---

## Step 13: Publish to Tableau Public (Free!)

1. **File → Save to Tableau Public As**
2. Sign in with your free Tableau Public account
3. Name it: `Hospital Patient Analytics 2022-2023`
4. Click **Save**
5. Your dashboard gets a public URL — **copy this link** and add it to:
   - Your GitHub README
   - Your LinkedIn profile
   - Your CV

---

## 💡 Key Insights to Call Out in Your Portfolio
- Emergency has highest wait times — 3× above target in peak months
- Oncology has lowest satisfaction (3.4/5) despite being a high-acuity unit
- 80+ age group has 2× the average length of stay
- June–August (Australian winter) sees a 28% spike in admissions
- Referral patients have significantly higher satisfaction than Emergency admissions
