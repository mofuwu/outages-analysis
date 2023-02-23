# Exploring Major Outages in the U.S
by Sean Chan (swchan@ucsd.edu) and Junyi Xu (jux006@ucsd.edu)


## Introduction
Question: What are factors of major outages which lead to significant financial loss?

Significance: Outages lead to huge financial losses for electricity providers as the income comes to a halt. In our project, we wanted to look at the characteristics of outages which lead to a greater loss in money. By identifying these specific characteristics, electricity providers
can look towards creating preventative measures against outages with these characteristics.

For our project, we were given a dataset of major outages in the United States between January 2000 and July 2016. Each row represents a single documented outage with their columns containing information on data such as but not limited to the year, month, state, and cause of the outage.
There are 1535 rows in the dataset which indicates that the dataset documents 1535 outage events. The columns that we look at in our project are detailed in the table below.

---
<iframe src="assets/table.html" width=800 height=600 frameBorder=0></iframe>

---

## Assessment of Missingness

### NMAR Analysis:


---

### Missingness Dependency:

<iframe src="assets/MAR_duration_category.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/MAR_duration_state.html" width=800 height=600 frameBorder=0></iframe>

---

## Cleaning and EDA

### Data Cleaning Steps:

---
### Univariate Analysis:

<iframe src="assets/res_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/com_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/ind_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>


---
### Bivariate Analysis:
<iframe src="assets/total_money_lost_geospatial.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/total_money_lost_vs_cause_category.html" width=800 height=600 frameBorder=0></iframe>

---

### Pivot Table Analysis:



| CAUSE.CATEGORY                |   RES.MONEY.LOST |   COM.MONEY.LOST |   IND.MONEY.LOST |
|:------------------------------|-----------------:|-----------------:|-----------------:|
| equipment failure             |         12.5011  |         11.5861  |          5.62598 |
| fuel supply emergency         |        179.434   |        184.48    |         57.5365  |
| intentional attack            |          2.86007 |          2.61357 |          1.22069 |
| islanding                     |          2.60097 |          2.95016 |          1.03395 |
| public appeal                 |         20.4576  |         16.5303  |          6.60629 |
| severe weather                |         34.252   |         28.9607  |         12.9862  |
| system operability disruption |          7.41131 |          6.70593 |          3.28277 |

---

## Hypothesis Testing

<iframe src="assets/hypothesis_testing.html" width=800 height=600 frameBorder=0></iframe>

Summary:
Unfortunately, the pivot table cannot tell a clear difference between the overall money lost in the commercial sector and the residential sector. Given that the average money lost is higher in residential sector under more categories of events, we decide to use permutation testing to further investigate whether the money lost in residential sector is larger than the money lost in commercial sector.

The null hypothesis is that, the average money lost in the residential sector is equal to the average money lost in the commercial sector during power outages . The alternative hypothesis is that, the average money lost in the commercial sector is larger than the average money lost in the residential sector during power outages. For the test statistic, we use the difference of the mean of two samples.

The p-value calculated using the simulation is 0.144. Under significance level of 0.05, we fail to reject null hypothesis. This result suggests that, although there are slight differences in the average money lost from residential sector and from commercial sectors under different categories of events that cause the power outage, we cannot say that the average money lost from two sectors are different.