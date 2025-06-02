# Study of Power Outages

**Name:** Shujia Chen, Qi Zhang(Sort by last name)

### Overview

This project is part of DSC 80 at UC San Diego. The project investigates patterns and causes of power outages using public datasets.

---

##  Introduction

## Data Cleaning and Exploratory Data Analysis

In the data cleansing phase, We attempted numeric type conversion for all columns so that quantitative data such as year and tariff could be statistically analyzed, while retaining categorical variables that could not be converted. Double counting of data was avoided by removing duplicate rows. Finally, we focused on five key fields (year, climate category, climate region, length of outage, residential electricity price) to create an analyzed subset that retained some missing values to reflect the true data state. The cleaned dataset is ready to be used to explore the association of blackout events with climate/electricity price factors, as exemplified in below:
<iframe
  src="assets/cleaned_table.html"
  width="700"
  height="500"
  frameborder="0"
></iframe>


## Assessment of Missingness

### NMAR Analysis.
We analyzed the dataset for missing patterns, focusing on the missing OUTAGE.DURATION column. Based on the data collection process, we believe that the missing column is likely NMAR (non-random missing):

Rationale for reasoning: power outage reports typically require the event to be completely over before the duration can be recorded. When an outage event is still in progress (power has not yet been restored), the utility is unable to report the exact duration, resulting in missing data. This missingness is directly related to the characteristics of the outage event itself (i.e., unobserved duration values).

NMAR JUDGEMENT: this missing mechanism meets the NMAR definition because the probability of missing depends on the unobserved value itself (prolonged outages are more likely to be missing).

Recommendation for conversion to MAR: additional data collection is required to make it MAR (missing at random):

The “status” of the outage event (whether it was resolved or not)

Report submission timestamp

Event start time

This additional information allows us to interpret the missing by features such as whether the event duration exceeds the report time.

<iframe src="assets/missing_values.html" width="700" height="600" frameborder="0"></iframe>

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
