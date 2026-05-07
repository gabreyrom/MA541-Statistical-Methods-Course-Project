# Statistical Analysis of Shot Events in La Liga 2017/18

**MA 541 — Statistical Methods | Stevens Institute of Technology**
**Author:** Gabriel Reynoso

---

## Overview

This project applies inferential statistics and predictive modeling to shot event data from the La Liga 2017/18 season. Using 971 shot records from the StatsBomb open dataset, the analysis examines which factors — geometric, contextual, and tactical — most influence whether a shot results in a goal.

---

## Dataset

- **Source:** [StatsBomb Open Data](https://github.com/statsbomb/open-data)
- **Competition:** La Liga 2017/18 (36 matches)
- **Records:** 968 shot events (after cleaning)
- **Goal rate:** 11.57% (112 goals)

### Key Variables

| Variable | Type | Description |
|---|---|---|
| `xg` | Continuous | StatsBomb expected goals value |
| `distance` | Continuous | Distance from goal (yards) |
| `angle` | Continuous | Angle to goal (degrees) |
| `body_part` | Categorical | Head, Right Foot, Left Foot |
| `play_pattern` | Categorical | Regular Play, Free Kick, Corner, etc. |
| `under_pressure` | Binary | Whether defender was applying pressure |
| `first_time` | Binary | Whether shot was a first touch |
| `goal` | Binary | Outcome — target variable |

---

## Methods

### Descriptive Statistics
- Summary statistics and cross-tabulations by category
- Spatial shot map visualization on a standard pitch

### Normality Assessment
- Shapiro-Wilk tests — all variables rejected normality (p < 0.05)
- Q-Q plots for visual confirmation

### Hypothesis Testing
- **Chi-square tests:** Independence between categorical variables and goal outcome
- **One-sample t-test:** Mean xG tested against a reference of 0.10
- **One-way ANOVA:** xG differences across body part and play pattern groups
- **Tukey HSD post-hoc:** Pairwise comparisons after significant ANOVA results
- **Power analysis:** Sample size requirements for detecting differences between shot types

### Predictive Modeling
- **Logistic Regression** — binary classification (goal vs. no goal)
  - Features: `distance`, `angle`, `under_pressure`, `first_time`
  - Threshold optimized via F1-score

---

## Key Results

### Chi-Square Tests

| Association | χ² | p-value | Significant? |
|---|---|---|---|
| Body part × Goal | 2.50 | 0.287 | No |
| Play pattern × Goal | 17.32 | 0.008 | Yes |
| Under pressure × Goal | 0.15 | 0.701 | No |
| First time × Goal | 14.92 | 0.0001 | Yes |

### Logistic Regression

| Feature | Odds Ratio | p-value |
|---|---|---|
| Distance | 0.9303 | 0.008 |
| Angle | 1.0300 | 0.003 |
| Under pressure | 0.7418 | 0.327 |
| First time | 1.4612 | 0.099 |

**Model performance:** AUC-ROC = 0.757 | Accuracy = 85% | F1 (goals) = 0.39 | Pseudo R² = 0.137

### Notable Findings
- First-time shots convert at nearly double the rate of touch-up shots (17.3% vs. 8.7%)
- xG varies significantly across play patterns (ANOVA: F = 10.19, p < 0.001)
- Distance and angle are the strongest geometric predictors of goal probability

---

## Repository Structure

```
├── final_project.ipynb     # Main analysis notebook
├── sandbox.ipynb           # Data extraction and preparation
├── final_df.json           # Processed shot dataset (968 records)
├── data/
│   ├── competitions.json
│   ├── la_liga_2017_18.json
│   └── events/             # Raw match event JSON files (36 matches)
└── figures/
    ├── fig_overview_grid.png
    ├── fig_shot_map.png
    ├── fig_qqplots.png
    ├── fig_boxplots_univariate.png
    ├── fig_correlation_heatmap.png
    ├── fig_categorical_goalrates.png
    ├── fig_anova_boxplots.png
    └── fig_logistic_evaluation.png
```

---

## Requirements

- Python 3.x
- `pandas`, `numpy`
- `matplotlib`, `seaborn`
- `scipy`, `statsmodels`
- `scikit-learn`

---

## Data Source

StatsBomb provides freely available event-level football data for research and educational use.
See their [open-data repository](https://github.com/statsbomb/open-data) and [terms of use](https://github.com/statsbomb/open-data/blob/master/LICENSE.pdf).