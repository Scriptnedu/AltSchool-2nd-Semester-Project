# AltSchool-2nd-Semester-Project
Python-based salary analysis of 250K global tech roles. Covers data wrangling, missing data imputation, outlier removal, EDA, NumPy simulations, and hypothesis testing to decode what drives compensation in Data and AI careers.

# The Tech Talent Premium
### *Decoding What Drives Salaries Across Global Tech Roles*

**Can we prove statistically that experience beats credentials in the global tech salary race?**
*This project says yes, and shows you exactly where the money goes.*

##  Table of Contents
- Business Problem & Goal
- Libraries Used
- Dataset Description
- Project Pipeline
- Key Insights & Findings
- Future Work

##  Business Problem & Goal

A **Global Data Science & AI talent company** wants to make smarter decisions about how it benchmarks, recruits, and advises tech professionals worldwide. To do that, it needs answers to three core questions:

- **Which roles and industries pay the most** — and by how much?
- **How do education, skills, certifications, remote work, and company size** relate to compensation?
- **Does more experience always mean more money** — or is that just conventional wisdom?

This project digs into a 250,000-record salary dataset to answer all three, using rigorous EDA, statistical imputation, and inferential testing to separate signal from noise.


### Libraries Used

| Library | Purpose |
| `pandas` | Data loading, wrangling, subsetting, feature engineering |
| `numpy` | Numerical computations, simulations, array operations |
| `matplotlib.pyplot` | Base plotting layer |
| `seaborn` | Statistical visualisations (boxplots, violin, heatmap, KDE) |
| `scipy` / `scipy.stats` | Z-score outlier detection; one-sample t-test |


## Dataset Description

| Attribute | Detail |
|---|---|
| **Source** | Google BigQuery |
| **Size** | 250,000 rows × 10 columns |
| **Target Variable** | salary (USD) |
| **Salary Range** | $31,867 – $333,046 |
| **Mean Salary** | ~$145,718 |
| **Std Deviation** | ~$37,408 |

### Feature Overview

| Column | Type | Description |
|---|---|---|
| job_title | Categorical | 12 unique tech roles |
| years_of_experience | Numerical | 0–20 years |
| education_level | Categorical | High School → PhD |
| skills_count | Numerical | 1–19 skills |
| industry | Categorical | Sector of employment |
| company_size | Categorical | Startup → Enterprise |
| location | Categorical | Country of employment |
| remote_work | Categorical | On-site / Hybrid / Full Remote |
| certifications | Numerical | 0–5 certificates |
| salary | Numerical | Annual compensation (USD) |

**Note:** The dataset was clean on load (zero nulls). Missingness was simulated to demonstrate real-world imputation workflows.


## Project Pipeline / Key Steps

### 1. Load & Inspect
- Loaded 250,000 rows from BigQuery CSV into pandas
- Renamed `experience_years` → `years_of_experience` for clarity
- Confirmed 0 missing values; validated categorical labels for mislabelling
- Key observation: right-skewed salary distribution (mean > median), suggesting high earners at the top


### 2. Data Wrangling & Feature Engineering

**Subsetting:** Narrowed focus to 4 Data/AI roles — *Data Analyst, Data Scientist, Machine Learning Engineer, AI Engineer*

**Three engineered features:**

| Feature | Logic | Purpose |
|---|---|---|
| experience_level | Binned into Entry / Mid / Senior / Expert | Enables group-based salary comparisons |
| skill_to_cert_ratio | skills_count / (certifications + 1) | Distinguishes generalists from certified specialists |
| salary_tier | Quantile-based: Low / Medium / High / Premium | Surfaces premium earner characteristics |


### 3. Handling Missing Data

Since the dataset had no real nulls, missingness was **deliberately simulated** to demonstrate production-ready imputation:

| Column | % Missing Simulated | Imputation Strategy | Reason |
|---|---|---|---|
| years_of_experience | 5% | **Median** | Robust to outliers |
| certifications | 4% | **Median** | Robust to outliers |
| education_level | 10% | **Mode** | Only valid central tendency for categoricals |
| industry | 7% | **Mode** | Only valid central tendency for categoricals |

Before/after distributions were visualised to confirm imputation preserved the original data shape.


### 4.  Outlier Detection & Removal

- **Detection:** Z-score method (|z| > 3) applied to the numerical columns of the working subset
- **Removal:** Trimming method — rows with extreme Z-scores were dropped, not capped
- Retained a clean, analysis-ready working dataset (jobs_trimmed_new) for all downstream steps


### 5. Exploratory Data Analysis (EDA)

A full investigation of salary relationships across all features:

- **Correlation heatmap:** years_of_experience shows the strongest positive numerical correlation with salary
- **Box plots:** Job title salary ranges; salary by experience level (steady upward trend from Entry to Expert)
- **Violin plot:** Salary vs education level; PhD earns slightly more but with higher variance
- **Horizontal bar chart:** Geographical salary distribution; US, Canada, UK, Germany clearly dominate
- **Side-by-side box plots:** Work mode vs company size; company size shows a clear upward trend, work mode differences are marginal
- **Grouped bar chart:** Industry median vs mean salary; Technology sector leads at ~$150K
- **Premium tier bar chart:** Higher education increases Premium tier reach, but not dramatically


### 6. NumPy Computations & Simulation

- Computed per-role mean, std deviation, min, and max salaries using numpy arrays
- Simulated **1,000,000 hypothetical salary draws** for the Data Scientist role using observed μ and σ, assuming a normal distribution
- Key findings from simulation:
  - **~5.40%** of simulated Data Scientist salaries exceed $200,000
  - Simulated distribution closely mirrors the observed data, validating the normality assumption


### 7. Inferential Statistics: One-Sample t-Test

**Hypothesis tested:**
*Do data professionals with above-average experience earn a mean salary that is significantly higher than the overall dataset mean?*

| | |
|---|---|
| **H₀ (Null)** | Senior group mean salary = Overall mean salary (experience makes no difference) |
| **H₁ (Alternative)** | Senior group mean salary > Overall mean salary |
| **Test** | One-sample t-test (one-tailed) |
| **Significance Level** | α = 0.05 |

**Results:**

| Metric | Value |
|---|---|
| Overall mean salary | ~$145,718 |
| Senior group mean salary | Significantly higher |
| t-statistic | **69.5737** |
| p-value (one-tailed) | **≈ 0.000000** |
| Decision | **REJECT H₀** |

**Interpretation:** There is overwhelming statistical evidence that senior data professionals (above-average experience) earn significantly more than the overall mean. Experience is not just correlated with salary, it has a **statistically significant causal direction**.


##  Key Insights & Findings

| # | Finding | Evidence |
|---|---|---|
| 1 | **Experience is the #1 salary driver** | Strongest positive correlation in heatmap + t-test rejection of H₀ |
| 2 | **Location matters, but only at the extremes** | US, Canada, UK, Germany stand apart; the middle cluster (Sweden, Netherlands, Singapore, etc.) converges at ~$143K–$145K |
| 3 | **Senior professionals earn significantly more** | H₀ rejected at α = 0.05; t = 69.57, p ≈ 0 |
| 4 | **AI & ML roles command higher pay than Data careers** | AI Engineer and ML Engineer lead median salary; Data Analyst is lowest |
| 5 | **Company size > work mode as a salary predictor** | Clear upward trend from Startup → Enterprise; remote/hybrid/on-site differences are marginal |
| 6 | **Skills and certifications alone don't drive salary** | Weak positive correlations with salary relative to years_of_experience |


## Future Work

- **Predictive Modelling:** Train regression models (Linear Regression, Random Forest, XGBoost) to build a salary predictor on this dataset
- **Role Expansion:** Extend the analysis beyond the 4 target roles to cover all 12 job titles in the dataset
- **Interaction Effects:** Investigate combined effects (e.g., experience × location, company size × role) to uncover compound salary drivers.
- **Time-Series Layer:** Source a versioned dataset to track how salary trends shift year over year across roles and geographies

