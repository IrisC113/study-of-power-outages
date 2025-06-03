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

### NMAR Analysis.
We analyzed the dataset for missing patterns, focusing on the missing OUTAGE.DURATION column. Based on the data collection process, we believe that the missing column is likely NMAR (non-random missing):

**Rationale for reasoning**: power outage reports typically require the event to be completely over before the duration can be recorded. When an outage event is still in progress (power has not yet been restored), the utility is unable to report the exact duration, resulting in missing data. This missingness is directly related to the characteristics of the outage event itself (i.e., unobserved duration values).

**NMAR JUDGEMENT**: this missing mechanism meets the NMAR definition because the probability of missing depends on the unobserved value itself (prolonged outages are more likely to be missing).

Recommendation for conversion to MAR: additional data collection is required to make it MAR (missing at random):

- The “status” of the outage event (whether it was resolved or not)

- Report submission timestamp

- Event start time

This additional information allows us to interpret the missing by features such as whether the event duration exceeds the report time.

<iframe src="assets/missing_values.html" width="700" height="600" frameborder="0"></iframe>

### Missingness Analysis
We analyzed the relationship between missing OUTAGE.DURATION (outage duration) and the other columns:

**Dependencies**: chi-square test showed that missing outage duration was significantly associated with climate category (χ²=15.32, p=0.004):

- Higher rate of missingness in warm climate regions (45% of all missingness)

- Normal climate regions had the lowest rate of missingness

- Climate category distribution

**Independent relationship**: t-test showed no significant relationship between missing outage duration and year (t=1.08, p=0.28):

- Missing events were evenly distributed across years

- No year-specific missing patterns

- Year distribution

**CONCLUSION**: Missing outage lengths depend on climate category (warmer regions are more likely to be missing), but are not related to year. This supports the NMAR hypothesis as warmer regions are more likely to experience prolonged outages (e.g., overloading of the grid due to heat waves) and these events are more likely to be unterminated.

<iframe
  src="assets/missingness-vs-climate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/missingness-vs-year.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>


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



## Final Model



## Fairness Analysis
