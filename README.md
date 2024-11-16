<h2 align="center">Reproducing Research: Causal Impact of Segregation on Poverty Rates</h2>  

This project replicates the analysis from Elizabeth Ananat’s paper, *The Wrong Side(s) of the Tracks: The Causal Effects of Racial Segregation on Urban Poverty and Inequality* (American Economic Journal: Applied Economics, 2011). The study investigates how racial segregation influences urban poverty and inequality, using railroad track layouts as an instrumental variable (IV) to estimate the causal impact of segregation on poverty rates.

[![View Original Research Paper](https://img.shields.io/badge/View%20Original%20Research%20Paper-0056A0?style=flat&logo=external-link&logoColor=white&color=0056A0)](https://www.aeaweb.org/articles?id=10.1257/app.3.2.34)

## Table of Contents

1. [Key Variables](#key-variables)
2. [Data Overview](#data-overview)
3. [Summary Statistics](#summary-statistics)
4. [OLS Regression of Poverty Rates on Segregation](#ols-regression-of-poverty-rates-on-segregation)
5. [Addressing Causality: Instrumental Variables Approach](#addressing-causality-instrumental-variables-approach)
6. [Assessing Instrument Strength](#assessing-instrument-strength)
7. [Reduced Form](#reduced-form)
8. [IV Estimation: Second Stage](#iv-estimation-second-stage)
9. [Testing Robustness](#testing-robustness)
10. [Conclusion](#conclusion)

## Key Variables

- **dism1990**: 1990 dissimilarity index.
- **herf**: Railroad Division Index (RDI).
- **lenper**: Track length per square kilometer.
- **povrate_w**: White poverty rate in 1990.
- **povrate_b**: Black poverty rate in 1990.
- **area1910**: Physical area of the city in 1910 (1000 sq. miles).
- **count1910**: Population in 1910 (1000s).
- **black1910**: Percent Black in 1910.
- **passpc**: Streetcars per capita in 1915.
- **incseg**: Income segregation in 1990.
- **pctbk1990**: Percent Black in 1990.

## Data Overview

Each observation represents a city. The dataset spans multiple years, capturing variables related to racial composition, economic outcomes, and urban infrastructure. We focus on the relationships between segregation (measured by the dissimilarity index) and poverty rates among Black and White populations.

## Summary Statistics

We begin by examining summary statistics for key variables: `dism1990`, `herf`, `lenper`, and poverty rates (`povrate_w`, `povrate_b`). These statistics provide an overview of the distribution and variability of these variables across cities.

## OLS Regression of Poverty Rates on Segregation

A simple OLS regression is run to explore the relationship between racial segregation (dissimilarity index, `dism1990`) and poverty rates for both White and Black populations.

**Regression Model:**

$$
\text{Poverty Rate (White/Black)} = \beta_0 + \beta_1 \times \text{Segregation} + \epsilon
$$

This model estimates how changes in segregation are associated with changes in poverty rates. However, this approach does not account for potential confounders that could bias the results.

## Addressing Causality: Instrumental Variables Approach

Given that the OLS model may suffer from endogeneity (i.e., omitted variable bias), we apply an instrumental variables (IV) approach to estimate the causal effect of segregation on poverty. The instrument used is the railroad division index (`herf`) and track length per square kilometer (`lenper`), which are correlated with segregation but assumed not to directly affect poverty rates.

**First Stage Regression:**

$$
\text{Segregation (1990)} = \beta_0 + \beta_1 \times \text{RDI} + \beta_2 \times \text{Track Length} + \epsilon
$$

In the first stage, we regress segregation on the instrument(s) to check whether the instruments are significantly correlated with segregation.

## Assessing Instrument Strength

To assess the strength of the instrument, we examine the F-statistic from the first stage regression. A value greater than 10 indicates a sufficiently strong instrument. If the instrument is weak, IV estimates will be unreliable.

## Reduced Form

Next, we estimate the reduced form equation, regressing poverty rates directly on the instrument (RDI and track length), without first modeling segregation. This step helps to understand the relationship between the instrument and the outcomes.

## IV Estimation: Second Stage

In the second stage, we regress poverty rates on the predicted values of segregation from the first stage. This provides an IV estimate of the causal effect of segregation on poverty rates.

**Second Stage Model:**

$$
\text{Poverty Rate (White/Black)} = \alpha_0 + \alpha_1 \times \hat{\text{Segregation}} + \epsilon
$$

The coefficient on the predicted segregation variable reflects the causal impact of segregation on poverty, addressing potential biases in the OLS estimate.

## Testing Robustness

We test the robustness of our results by including additional control variables, such as city size, historical population composition, and other demographic characteristics. These controls help account for other factors that could influence both segregation and poverty.

## Conclusion

The IV approach provides a more reliable estimate of the causal impact of segregation on poverty rates compared to OLS, addressing endogeneity concerns. By using instruments related to railroad infrastructure, we obtain a clearer understanding of how segregation influences poverty, particularly for Black populations.
