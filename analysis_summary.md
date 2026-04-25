# Analysis Summary: Racial Disparities in Mental Health Among Military Personnel and Veterans

## Overview

This analysis examines racial and ethnic disparities in mental health outcomes among a sample of 1,495 U.S. military personnel and veterans. The central question is whether observed outcome disparities across racial groups reflect differences in *exposure* to stressors (combat, daily stress) or differences in *vulnerability* — that is, how strongly individuals react to a given level of stress. The analysis follows a sequential pathway: **combat exposure → daily stress burden → mental health outcomes**, testing whether this chain accounts for racial gaps.

---

## Data and Measures

**Sample:** N = 1,495 service members/veterans (survey collected beginning May 2020; cross-sectional design). Analytic samples vary slightly by outcome due to missing data (range: 1,457–1,495). Stress-index models use n = 882 (respondents who rated at least one stressor as stressful).

**Racial/ethnic groups:**
- White (n ≈ 1,109)
- Black or African American (n ≈ 211)
- Hispanic or Latino (n ≈ 84)
- Asian (n ≈ 42)
- American Indian or Alaska Native (n ≈ 13)
- Native Hawaiian or Other Pacific Islander (n ≈ 10)

### Key Variables

**Combat Exposure** — Combat Exposure Scale (CES), 7 items capturing frequency of:
- Dangerous patrols, coming under enemy fire, being surrounded, witnessing casualties, firing on the enemy, seeing others wounded, and perceived danger of injury/death (each rated 1–5).
- A single composite index was created by z-scoring and averaging all 7 items (higher = more exposure).

**Daily Stress Burden** — Participants indicated which of 7 common stressors they experienced (arguments, avoided conflict, work stress, home stress, stress affecting close others, health concerns) and rated each endorsed stressor on a 1–4 stressfulness scale. Stress burden was computed as:

> **Stress Burden = (number of stressors endorsed) × (mean stressfulness rating)**

A stress *index* (mean stressfulness among those endorsing ≥1 stressor) was also used in interaction models.

**Outcome Variables:**
| Variable | Scale | Direction |
|---|---|---|
| SBQ Total (suicide risk) | Suicide Behaviors Questionnaire, continuous | Higher = more risk |
| SBQ Lifetime flag | Binary (at risk vs. not) | — |
| MISS-SF Total (PTSD symptoms) | Mississippi Scale, continuous | Higher = more symptoms |
| WHOQOL PSY | Psychological quality of life | Higher = better |
| WHOQOL PH | Physical/functional quality of life | Higher = better |
| WHOQOL SOCREL | Social relationships quality of life | Higher = better |
| Help-seeking mean | Continuous | Higher = more help-seeking |
| Purpose/meaning mean | Continuous | Higher = greater sense of purpose |

---

## Methods

All analyses were conducted in Python using `pandas`, `statsmodels`, `scipy`, `seaborn`, and `matplotlib`.

### 1. Descriptive Analysis
Distributions of combat exposure, stress burden, and all outcome variables were examined by race/ethnicity using stacked bar charts, box plots, and group means.

### 2. One-Way ANOVA
Used to test whether each outcome variable differs significantly across racial groups (model: `Y ~ race`). Reports F-statistic, p-value, and R².

### 3. OLS Linear Regression — Sequential Models
Three model types were estimated for each outcome:

1. **Unadjusted race model:** `Y ~ race` — establishes baseline racial differences
2. **Stress-adjusted model:** `Y ~ race + stress_burden` — tests how much racial differences shrink after accounting for stress
3. **Full chain model:** `Y ~ race + combat_exposure + stress_burden` — tests whether combat exposure adds further explanatory power beyond stress

The *reduction in race coefficients* from unadjusted to adjusted models serves as an indicator of mediation (i.e., how much of the racial gap is explained by the added variable).

### 4. Interaction Models
To test whether stress *affects* outcomes differently by race (i.e., differential vulnerability), interaction models were estimated:

> `SBQ ~ stress_index * race`

Race-specific slopes were extracted and a **likelihood ratio test** compared the interaction model to the main-effects-only model, with the null hypothesis that all race × stress interaction terms are zero (χ²(5)).

### 5. Type II ANOVA
Applied to full regression models to assess the significance of each predictor (race, combat, burden) while accounting for the others.

---

## Results

### Racial Differences in Combat Exposure

Non-White groups reported higher average combat exposure than White respondents. The gap was largest for American Indian/Alaska Native respondents.

| Group | Mean Combat Index | SD |
|---|---|---|
| White | -0.069 | 0.776 |
| Black or African American | 0.134 | 0.907 |
| Hispanic or Latino | 0.277 | 0.875 |
| Asian | 0.278 | 0.922 |
| American Indian or Alaska Native | 0.799 | 1.086 |
| Native Hawaiian/Pacific Islander | 0.244 | 0.790 |

**ANOVA:** F(5, 1463) = 8.46, p < 0.001, R² = 0.028

American Indian/Alaska Native respondents report nearly one full standard deviation above the sample mean — a substantial difference.

### Racial Differences in Stress Burden

Non-White groups also reported higher stress burden, with Hispanic or Latino respondents showing the highest average.

| Group | Mean Stress Burden | SD |
|---|---|---|
| White | 3.57 | 4.97 |
| Black or African American | 4.82 | 5.83 |
| Hispanic or Latino | 6.51 | 6.37 |
| Asian | 5.05 | 6.68 |
| American Indian or Alaska Native | 4.69 | 6.06 |
| Native Hawaiian/Pacific Islander | 4.60 | 5.85 |

**ANOVA:** F(5, 1463) = 6.77, p < 0.001, R² = 0.023

### Combat Exposure Predicts Stress Burden

Higher combat exposure is associated with greater daily stress burden:

- **b = 2.20**, SE = 0.159, t = 13.82, p < 0.001, 95% CI [1.887, 2.511]
- **R² = 0.115**

This establishes that combat exposure is upstream in the stress-to-outcome chain.

### Stress Burden Predicts Suicide Risk

Stress burden is a strong predictor of SBQ total score:

- **b = 0.271**, SE = 0.017, t = 16.09, p < 0.001, 95% CI [0.238, 0.304]
- **R² = 0.150**

### Does Stress Affect Outcomes Equally Across Racial Groups?

The interaction model (`SBQ ~ stress_index * race`) yielded a **non-significant** likelihood ratio test: **χ²(5) = 6.04, p = 0.303**. This means there is no statistically significant evidence that stress translates into worse outcomes more strongly for any particular racial group. Race-specific slopes were:

| Group | Stress → SBQ Slope | 95% CI |
|---|---|---|
| White | 1.645 | [1.303, 1.988] |
| Black or African American | 1.325 | [0.636, 2.014] |
| Hispanic or Latino | 1.511 | [0.450, 2.573] |
| Asian | -0.090 | [-2.397, 2.217] |
| American Indian or Alaska Native | 7.083 | [0.412, 13.754] |
| Native Hawaiian/Pacific Islander | -0.495 | [-5.417, 4.426] |

Slopes for smaller groups (Asian, American Indian/Alaska Native, Native Hawaiian/Pacific Islander) have very wide confidence intervals due to small sample sizes — making them unreliable. For the two largest non-White groups (Black and Hispanic respondents), slopes are similar to and statistically indistinguishable from the White slope.

### Does Stress Explain Racial Gaps? Mediation Results

**Suicide Risk (SBQ Total):**

| Model | R² | F(race) | p(race) |
|---|---|---|---|
| Race only | 0.016 | 4.69 | 0.0003 |
| Race + stress burden | 0.158 | 2.94 | 0.012 |

Race coefficient reductions after controlling for stress burden (movement toward zero = partial mediation):

| Group | Unadjusted coeff | Adjusted coeff | Reduction |
|---|---|---|---|
| American Indian/Alaska Native | 2.68 | 2.38 | -0.30 |
| Asian | 1.12 | 0.73 | -0.40 |
| Black or African American | 0.49 | 0.15 | -0.34 |
| Hispanic or Latino | 1.03 | 0.24 | -0.79 |
| Native Hawaiian/Pacific Islander | 3.11 | 2.83 | -0.28 |

The Hispanic/Latino gap in suicide risk is reduced by roughly 76% after accounting for stress burden, and the Black/African American gap falls from 0.49 to 0.15 (a ~69% reduction), becoming non-significant.

**PTSD Symptoms (MISS-SF):**

| Model | R² |
|---|---|
| Race only | 0.031 |
| Burden only | 0.226 |
| Race + burden | 0.239 |

Stress burden alone explains more than 7× the variance in PTSD symptoms compared to race. Burden coefficient: **b = 1.37**, SE = 0.066, p < 0.001. After controlling for burden, the race coefficients are substantially reduced for all groups (reductions ranging from 1.4 to 3.8 points on the MISS-SF scale).

**Quality of Life Outcomes:**

Stress burden is a significant predictor across all QoL domains, but racial differences in QoL were generally smaller and often not significant:

| Outcome | Burden b | R²(burden only) | Race unadj. p | Race adj. p |
|---|---|---|---|---|
| Psychological QoL | -1.34 | 0.121 | 0.218 (ns) | 0.396 (ns) |
| Physical QoL | -1.34 | 0.128 | 0.039 | 0.275 (ns) |
| Social QoL | -0.94 | 0.043 | 0.972 (ns) | 0.777 (ns) |

Where racial differences in QoL existed (physical QoL), they were no longer significant after controlling for stress burden.

### Full Sequential Chain: Combat → Stress → Suicide Risk

The complete pathway model (`SBQ ~ race + combat_exposure + stress_burden`) shows both combat and stress are independently significant:

| Predictor | b | SE | t | p |
|---|---|---|---|---|
| Combat exposure | 0.478 | 0.116 | 4.12 | < 0.001 |
| Stress burden | 0.243 | 0.018 | 13.61 | < 0.001 |

**Model R² = 0.168** (compared to 0.158 for stress burden + race alone). Adding combat exposure further reduced race coefficients, particularly for American Indian/Alaska Native respondents (additional reduction of 0.39 points).

Type II ANOVA: F(race) = 2.32, p = 0.042; F(combat) = 17.00, p < 0.001; F(burden) = 185.30, p < 0.001.

---

## Interpretation

### What the results mean

**1. Racial disparities in mental health are real but largely exposure-driven.**
Non-White veterans in this sample report higher rates of PTSD symptoms and suicide risk. However, these gaps are substantially explained — and in several cases eliminated — once differences in stress burden are taken into account. The data do not support the conclusion that non-White veterans are *more sensitive* to stress; rather, they appear to be experiencing *more* stress.

**2. Non-White veterans face higher combat exposure.**
All non-White groups report higher average combat exposure than White respondents. American Indian/Alaska Native veterans show the most pronounced difference (nearly one full standard deviation above the sample mean). This unequal distribution of combat exposure is a structural upstream factor.

**3. Stress is the central explanatory mechanism.**
Stress burden — the combined weight of how many daily stressors one faces and how stressful they feel — is consistently the strongest predictor across all outcomes: suicide risk, PTSD, psychological, physical, and social quality of life. It accounts for far more variance than race alone in every model.

**4. The stress-to-outcome relationship does not significantly differ by race.**
The interaction test (χ²(5) = 6.04, p = 0.303) suggests that a given level of stress burden predicts roughly equivalent increases in suicide risk regardless of race. This finding supports an exposure-based (rather than vulnerability-based) explanation for disparities.

**5. Help-seeking and sense of purpose do not appear to drive disparities.**
These constructs were relatively consistent across racial groups, suggesting the observed mental health gaps are not attributable to differences in help-seeking behavior or meaning in life.

### Implications

The findings point toward **structural and systemic factors** — particularly differential exposure to combat and accumulation of daily stressors — as the primary drivers of racial disparities in veteran mental health. This has important implications for intervention:

- Reducing disparities may require addressing *upstream exposure inequalities* (e.g., differential assignment to high-risk roles) rather than focusing solely on downstream psychological treatment.
- Stress reduction and stress management interventions could be particularly beneficial for groups carrying higher average stress burdens.
- Universal screening and early intervention for high-stress burden may help close outcome gaps across racial groups, given the consistent slope of stress-to-outcome associations.

### Limitations to Keep in Mind

- **Small cell sizes** for American Indian/Alaska Native (n=13), Asian (n=42), and Native Hawaiian/Pacific Islander (n=10) groups make race-specific estimates unreliable and limit statistical power for these comparisons.
- The **cross-sectional design** means causal ordering (combat → stress → outcomes) is inferred, not proven.
- Mediation analyses here are sequential regression comparisons, not formal causal mediation with bootstrapped indirect effects, so the language of "explaining" racial gaps is descriptive rather than strictly causal.
- The sample may not be representative of all military personnel or veterans.
