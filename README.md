# Global Cancer Patients (2015–2024) — Exploratory & Statistical Analysis

An end-to-end exploratory data analysis (EDA), statistical testing, and machine learning project on a global cancer patient dataset (50,000 patients, 2015–2024). The project investigates demographic patterns, risk factors, treatment economics, and the predictors of cancer severity and survival.

---

## Dataset

**File:** `global_cancer_patients_2015_2024.csv` (50,000 rows)

| Column | Description |
|---|---|
| `Patient_ID` | Unique patient identifier |
| `Age` | Patient age |
| `Gender` | Male / Female / Other |
| `Country_Region` | One of 10 countries (Australia, USA, UK, India, Germany, Russia, Brazil, Pakistan, China, Canada) |
| `Year` | Year of record (2015–2024) |
| `Genetic_Risk`, `Air_Pollution`, `Alcohol_Use`, `Smoking`, `Obesity_Level` | Standardized risk factor scores (0–10) |
| `Cancer_Type` | Lung, Colon, Prostate, Leukemia, Liver, Skin, Cervical, Breast |
| `Cancer_Stage` | Stage 0, I, II, III, IV |
| `Treatment_Cost_USD` | Cost of treatment in USD |
| `Survival_Years` | Years survived post-diagnosis |
| `Target_Severity_Score` | Composite cancer severity score |

---

## Project Structure

```
├── project1.py                                   # Main analysis script
├── global_cancer_patients_2015_2024.csv          # Source dataset
└── Figure_1.png ... Figure_12.png                # Generated plots
```

---

## Requirements

```
numpy
pandas
seaborn
matplotlib
scipy
scikit-learn
statsmodels
```

Install with:
```bash
pip install numpy pandas seaborn matplotlib scipy scikit-learn statsmodels
```

> **Note:** Update the file path in `project1.py` (`pd.read_csv(...)`) to point to your local copy of the dataset before running.

---

## Analysis Workflow

### 1. Descriptive Analysis
- **Age** — KDE + histogram show a broad, fairly uniform spread from ~20–90 years (`Figure_1`).
- **Gender** — Near-equal split across Male (16,796), Female (16,709), and Other (16,495) (`Figure_2`).
- **Country/Region** — Roughly even distribution across all 10 countries (`Figure_3`).
- **Cancer Type** — 8 cancer types with comparable counts; Colon and Prostate are most frequent (`Figure_4`).
- **Cancer Stage** — 5 stages (0–IV) with near-equal representation, Stage II most common (`Figure_5`).
- **Treatment Cost** — Uniformly distributed between roughly $10K–$100K, no skew (`Figure_6`).

### 2. Risk Factor Analysis
Linear regression of each risk factor (Genetic Risk, Air Pollution, Alcohol Use, Smoking, Obesity Level) against `Target_Severity_Score` (`Figure_7`). All show weak-to-moderate positive relationships (R² ≈ 0.06–0.23), with Smoking and Genetic Risk showing the strongest individual trends.

### 3. Early-Stage Diagnosis Rates
Proportion of patients diagnosed at Stage 0/I, computed per cancer type. Rates range ~38–40% across cancers, with Liver cancer highest and Lung cancer lowest — highlighting a gap in early lung cancer detection.

### 4. Predictive Modeling (Random Forest)
- **Correlation analysis** (Pearson & Spearman) between risk factors and both `Target_Severity_Score` and `Survival_Years`. Smoking correlates most strongly with severity (~0.48); no risk factor meaningfully correlates with survival years.
- **Random Forest Regressor** trained to predict `Target_Severity_Score`:
  - Train R² ≈ 0.97, Test R² ≈ 0.77
  - Feature importance (`Figure_8`) ranks **Smoking** and **Genetic Risk** as top predictors, followed by Air Pollution, Alcohol Use, Treatment Cost, and Obesity Level. Age, Gender, Cancer Type/Stage, Country, and Year contribute minimally.
- A second Random Forest (with `GridSearchCV` hyperparameter tuning) was built for `Survival_Years`, but performance and correlation analysis both indicate the available features have little to no predictive power for survival duration (`Figure_9`).

### 5. Economic Burden Analysis
- **Treatment cost by country, age group, and gender** (`Figure_10`, `Figure_11`):
  - Higher-income countries (USA, Australia, China) show higher average costs.
  - Gender has minimal effect on average treatment cost.
  - Costs rise with age, especially 61+, most notably in Australia and the USA.
  - Countries with strong public healthcare systems (Canada, Germany, UK) show more stable costs across age groups.

### 6. Hypothesis Testing
- **Treatment Cost vs. Survival Years** — Pearson and Spearman correlation tests plus a regression plot (`Figure_12`) show **no significant correlation** between treatment cost and survival years.
- **Cancer Stage vs. Treatment Cost / Survival Years** — Shapiro-Wilk normality checks followed by Kruskal-Wallis tests show **no significant difference** in either cost (p ≈ 0.42) or survival years (p ≈ 0.60) across cancer stages.
- **Genetic Risk × Smoking Interaction** — An OLS regression with an interaction term tests whether genetic risk amplifies the effect of smoking on severity. The interaction term is small and non-significant (p ≈ 0.62), indicating **no evidence of interaction** between these two risk factors.

---

## Key Findings

1. The dataset is well-balanced across demographic and clinical categories, supporting robust comparative analysis.
2. **Smoking and Genetic Risk** are the strongest predictors of cancer severity; demographic variables (age, gender, country) matter far less.
3. **Treatment cost and cancer stage have no significant relationship with survival years** — survival duration in this dataset appears largely independent of the measured features.
4. **Treatment cost is driven more by geography and age than by gender or disease severity**, pointing to systemic economic factors (healthcare infrastructure, age-related care intensity) rather than clinical ones.
5. Early-stage diagnosis rates are broadly similar across cancer types, with lung cancer lagging behind — suggesting room for improved screening.

---

## Figures

| Figure | Description |
|---|---|
| 1 | Age distribution (KDE + histogram) |
| 2 | Gender count |
| 3 | Country/Region distribution |
| 4 | Cancer type count |
| 5 | Cancer stage count |
| 6 | Treatment cost distribution |
| 7 | Risk factors vs. severity score (regression) |
| 8 | Feature importance for severity score (Random Forest) |
| 9 | Survival years distribution |
| 10 | Average treatment cost by country, age group, gender |
| 11 | Average treatment cost heatmap (age group × country) |
| 12 | Treatment cost vs. survival years (regression) |

---

## Limitations & Future Work

- `Survival_Years` and `Target_Severity_Score` appear to be synthetically generated in a way that limits real-world predictive relationships (particularly for survival).
- Future work could incorporate treatment type, comorbidities, or time-series trends across `Year` to strengthen survival modeling.
- Model tuning was limited to Random Forest; gradient boosting or ensemble stacking could be explored for improved test performance.

---

## Author

**Kanishk**
B.Com | Masters in Economics
Gokhale Institute of Politics and Economics, Pune

[GitHub](https://github.com/kanishknarwani) | [LinkedIn](https://linkedin.com/in/kanishk-narwani)
