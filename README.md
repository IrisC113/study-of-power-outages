# Dark Data: Exploring Power Outage

**Name:** Shujia Chen, Qi Zhang

### Overview

This project is part of DSC 80 at UC San Diego. The project investigates patterns, price and causes of power outages using public datasets.

---

##  Introduction

This project is based on the U.S. Energy Information Administration's (EIA) publicly available outage dataset, which systematically documents large-scale power outages across the U.S. for the period **2000-2016**. The raw data contains multiple dimensions of grid operational metrics, and after rigorous cleaning, we focus on five core variables: year of event (`YEAR`), climate condition classification (`CLIMATE.CATEGORY`), geoclimatic region (`CLIMATE.REGION`), outage duration (`OUTAGE.DURATION`) , and residential electricity price (`RES.PRICE`). Together, these variables form the basis for analyzing the impact of climate and energy-economic factors on grid stability.

We are committed to answering the key question, **How do climatic conditions and electricity price factors affect the duration and frequency of outages?** This research is of great relevance: power interruptions cause economic losses in the U.S. of about 50 billion dollar per year on average (U.S. Department of Energy, 2022). For example, the Texas cold snap in 2021 left 5 million people without power and caused 19.5 dollar billion in damages, and the California precautionary outage in 2019 affected 3 million residents. By revealing patterns of climate and electricity price impacts on grid stability, this study can provide data-driven decision support for grid upgrade priority zone identification, extreme weather contingency planning, and electricity price policy reform.

The dataset contains 1,459 records of power outage events spanning 17 years. The following figure illustrates the distribution of key features:

<iframe
  src="assets/key-feature-statistics.html"
  width="900"
  height="400"
  frameborder="0"
></iframe>

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

In the data cleansing phase, We attempted numeric type conversion for all columns so that quantitative data such as year and tariff could be statistically analyzed, while retaining categorical variables that could not be converted. Double counting of data was avoided by removing duplicate rows. Finally, we focused on five key fields (year, climate category, climate region, length of outage, residential electricity price) to create an analyzed subset that retained some missing values to reflect the true data state. The cleaned dataset is ready to be used to explore the association of blackout events with climate patterns and electricity price fluctuations

<iframe
  src="assets/cleaned_outage_subset.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

### Univariate Analysis

In the univariate analysis, we focused on the distributional characteristics of the number of blackout events:

* Annual trends

A line graph shows the change in the frequency of blackout events between 2000 and 2015. The data show clear peaks in 2003, 2008 and 2011, with a maximum in 2011, indicating the presence of systemic risk factors in these years.

<iframe
  src="assets/num-outages-over-time.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>


* Climate category distribution A bar chart was used to compare the frequency of outages under different climatic conditions.” Normal” climate has the highest share of outages, while outages in "cold" and "warm" climates occur less frequently, respectively., suggesting that the vulnerability of the grid is more of a concern in regular climate conditions.

<iframe
  src="assets/outages-by-climate.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

In a bivariate analysis, we explored the association of outage duration with other variables:

* Climate regional shadows

Regional variability is revealed through a strip chart, with the Southeast having the longest median duration of outages and the West having the shortest, reflecting significant regional disparities in infrastructure resilience.

<iframe
  src="assets/outage-duration-by-region.html"
  width="1000"
  height="700"
  frameborder="0"
></iframe>

* Electricity price correlation

The scatterplotshows a weak positive correlation between electricity prices and outage length, with extreme cases of long outages occurring in areas of high electricity prices, suggesting that regulatory policy or underinvestment may be driving up both electricity prices and outage risk.

<iframe
  src="assets/outage-vs-resprice.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates
By grouping average outage lengths (in minutes) by climate region, we find significant geographic differences in grid stability:

- East North Central has the highest average outage duration, well above the national average;

- West and Southwest perform best.

- West North Central had the fastest recovery.

<iframe
  src="assets/avg-duration-by-region.html"
  width="500"
  height="400"
  frameborder="0"
></iframe>

## Assessment of Missingness

### NMAR Analysis
We focus on the missing pattern in the `OUTAGE.DURATION` column as belonging to **NMAR (Non-Missing at Random):** 

**Basis**: the length of the outage must be at the end of the event before it can be recorded. If the outage has not been restored (duration unknown), the utility cannot report the data. 
            probability of missing depends on the unobserved value itself (the longer the duration, the more likely it is to be missing due to non-ending)

**Converted to MAR Recommendation**: the following data need to be added 
  - Outage event status (resolved or not)  
  - Report submission timestamp  
  - Event start time  

This additional information allows us to interpret the missing by features such as whether the event duration exceeds the report time.

<iframe
  src="assets/missing_values.html"
  width="700" height="600"
  frameborder="0"
></iframe>

### Missingness Dependency Analysis
I will analyze the dependencies with `CLIMATE.CATEGORY` and `YEAR` for the missing case of `OUTAGE.DURATION`.

#### **Climate Category Analysis**

Research question: does the absence of `OUTAGE.DURATION` depend on climate category?

- **Null hypothesis (H₀)**: the distribution of climate categories is the same in the time-length missing and non-missing groups

- **Alternative hypothesis (H₁)**: the distribution of climate categories is different in the time-length missing and non-missing groups

<iframe
  src="assets/climate-missingness-proportion.html"
  width="1000"
  height="700"
  frameborder="0"
></iframe>

**Summarize**

There was a significant association between climate category and missing outage duration (p=0.042). The permutation test rejected the original hypothesis, indicating that the pattern of missing `OUTAGE.DURATION` was statistically different in different climatic regions. Visualization of the data reveals a clear pattern: **Warm** have the highest percentage of missing outage durations, **Cold** have the lowest percentage of missing durations, and **Normal** fall in between. This systematic variation suggests that missing data is not random and may be related to climate-related operational processes, reporting mechanisms, or infrastructure characteristics. The results of this analysis suggest the need to consider the impact of climate factors on data completeness in subsequent studies and to apply targeted treatmentsto avoid biased conclusions.

<iframe
  src="assets/year-missingness-permutation-test-blue.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### **Year Analysis**
Research question: is the absence of OUTAGE.DURATION dependent on the year?

- **Null hypothesis (H₀):** the distribution of years is the same in the missing and non-missing groups in terms of length of time.

- **Alternative hypothesis (H₁):** the distribution of years is different in the time-length missing and non-missing groups.

<iframe
  src="assets/year-missingness-proportion.html"
  width="1000"
  height="700"
  frameborder="0"
></iframe>

**Summarize**

Based on the results of the permutation test, both climate category (p=0.042) and year (p=0.026) are significantly associated with missing outage duration, indicating a systematic bias in the missing pattern: the missing rate is significantly higher in warm climate zones than in cold climate zones, and the year dimension presents a fluctuating characteristic of early (2000-2003) and recent (2014-2016) missing peaks, and a mid-term (2005- 2010) trough fluctuations. These non-random missingness patterns suggest that data correction for climatic and chronological factors is needed in the actual analysis to avoid biased conclusions.

<iframe
  src="assets/year-missingness-permutation-test-blue.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing

We tested whether there is a significant linear correlation between outage duration and residential electricity prices.

### Research Problem

**Null Hypothesis(H₀)**: Outage duration is not linearly correlated with electricity prices (ρ = 0)

**Alternative hypothesis (H₁)**: Outage duration is linearly correlated with electricity price (ρ ≠ 0)

**Test Methods**
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

- **Observed correlation coefficient**: 0.0100
- **Permutation test p-value**: 0.7025
- **Significance level**: α = 0.05

**Conclusion**
Fail to reject the null hypothesis (p-value = 0.7025 > 0.05). There is no statistically significant linear correlation between outage duration and residential electricity prices. The observed correlation coefficient (0.0100) is consistent with random chance under the null hypothesis of no correlation.

## Framing a Prediction Problem

My prediction task is to predict the severity of a major power outage, measured by the duration of the outage in minutes.

This is a regression problem because the response variable is continuous.

At the time of prediction, we would know the year, climate category, region, and residential electricity price, so we restrict our model to only use features available at that time. These features may reflect the underlying factors, like regional weather patterns.

The model is evaluated using Root Mean Squared Error (RMSE), as this metric penalizes larger prediction errors and is interpretable in the same unit as the target variable (minutes).

## Baseline Model

My model is a regression model that uses the features `CLIMATE.CATEGORY`, `YEAR`, and `RES.PRICE` to predict the duration of a major power outage in minutes. This information can help energy providers better prepare for the severity of outages and appropriate response strategies, such as  infrastructure planning.
The features are:

`CLIMATE.CATEGORY (nominal)`, `YEAR (ordinal)` and `RES.PRICE (quantitative)`. The target column OUTAGE.DURATION is continuous, and it is measured in minutes. The data is heavily skewed due to a small number of extremely long outages.

The performance of this baseline model was modest, with an RMSE of 8919.55 minutes. This high error reflects the strong right-skew in outage duration, meaning a few extremely long outages greatly influence the overall prediction error.

## Final Model

For my final model, I used a RandomForestRegressor and included the original features from the baseline model，`CLIMATE.CATEGORY`, `YEAR`, `RES.PRICE`, and also two new engineered features: `YEAR_BUCKET` and `LOG_RES.PRICE`. 

`YEAR_BUCKET` groups years into 5-year intervals to simplify time-based patterns, while `LOG_RES.PRICE` is the log-transformed version of residential electricity price, which helps reduce the impact of extreme values.

These features were designed to help the model capture patterns more effectively, especially given how skewed the outage durations are.

I applied a StandardScaler to the numerical features to stabilize model training. I considered using a QuantileTransformer to better handle outliers, but it caused instability and repetitive warnings during cross-validation. Eventually, StandardScaler was chosen because it's simple and effective, especially with tree-based models like Random Forest.

I used GridSearchCV to find the best hyperparameters and these were:
- **n_estimators:** 200
- **max_depth:** 10
- **min_samples_split:** 2

After training, the model achieved an RMSE of 8292.16 minutes, which is an improvement over the baseline model’s RMSE of 8919.55. It shows that adding relevant features and tuning the model carefully can lead to better performance, especially on data with a lot of variation like this.

## Fairness Analysis

My groups for the fairness analysis are cold vs. warm climate categories. This is defined based on the **CLIMATE.CATEGORY**, where Group X includes all observations labeled as “cold” and Group Y includes those labeled as “warm.” I chose these two groups because climate plays a big role in power outages. They could affect how accurately the model predicts outage durations. I wanted to see if my model was equally accurate across these different climate types.

My evaluation metric is Mean Absolute Error (MAE) since this is a regression model and MAE directly quantifies prediction error in minutes. I used permutation tests to compare the MAE for cold vs. warm climates under label shuffling, and then compared this distribution to my initially observed MAE difference.

- **Null Hypothesis:** The model is fair. Its MAEs for cold and warm climate regions are roughly the same.
- **Alternative Hypothesis:** The model is unfair. Its MAE for cold regions is significantly higher than that for warm regions.

I performed a permutation test with 1000 trials, using a significance level of 0.05. The observed mean absolute error difference was −50.02 minutes, and the resulting p-value was 0.5850. Because this is above the significance level, I fail to reject the null hypothesis. The model does not appear to perform significantly worse for cold climate categories.

The figure below shows the distribution of the test statistic.

<iframe
  src="assets/permutation-test-cold-warm.html"
  width="1000"
  height="700"
  frameborder="0"
></iframe>

**Test Statistic:** Observed Mean Absolute Error Difference (cold - warm): -50.02 minutes  
**Significance Level:** α = 0.05  
**p-value:** 0.5850

**Conclusion:**  

Fail to reject the null hypothesis (p-value = 0.5850 > 0.05). There is no statistically significant difference in MAE between cold and warm climate regions. The observed error difference (-50.02 minutes) is consistent with random chance under the null hypothesis of fairness.
