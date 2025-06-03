# Study of Power Outages

**Name:** Shujia Chen, Qi Zhang(Sort by last name)

### Overview

This project is part of DSC 80 at UC San Diego. The project investigates patterns, price and causes of power outages using public datasets.

---

##  Introduction

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

In the data cleansing phase, We attempted numeric type conversion for all columns so that quantitative data such as year and tariff could be statistically analyzed, while retaining categorical variables that could not be converted. Double counting of data was avoided by removing duplicate rows. Finally, we focused on five key fields (year, climate category, climate region, length of outage, residential electricity price) to create an analyzed subset that retained some missing values to reflect the true data state. The cleaned dataset is ready to be used to explore the association of blackout events with climate/electricity price factors, as exemplified in below:
<iframe
  src="assets/cleaned_table.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

### Univariate Analysis

In the univariate analysis, we focused on the distributional characteristics of the number of blackout events:

* Annual trends
A line graph shows the change in the frequency of blackout events between 2000 and 2015. The data show clear peaks in 2003, 2008 and 2011, with a maximum in 2011 (around 600 events), indicating the presence of systemic risk factors (e.g. extreme weather or aging infrastructure) in these years.
<iframe
  src="assets/num-outages-over-time.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>


* Climate category distribution
A bar chart was used to compare the frequency of outages under different climatic conditions.” Normal” climate has the highest share of outages (about 75%), while ‘cold’ and ‘warm’ climates account for 15% and 10%, respectively, suggesting that the vulnerability of the grid is more of a concern in regular climate conditions.
<iframe
  src="assets/outages-by-climate.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

In a bivariate analysis, we explored the association of outage duration with other variables:

* Climate regional shadows
Regional variability is revealed through a strip chart, with the Southeast (Southeast) having the longest median duration of outages (~8,000 minutes) and the West (West) having the shortest (~500 minutes), reflecting significant regional disparities in infrastructure resilience.
<iframe
  src="assets/outage-duration-by-region.html"
  width="1000"
  height="700"
  frameborder="0"
></iframe>

* Electricity price correlation
The scatterplotshows a weak positive correlation between electricity prices and outage length (R² ≈ 0.18), with extreme cases of long outages (>50,000 minutes) occurring in areas of high electricity prices (>25 cents/kWh), suggesting that regulatory policy or underinvestment may be driving up both electricity prices and outage risk.
<iframe
  src="assets/outage-vs-resprice.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates
By grouping average outage lengths (in minutes) by climate region, we find significant geographic differences in grid stability:

- East North Central has the highest average outage duration (5,389 minutes ≈ 90 hours), well above the national average;

- West and Southwest perform best (1,628 minutes and 1,566 minutes ≈ 27 hours);

- West North Central had the fastest recovery (only 697 minutes ≈ 11.6 hours).
<iframe
  src="assets/avg-duration-by-region.html"
  width="500"
  height="400"
  frameborder="0"
></iframe>

## Assessment of Missingness

### NMAR Analysis
We focus on the missing pattern in the **OUTAGE.DURATION** column as belonging to **NMAR (Non-Missing at Random)**:  

**Basis**: the length of the outage must be at the end of the event before it can be recorded. If the outage has not been restored (duration unknown), the utility cannot report the data
             probability of missing depends on the unobserved value itself (the longer the duration, the more likely it is to be missing due to non-ending)

**Converted to MAR Recommendation**: the following data need to be added  
  - Outage event status (resolved or not)  
  - Report submission timestamp  
  - Event start time  

This additional information allows us to interpret the missing by features such as whether the event duration exceeds the report time.

<iframe src="assets/missing_values.html" width="700" height="600" frameborder="0"></iframe>

### Missingness Dependency Analysis
I will analyze the dependencies with CLIMATE.CATEGORY and YEAR for the missing case of OUTAGE.DURATION following the sample structure you provided.

#### **Climate Category Analysis**
- Research question: does the absence of OUTAGE.DURATION depend on climate category?
-**Null hypothesis (H₀)**: the distribution of climate categories is the same in the time-length missing and non-missing groups

-**Alternative hypothesis (H₁)**: the distribution of climate categories is different in the time-length missing and non-missing groups
<iframe
  src="assets/year-missingness-proportion.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

**summarize**

The analysis shows that there is a significant difference between the missing and non-missing groups in terms of climate categories, and in particular, warm climate zones have a higher rate of missing outage durations, which supports the NMAR hypothesis. Higher temperatures lead to higher grid loads and longer outage durations, which in turn result in missing data, and thus lower outage data completeness in warmer climate zones. The results of the analysis may underestimate the average length of outages in the region, and it is recommended to prioritize the deployment of real-time monitoring systems in warm climate zones to improve data completeness.

#### **Year Analysis**
Research question: is the absence of OUTAGE.DURATION dependent on the year?

-**Null hypothesis (H₀)**: the distribution of years is the same in the missing and non-missing groups in terms of length of time

-**Alternative hypothesis (H₁)**: the distribution of years is different in the time-length missing and non-missing groups
<iframe
  src="assets/year-missingness-proportion.html"
  width="600"
  height="700"
  frameborder="0"
></iframe>

**summarize**

The analysis showed that there was no significant difference in year distribution between the missing and non-missing groups, and that missing outage length was not related to year (p > 0.05), refuting the influence of temporal factors on missing patterns. The data collection process and missing mechanism remained stable during 2000-2016, ruling out time-related explanations such as “imperfect records in the early years” or “system improvements in recent years”. As a result, no year-specific data correction was required, analyses were comparable across years, and missingness mechanisms remained consistent over the study period.

## Hypothesis Testing

We tested whether there is a significant linear correlation between outage duration and residential electricity prices

### Research Problem

**Null Hypothesis(H₀)**: Outage duration is not linearly correlated with electricity prices (ρ = 0)

**Alternative hypothesis (H₁)**: Outage duration is linearly correlated with electricity price (ρ ≠ 0)

### Test Methods

1. Verify the correlation using the **Permutation test**:
   - Calculate the observed Pearson correlation coefficient (r = 0.0100)
   - Establish the null distribution by 10,000 random permutations
   - Calculate how extreme the observations are in the null distribution

2. Rationale for selection:
   - Displacement test does not rely on normal distribution assumptions
   - More robust than traditional t-test
   - Good for medium sample size (n=1459)

<iframe
  src="assets/permutation-test-corr.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

### Results

- **Observed correlation coefficient**: 0.0100
- **Placement test p-value**: 0.7025
- **Significance level**: α = 0.05

### Conclusion 
The analysis provides no substantive evidence for a linear relationship between outage duration and electricity prices. The statistically significant group difference warrants further investigation but should not be interpreted as evidence of a direct relationship between these specific variables without additional controls.

**Final recommendation**: tariff policy development should focus on regional economic factors rather than outage duration, and it is also recommended that more data on regional characteristics be collected to explain the observed patterns of tariff differences.

## Framing a Prediction Problem

My prediction task is to predict the severity of a major power outage, measured by the duration of the outage in minutes.

This is a regression problem because the response variable is continuous.

At the time of prediction, we would know the year, climate category, region, and residential electricity price, so we restrict our model to only use features available at that time. These features may reflect the underlying factors, like regional weather patterns.

The model is evaluated using Root Mean Squared Error (RMSE), as this metric penalizes larger prediction errors and is interpretable in the same unit as the target variable (minutes).

## Baseline Model

My model is a regression model that uses the features **CLIMATE.CATEGORY**, **YEAR**, and **RES.PRICE** to predict the duration of a major power outage in minutes. This information can help energy providers better prepare for the severity of outages and appropriate response strategies, such as  infrastructure planning.
The features are:
<<<<<<< HEAD

**CLIMATE.CATEGORY (nominal)**, **YEAR (ordinal)** and **RES.PRICE (quantitative)**. The target column **OUTAGE.DURATION** is continuous, and it is measured in minutes. The data is heavily skewed due to a small number of extremely long outages.

=======
**CLIMATE.CATEGORY (nominal)**, **YEAR (ordinal)** and **RES.PRICE (quantitative)**. The target column **OUTAGE.DURATION** is continuous, and it is measured in minutes. The data is heavily skewed due to a small number of extremely long outages.
>>>>>>> dd54d7a7e52d6b632f05f51dcf77c368fa7cdfc2
The performance of this baseline model was modest, with an RMSE of 8919.55 minutes. This high error reflects the strong right-skew in outage duration, meaning a few extremely long outages greatly influence the overall prediction error.

## Final Model

**Final Model**
For my final model, I used a RandomForestRegressor and included the original features from the baseline model，**CLIMATE.CATEGORY**, **YEAR**, **RES.PRICE**, and also two new engineered features: **YEAR_BUCKET** and **LOG_RES.PRICE**. These additions were meant to help the model capture patterns more effectively, especially given how skewed the outage durations are.

**YEAR_BUCKET** groups years into 5-year intervals to simplify time-based patterns.

**LOG_RES.PRICE** is the log-transformed version of residential electricity price, which helps reduce the impact of extreme values.

I also applied a QuantileTransformer to the numerical features to handle outliers more robustly.

To adjust model better, I used GridSearchCV and tested different combinations of hyperparameters. The best ones were:
- n_estimators: 200
- max_depth: 10
- min_samples_split: 2

After training, the model achieved an RMSE of 8234.37 minutes, which is an improvement over the baseline model’s RMSE of 8919.55. It shows that adding relevant features and tuning the model carefully can lead to better performance, especially on data with a lot of variation like this.

## Fairness Analysis
