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

---

## Cleaning and EDA

### Data Cleaning Steps:

---
### Univariate Analysis:
<iframe src="assets/com_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/ind_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/res_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

---
### Bivariate Analysis:
<iframe src="assets/total_money_lost_geospatial.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/total_money_lost_vs_cause_category.html" width=800 height=600 frameBorder=0></iframe>

---

### Pivot Table Analysis:

| CAUSE.CATEGORY                |   RES.MONEY.LOST |   COM.MONEY.LOST |   IND.MONEY.LOST |
|:------------------------------|-----------------:|-----------------:|-----------------:|
| equipment failure             |         12.7361  |         11.8952  |          5.6062  |
| fuel supply emergency         |        162.82    |        165.789   |         44.5407  |
| intentional attack            |          2.74004 |          2.50216 |          1.15315 |
| islanding                     |          2.68427 |          3.05349 |          1.06663 |
| public appeal                 |         20.4576  |         16.5303  |          6.60629 |
| severe weather                |         34.5171  |         29.1123  |         13.0284  |
| system operability disruption |          7.42418 |          6.67347 |          3.30064 |
---

## Hypothesis Testing
