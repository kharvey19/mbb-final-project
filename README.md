# Veteran Well-Being Analysis (ICPSR 38304)

This project analyzes veteran survey data to estimate how combat exposure, age, and income relate to a composite psychological well-being score.

## Project Scope

- Data source: https://www.icpsr.umich.edu/web/ICPSR/studies/38304#
- Main workflow: `analysis.ipynb`
- Research focus: whether combat exposure predicts lower well-being after controlling for demographic and socioeconomic factors

## Well-Being Index (Outcome Variable)

The notebook builds `wellbeing_index_z` by combining positive and negative mental-health indicators:

- Positive items: `WELLNESS_1`, `WHO19`, `WELLNESS_25`
- Reverse-coded negative items: `WHO26` (or `WHO_26`), `SBQ_1`, `MISSSF_1`, `LONELINESS_1-3`
- Steps: reverse-code negatives -> z-score all items -> average row-wise (complete cases only)

Reported reliability from the analysis: Cronbach's alpha ~= 0.821.

## Key Findings

- Age is positively associated with well-being in the full sample.
- Combat exposure is negatively associated with well-being.
- Income is a significant protective factor (higher income, higher predicted well-being).
- Age x combat interaction is not statistically strong, suggesting combat shifts baseline well-being more than slope over age.
- In the under-50 subsample, the negative combat association is stronger than in the full sample.

## Tables

### Table 1. Demographic and Socioeconomic Factors

| Variable | Metric | Younger Subsample (N=530) | Full Dataset (N=1,495) |
| --- | --- | --- | --- |
| Age | Mean (SD) | 39.4 (+/- 6.2) years | 54.2 (+/- 14.8) years |
| Income | Median | ~$72,000 | ~$76,000 |
| Race: White | % of sample | 358 (68%) | 1,061 (71%) |
| Race: Black | % of sample | 75 (14%) | 212 (14%) |
| Race: Hispanic | % of sample | 55 (10%) | 119 (8%) |
| Race: Other | % of sample | 42 (8%) | 103 (7%) |
| Combat Exposure (CES) | Mean score (0-35) | 14.8 | 13.9 |

### Table 2. Well-Being Index by Sociodemographic Factors

| Group | Sub-group | Definition | Younger Subsample (N=530) | Full Dataset (N=1,495) |
| --- | --- | --- | --- | --- |
| Overall Average | Mean Z-score | Standardized | -0.12 | 0.00 |
| Race | White | Reference group | -0.10 | +0.02 |
| Race | Black |  | -0.08 | -0.04 |
| Race | Hispanic |  | -0.15 | -0.11 |
| Race | Other | Asian, PI, Multi | -0.18 | -0.14 |
| Combat Exposure | Low | CES <= 7 | +0.14 | +0.19 |
| Combat Exposure | High | CES >= 20 | -0.42 | -0.38 |
| Income | Top 25% | >$110k/year | +0.28 | +0.33 |
| Income | Bottom 25% | <$40k/year | -0.35 | -0.41 |

### Table 3. Regression Results (Predicting Well-Being)

| Predictor | Coefficient | Std Error | p-value |
| --- | ---: | ---: | ---: |
| Intercept | -0.5908 | 0.258 | 0.022 |
| AGE | 0.0157 | 0.007 | 0.019 |
| INCOME | 2.61e-07 | 1.22e-07 | 0.032 |
| CES_TOTAL | 0.1115 | 0.110 | 0.311 |
| CES x AGE (interaction) | -0.0056 | 0.003 | 0.070 |
| CES x Black | 0.0174 | 0.065 | 0.788 |
| CES x Hispanic | 0.0313 | 0.086 | 0.716 |
| CES x Other Race | -0.1294 | 0.123 | 0.293 |

## Relevant Plots

### Age Distribution

![Age Distribution](docs/plots/age_distribution_notebook.png)

### Well-Being vs Age

![Well-Being vs Age](docs/plots/wellbeing_by_age_notebook.png)

### Well-Being vs Combat Exposure

![Well-Being vs Combat Exposure](docs/plots/wellbeing_by_combat_notebook.png)

## Aside: Widowhood

![Widowed-Only Age and Well-Being](docs/plots/widowed_only_age_wellbeing.png)

![Widowhood Comparison](docs/plots/widowhood_comparison_age_wellbeing.png)

## Reproduce

1. Open `analysis.ipynb`
2. Run cells top-to-bottom
3. Confirm the regression sections:
   - Full sample model
   - Under-50 model
   - Nested model comparison (`time_since_service_years` sensitivity)

## Notes

- This repository currently includes exploratory notebook work and presentation-oriented outputs.
- The README is a concise synthesis of `analysis.ipynb` and the follow-up findings document.
