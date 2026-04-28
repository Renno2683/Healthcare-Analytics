# 🏥 Healthcare Patient Analytics Dashboard

> End-to-end BI project built in both **Power BI** and **Tableau** — covering patient admissions, department performance, and satisfaction analysis across a synthetic Australian hospital dataset.

---

## 📌 Project Overview

This project answers real operational questions a **hospital data analyst** would face:

- Which departments are exceeding wait time targets?
- How do patient outcomes and satisfaction vary across departments?
- What seasonal patterns exist in admissions?
- Which patient segments have the highest readmission risk?

---

## 🗂️ Repository Structure

```
healthcare-analytics/
│
├── README.md
│
├── data/
│   ├── healthcare_dataset.xlsx   ← Main dataset (import into Power BI or Tableau)
│   ├── admissions.csv            ← 5,000 patient admission records
│   ├── departments.csv           ← Department capacity & targets
│   └── monthly_kpis.csv          ← Pre-aggregated monthly KPIs
│
├── guides/
│   ├── PowerBI_Guide.md          ← Step-by-step Power BI dashboard build
│   └── Tableau_Guide.md          ← Step-by-step Tableau dashboard build
│
└── images/                       ← Add your dashboard screenshots here
```

---

## 📊 Dataset Overview

| Table | Rows | Description |
|-------|------|-------------|
| `admissions` | 5,000 | Patient-level records with outcomes, wait times, satisfaction |
| `departments` | 7 | Department capacity, staffing, and KPI targets |
| `monthly_kpis` | 24 | Pre-aggregated monthly performance metrics |

**Date range:** January 2022 – December 2023  
**Departments:** Emergency, Cardiology, Orthopaedics, Oncology, Neurology, Paediatrics, General Medicine  
**Synthetic data** — no real patient information used

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **Power BI Desktop** | Interactive dashboard (3 pages) |
| **Tableau Public** | Published interactive dashboard (3 views) |
| **Python** | Dataset generation (pandas, numpy, openpyxl) |
| **Excel** | Data source with 4 structured sheets |

---

## 🚀 How to Use

### Power BI
1. Download `data/healthcare_dataset.xlsx`
2. Open Power BI Desktop → Get Data → Excel
3. Follow `guides/PowerBI_Guide.md` step by step

### Tableau
1. Download `data/healthcare_dataset.xlsx`
2. Open Tableau Public (free) → Connect → Excel
3. Follow `guides/Tableau_Guide.md` step by step
4. Publish your finished dashboard to Tableau Public for a live portfolio link

---

## 📊 Key Findings

| Metric | Finding |
|--------|---------|
| 🚨 Wait times | Emergency dept averages 45 mins — 50% above the 30-min target |
| 😟 Satisfaction | Oncology has the lowest score (3.4/5) despite high clinical acuity |
| 📈 Seasonality | Australian winter (June–August) drives a 28% spike in admissions |
| 🔁 Readmissions | 12% 30-day readmission rate — above the 10% benchmark |
| 👴 Age insight | Patients aged 80+ have 2× the average length of stay |
| 🌟 Best channel | Referral patients show consistently higher satisfaction scores |

---

## 🔗 Live Dashboard Links

| Tool | Link |
|------|------|
| Tableau Public | https://prod-apsoutheast-a.online.tableau.com/t/ranimchaar-fab6b5205e/views/HospitalPatientAnalytics2022-2023_17773458520660/MonthlyAdmissions |
| Power BI | *(Add your published link here if using Power BI Service)* |

---

## 👤 Author

**Ranim Chaar** - Aspiring Data Analyst transitioning from Finance & Administration.

🔗 [LinkedIn](https://linkedin.com/in/ranim-c-07571836a) · 🐙 [GitHub](https://github.com/Renno2683)

---

## 📄 License

MIT — free to fork, adapt, and use as a portfolio template.
