<h2 align="center">Reproducing Research: Causal Impact of Segregation on Poverty Rates</h2>  
    
           
  
<table width="80%" align="center">
  <tr>   
    <td colspan="2" align="center"><strong>Table of Contents</strong></td>  
  </tr> 
  <tr>
    <td>1. <a href="#key-variables">Key Variables</a></td>   
    <td>5. <a href="#assessing-instrument-strength">Assessing Instrument Strength</a></td>
  </tr>
  <tr> 
    <td>2. <a href="#data-overview">Data Overview</a></td>   
    <td>6. <a href="#reduced-form">Reduced Form</a></td>
  </tr>
  <tr>
    <td>3. <a href="#summary-statistics">Summary Statistics</a></td>   
    <td>7. <a href="#iv-estimation-second-stage">IV Estimation: Second Stage</a></td>
  </tr>
  <tr>
    <td>4. <a href="#ols-regression-of-poverty-rates-on-segregation">OLS Regression of Poverty Rates on Segregation</a></td>   
    <td>8. <a href="#conclusion">Conclusion</a></td>
  </tr>
  <tr>
    <td colspan="2">9. <a href="#references">References</a></td>   
  </tr>
</table>




## Overview
This project replicates the analysis from Elizabeth Ananat’s paper, *The Wrong Side(s) of the Tracks: The Causal Effects of Racial Segregation on Urban Poverty and Inequality* (American Economic Journal: Applied Economics, 2011). The study investigates how racial segregation influences urban poverty and inequality, using railroad track layouts as an instrumental variable (IV) to estimate the causal impact of segregation on poverty rates.

<br>

 [![View Original Research Paper](https://img.shields.io/badge/View%20Original%20Research%20Paper-0056A0?style=flat&logo=external-link&logoColor=white&color=0056A0)](https://www.aeaweb.org/articles?id=10.1257/app.3.2.34)

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

This data is from the AER’s website, which links to the ICPSR’s data repository. Anyone can sign in to access the replication data files.

Each observation represents a city. The dataset spans multiple years, capturing variables related to racial composition, economic outcomes, and urban infrastructure. We focus on the relationships between segregation (measured by the dissimilarity index) and poverty rates among Black and White populations.

[![OpenICPSR Data](https://img.shields.io/badge/OpenICPSR_Data-0056A0)](https://www.openicpsr.org/openicpsr/project/113786/version/V1/view?path=/openicpsr/113786/fcr:versions/V1/data&type=folder)


## Summary Statistics

We begin by examining summary statistics for key variables: `dism1990`, `herf`, `lenper`, and poverty rates (`povrate_w`, `povrate_b`). These statistics provide an overview of the distribution and variability of these variables across cities.

<img src="https://github.com/RoryQo/Research-Reproduction_Causal-Effect-of-Segregation-on-Poverty-Rates/blob/main/Figures/Sum.jpg" width=450px>

```
summary_stats <- df %>%  summarise(
       Mean = colMeans(., na.rm = TRUE),
          SD = sapply(., sd, na.rm = TRUE),
             Min = sapply(., min, na.rm = TRUE),
                Max = sapply(., max, na.rm = TRUE))
```


## OLS Regression of Poverty Rates on Segregation

A simple OLS regression is run to explore the relationship between racial segregation and poverty rates for both White and Black populations.



**Regression Model:**

$$
\text{Poverty Rate (White/Black)} = \beta_0 + \beta_1 \times \text{Segregation} + \epsilon
$$

<p align="center">
  <img src="https://github.com/RoryQo/Research-Reproduction_Causal-Effect-of-Segregation-on-Poverty-Rates/blob/main/Figures/NonCausalTable.jpg" width="450">
</p>


A one standard deviation increase in the segregation index is associated with a one percentage point decrease in white poverty and a 2.5 percentage point increase in black poverty.

This model estimates how changes in segregation are associated with changes in poverty rates. However, this approach does not account for potential confounders that could bias the results.


## Addressing Causality: Instrumental Variables Approach

Given that the OLS model may suffer from endogeneity (i.e., omitted variable bias), we apply an instrumental variables (IV) approach to estimate the causal effect of segregation on poverty. The instrument used is the railroad division index (`herf`), with  track length per square kilometer (`lenper`) as a control, RDI is correlated with segregation but assumed not to directly affect poverty rates, as railroads were laid out randomly or according to transport efficiency before the cities developed.

**First Stage Regression:**

$$
\text{Segregation (1990)} = \beta_0 + \beta_1 \times \text{RDI} + \beta_2 \times \text{Track Length} + \epsilon
$$

<p align="center">
  <img src="https://github.com/RoryQo/Research-Reproduction_Causal-Effect-of-Segregation-on-Poverty-Rates/blob/main/Figures/First-stage.jpg" width="340" style="display: inline-block; margin-right: 10px;">
  <img src="https://github.com/RoryQo/Research-Reproduction_Causal-Effect-of-Segregation-on-Poverty-Rates/blob/main/Figures/Graph.jpg" width="400" style="display: inline-block;">
</p>



In the first stage, we regress segregation on the instrument(s) to check whether the instruments are significantly correlated with segregation.

A one standard deviation increase in the RDI is associated
with a 5 point (0.14 ∗ (0.357) = 0.049) increase in the segregation index.

## Assessing Instrument Strength

To assess the strength of the instrument, we examine the F-statistic from the first stage regression. A value greater than 10 indicates a sufficiently strong instrument. If the instrument is weak, IV estimates will be unreliable.  Our model has an F statistic of 14.98.

```
summary(model)$fstat[1]
```

## Reduced Form

Next, we estimate the reduced form equation, regressing poverty rates directly on the instrument (RDI and track length), without first modeling segregation. This step helps to understand the relationship between the instrument and the outcomes.

<p align="center">
  <img src="https://github.com/RoryQo/Research-Reproduction_Causal-Effect-of-Segregation-on-Poverty-Rates/blob/main/Figures/Graph2.jpg" width="475">
</p>

## IV Estimation: Second Stage

In the second stage, we regress poverty rates on the predicted values of segregation from the first stage. This provides an IV estimate of the causal effect of segregation on poverty rates.

**Second Stage Model:**

$$
\text{Poverty Rate (White/Black)} = \alpha_0 + \alpha_1 \times \hat{\text{Segregation}} + \epsilon
$$

```
 model_BI<-felm(povrate_b~lenper|0|(dism1990~herf+lenper),data=df)
```

<p align="center">
  <img src="https://github.com/RoryQo/Research-Reproduction_Causal-Effect-of-Segregation-on-Poverty-Rates/blob/main/Figures/IVO.jpg" width=450px>
</p>


The coefficient on the predicted segregation variable reflects the causal impact of segregation on poverty, addressing potential biases in the OLS estimate.  

The results indicate segregation has a statistically significant *causal* impact on the poverty rates of the black population (increasing segregation induces higher poverty rates among the black population).  The regression also indicates a statistically significant causal impact on the poverty rates among the white population (increasing segregation induces lower poverty rates among the white population).

## Testing Robustness

We test the robustness of our results by including additional control variables, such as city size, historical population composition, and other demographic characteristics. These controls help account for other factors that could influence both segregation and poverty.

## Conclusion

The IV approach provides a more reliable estimate of the causal impact of segregation on poverty rates compared to OLS, addressing endogeneity concerns. By using instruments related to railroad infrastructure, we obtain a clearer understanding of how segregation influences poverty, particularly for Black populations. The results indicate segregation has a statistically significant *causal* impact on the poverty rates of the black population (increasing segregation induces higher poverty rates among the black population).

Comparing the simple regression 

$$
\text{Poverty Rate (White/Black)} = \beta_0 + \beta_1 \times \text{Segregation} + \epsilon
$$

with the IV regression

$$
\text{Poverty Rate (White/Black)} = \alpha_0 + \alpha_1 \times \hat{\text{Segregation}} + \epsilon
$$

<p align="center">
  <img src="https://github.com/RoryQo/Research-Reproduction_Causal-Effect-of-Segregation-on-Poverty-Rates/blob/main/Figures/IVR.jpg" width=550px>
</p>

## References

Ananat, Elizabeth Oltmans. “The Wrong Side(S) of the Tracks: The Causal Effects of Racial Segregation on Urban Poverty and Inequality.” American Economic Journal: Applied Economics, vol. 3, no. 2, 1 Apr. 2011, pp. 34–66, www.nber.org/system/files/working_papers/w13343/w13343.pdf, https://doi.org/10.1257/app.3.2.34.
