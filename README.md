# 👥 HR Employee Attrition Analysis & Dashboard

An end-to-end HR analytics project — combining exploratory data analysis, a machine learning attrition-prediction model (Python), and an interactive Power BI dashboard for HR stakeholders.



![Dashboard Preview](dashboard_preview.png)



---

## 📌 Project Overview

Employee attrition is one of the most expensive and disruptive problems an HR team can face — every departure costs recruiting time, onboarding cost, and lost institutional knowledge. This project analyzes IBM's HR Employee Attrition dataset to:

- Understand **which employees are leaving and why**
- Identify **key drivers** of attrition through exploratory analysis
- Build a **predictive model** to flag at-risk employees
- Present findings through an **interactive Power BI dashboard** for HR and management

---

## 🗂️ Repository Structure
├── HR-Employee-Attrition.csv       # Raw dataset used for analysis, modeling, and the dashboard
├── hr_attrition_dashboard.pbix     # Power BI dashboard (interactive, 3-page report)
├── dashboard_preview.png           # Static preview image of the Power BI dashboard
├── EDA.ipynb                       # Part 1: Exploratory data analysis
├── ML_Model.ipynb                  # Part 2: Attrition prediction model (Random Forest)
└── README.md


## 🧹 Dataset

**File:** `HR-Employee-Attrition.csv`
**Rows:** 1,470 employees
**Columns:** 35 (31 after dropping constant/ID columns for modeling)
**Missing values:** 0

This is the well-known **IBM HR Analytics Employee Attrition** dataset — one row per employee, covering demographics, compensation, job role, satisfaction scores, and tenure, with a binary attrition label.

### Headline numbers

| Metric | Value |
|---|---|
| Total employees | 1,470 |
| Employees who left (Attrition = Yes) | 237 |
| Employees who stayed (Attrition = No) | 1,233 |
| **Overall attrition rate** | **16.1%** |
| Avg age (stayed vs. left) | 37.6 vs. 33.6 years |
| Avg monthly income (stayed vs. left) | $6,833 vs. $4,787 |
| Avg years at company (stayed vs. left) | 7.4 vs. 5.1 years |
| Avg total working years (stayed vs. left) | 11.9 vs. 8.2 years |

### Attrition rate by segment

**By overtime status**

| OverTime | Attrition Rate |
|---|---|
| Yes | 30.5% |
| No | 10.4% |

**By business travel**

| Business Travel | Attrition Rate |
|---|---|
| Travel_Frequently | 24.9% |
| Travel_Rarely | 15.0% |
| Non-Travel | 8.0% |

**By marital status**

| Marital Status | Attrition Rate |
|---|---|
| Single | 25.5% |
| Married | 12.5% |
| Divorced | 10.1% |

**By top/bottom job roles**

| Job Role | Attrition Rate |
|---|---|
| Sales Representative | 39.8% |
| Laboratory Technician | 23.9% |
| Human Resources | 23.1% |
| Research Director | 2.5% |
| Manager | 4.9% |

Employees working overtime leave at **~3x** the rate of those who don't — the single strongest segment-level signal in the data.

### Column reference

| Category | Fields |
|---|---|
| Demographics | Age, Gender, MaritalStatus, EducationField, Education |
| Job details | Department, JobRole, JobLevel, BusinessTravel, OverTime |
| Compensation | DailyRate, HourlyRate, MonthlyIncome, MonthlyRate, PercentSalaryHike, StockOptionLevel |
| Satisfaction/engagement | EnvironmentSatisfaction, JobSatisfaction, JobInvolvement, RelationshipSatisfaction, WorkLifeBalance, PerformanceRating |
| Tenure | TotalWorkingYears, YearsAtCompany, YearsInCurrentRole, YearsSinceLastPromotion, YearsWithCurrManager, NumCompaniesWorked, TrainingTimesLastYear |
| Dropped (constant/ID, no signal) | EmployeeCount, EmployeeNumber, Over18, StandardHours |
| Target | Attrition (Yes/No) |

---

## 🔄 What I Did: Raw Data → Interactive Dashboard

**1. Started from the raw IBM HR Attrition dataset** — one row per employee, with text categories (`"Yes"/"No"`, `"Travel_Rarely"`, etc.) and a mix of numeric and categorical fields.

**2. Cleaned the data**
- Dropped four columns with zero analytical value: `EmployeeCount` and `StandardHours` (constant for every row), `Over18` (constant), and `EmployeeNumber` (a unique ID with no predictive signal) — reducing the dataset from 35 to 31 columns.
- Verified there were no missing values, so no imputation was required.

**3. Encoded categorical variables**
- Applied label encoding to every text/object column (`Attrition`, `BusinessTravel`, `Department`, `EducationField`, `Gender`, `JobRole`, `MaritalStatus`, `OverTime`) so the data could feed directly into a machine learning model.

**4. Explored the data (EDA notebook)**
- Checked shape, data types, and missing values.
- Visualized the overall attrition split, and attrition broken down by department, overtime status, age, monthly income, job satisfaction, and years since last promotion.
- Built a correlation heatmap across all numeric fields to surface which variables move together.

**5. Built a predictive model (ML notebook)**
- Split the data 80/20 into training and test sets (`random_state=42` for reproducibility).
- Trained a **Random Forest Classifier** (100 trees) to predict attrition from the remaining 30 features.
- Evaluated the model with accuracy, a confusion matrix, and a full classification report.
- Extracted feature importances to identify the top predictors of attrition.

**6. Built the interactive dashboard (Power BI)**
- Loaded the raw dataset into Power BI and built DAX measures (`Total Employees`, `Attrition Count`, `Attrition Rate`, `Avg Monthly Income`).
- Designed a **3-page report** — Overview, Attrition Analysis, and Employee Details — with KPI cards, breakdown charts, and a key-drivers table.
- Added Department and Gender slicers so HR stakeholders can filter every page live, without touching the underlying data.

---

## 🤖 Model Results (Random Forest Classifier)

| Metric | Value |
|---|---|
| **Overall accuracy** | **88.1%** |
| Precision (No / Yes) | 0.88 / 0.83 |
| Recall (No / Yes) | 1.00 / 0.13 |
| F1-score (No / Yes) | 0.94 / 0.22 |

**Top predictive features:** Monthly Income, OverTime, Age, Daily Rate, Total Working Years, Monthly Rate, Hourly Rate, Distance From Home, Years At Company, Num Companies Worked.

> ⚠️ **Note on model quality:** the dataset is imbalanced (only 16% attrition), so while overall accuracy looks strong, recall on the "Yes" (attrition) class is low (0.13) — meaning the model currently misses most employees who actually leave. This is flagged here honestly rather than glossed over; addressing class imbalance (e.g. class weighting, SMOTE, or a different threshold) is a natural next step.

---

## 📊 Power BI Dashboard
**File:** `hr_attrition_dashboard.pbix`

A 3-page interactive report built on the cleaned dataset.

**Page 1 — Overview**
- KPI cards: Attrition Count, Total Employees, Avg Monthly Income, Attrition Rate %
- Attrition by Department (bar chart)
- Attrition by Gender (donut chart)
- Department and Gender slicers

**Page 2 — Attrition Analysis**
- Same KPI cards
- Attrition by Overtime Status (donut chart)
- Attrition by Job Role (column chart)
- Attrition by Job Satisfaction Level (bar chart)

**Page 3 — Employee Details**
- Same KPI cards
- Key Drivers of Employee Attrition by Department (table)
- Attrition by WorkLifeBalance (bar chart)
- Attrition by YearsSinceLastPromotion (column chart)

### Preview


![Dashboard Preview](dashboard_preview.png)


## 🛠️ Tools & Technologies

- **Python** — Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn
- **Jupyter Notebook** — EDA and model development
- **Power BI** — Interactive dashboard and DAX measures
- **Git/GitHub** — Version control and project hosting

---

📌 Key Insights
Overtime is the single strongest attrition signal: employees working overtime leave at 30.5% versus 10.4% for those who don't — nearly 3x.
Single employees churn the most (25.5%) compared to married (12.5%) or divorced (10.1%) employees.
Sales Representatives have by far the highest attrition of any role (39.8%), while Research Directors and Managers are the most stable (under 5%).
Employees who leave are younger, less tenured, and lower paid on average — 33.6 years old with 5.1 years at the company and $4,787 avg monthly income, versus 37.6 years, 7.4 years tenure, and $6,833 avg income for those who stay.
A Random Forest model predicts attrition with 88.1% accuracy, but struggles to catch actual leavers (13% recall on the attrition class) — a reminder that headline accuracy can hide a real business problem on an imbalanced dataset.

👤 Author
Shreya Nakhale
