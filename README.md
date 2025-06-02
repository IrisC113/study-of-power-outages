# Study of Power Outages

**Name:** Shujia Chen, Qi Zhang(Sort by last name)

### Overview

This project is part of DSC 80 at UC San Diego. The project investigates patterns and causes of power outages using public datasets.

---

##  Introduction

## Data Cleaning and Exploratory Data Analysis

In the data cleansing phase, we first restructured the original Excel file by skipping the first five rows of non-data content and treating row 6 as a valid table header, and removing row 0, which contained the unit description. Rows and columns with all null values were then removed to ensure data purity, and column names were formatted (e.g., removing spaces). We attempted numeric type conversion for all columns so that quantitative data such as year and tariff could be statistically analyzed, while retaining categorical variables that could not be converted. Double counting of data was avoided by removing duplicate rows. Finally, we focused on five key fields (year, climate category, climate region, length of outage, residential electricity price) to create an analyzed subset that retained some missing values (e.g., climate category in row 1530) to reflect the true data state. The cleaned dataset is ready to be used to explore the association of blackout events with climate/electricity price factors, as exemplified in the first 10 rows below:

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
