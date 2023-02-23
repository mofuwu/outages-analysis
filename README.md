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

<iframe src="assets/MAR_duration_state.html" width=800 height=600 frameBorder=0></iframe>

The permutation test above checks the missingness dependency of power outage duration on the US states. A p-value of 0.264 indicats that the missingness of power outage duration is independent of US states.

<iframe src="assets/MAR_duration_category.html" width=800 height=600 frameBorder=0></iframe>

The permutation test above checks the missingness dependency of power outage duration on the category of the cause. A p-value of 0.003 confirms this dependency.

To perform imputation on columns that assossiate with electricity consumptions and columns that assossiate with electricity price, we conduct the same permutation testing one each column and got all p-values greater than 0.05. The results demonstrate the validity of using probabilistic imputation conditioned on the categories of the events that cause the power outages.

---

## Cleaning and EDA

|    | U.S._STATE   | POSTAL.CODE   | CAUSE.CATEGORY     |   OUTAGE.DURATION |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.MONEY.LOST |   COM.MONEY.LOST |   IND.MONEY.LOST |   TOTAL.MONEY.LOST |
|---:|:-------------|:--------------|:-------------------|------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-----------------:|-----------------:|-----------------:|-------------------:|
|  1 | Minnesota    | MN            | severe weather     |              3060 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      13.8015     |       9.90095    |       7.33967    |        1863.55     |
|  2 | Minnesota    | MN            | intentional attack |                 1 |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |       0.00320571 |       0.00292555 |       0.00204211 |           0.490377 |
|  3 | Minnesota    | MN            | severe weather     |              3000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |       7.97474    |       7.37789    |       5.92218    |        1276.81     |
|  4 | Minnesota    | MN            | severe weather     |              2550 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |       9.2775     |       7.63124    |       5.68361    |        1356.17     |
|  5 | Minnesota    | MN            | severe weather     |              1740 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |       7.69004    |       6.36897    |       3.99076    |        1083.51     |

### Data Cleaning Steps:

Based on the result of the missingness dependencies, we perform within-group mean impuatation on the duration of the power outage to account for its dependency on the categories of the event that cause the power outage.

Since we reason that columns related to electricity consumptions and electricity prices are Not Missing At Random, we use probabilistic imputation to replace missing values. As the electricity consumptions of three sectors should add up to the total electricity consumption for each record, we perform the probabilistic imputation of these four columns together to retain consistency. This is the same for the four columns related to electricity price.

For mathematical convenience, we change columns that have numerical values into type of float.

We adopt the money lost during each power outage event as the main measurement of financial severity, which gives very straightfoward assessment of the financial impact of the power outage on the electricity provider. The money lost is calculated by multiplying electricity consumption (megawatts-hour), average monthly electricity price (cents/killowatt-hour), and duration of the outage events (minutes). We calculate the money lost from each sector as well as the total money lost. During the calculate, we take into account the inconsistency of units. The unit of the money lost is billion dollars.

#### Added Columns:

| Added Columns    | Description                                                                                 |
|------------------|---------------------------------------------------------------------------------------------|
|  RES.MONEY.LOST  | In the residential sector, the amount of U.S dollars lost in billion dollars from an outage |
|  COM.MONEY.LOST  | In the commercial sector, the amount of U.S dollars lost in billion dollars from an outage  |
|  IND.MONEY.LOST  | In the industrial sector, the amount of U.S dollars lost in billion dollars from an outage  |
| TOTAL.MONEY.LOST | The amount of U.S dollars lost in billion dollars from an outage                            |

---

---
### Univariate Analysis:

<iframe src="assets/res_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

The histogram shows the distribution of the total money lost in residential sector during each power outage. A significantly right-skewed plot indicates that most of the power outage cause relatively small amount of money lost. The largest number of power outages falls into the range that less than 10 billion dollars are lost. Cases where more than 500 billion dollars are lost are approximately equally rare.

<iframe src="assets/com_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

The histogram shows the distribution of the total money lost in commercial sector during each power outage. Similar to the distribution for residencial sector, most of the money lost are below 15 billion dollars. However, there are more extreme cases relating to commercial sector. Several cases results in more than 1 trillion dollars of money lost.

<iframe src="assets/ind_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

The histogram shows the distribution of the total money lost in industrial sector during each power outage. It bears the same shape as the distribution related to other sectors, and most of the power outage cases have money lost within 15 billion dollars. However, the money lost in industrial sector has a noticeably smaller range. The highest money lost is only around 200 billion dollars.

---
### Bivariate Analysis:
<iframe src="assets/total_money_lost_geospatial.html" width=800 height=600 frameBorder=0></iframe>

The barchart shows that there are remarkable differences between the average money lost under different categories of events that cause the power outage. Power outage that is induced by fuel supply emergency leads to significantly higher money lost during a single power outage than other categories of events. Based on this plot, we can conclude that categories of events is a key characteristic that determines the financial severity of the power outage for the electricity provider. Among all categories, fuel supply emergency is worth particular attention for its tendency to cause huge amount of money lost.

<iframe src="assets/total_money_lost_vs_cause_category.html" width=800 height=600 frameBorder=0></iframe>

Besides the categories of events that cause the power outage, we also look into the geographical factors. The choropleth graph depicts the average money lost due to power outage in each US state. Over the different states, we can observe some variations in the average money lost. The financial lost on average is the highest in New York, the financial center of US. Texas has the second highest money lost on average, possibly due to its large number of residents as well as industries.

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

Given that the distributions of money lost in residential, commercial, and industrial sector are very similar, it is hard to tell whether the money lost in one sector is different from the other. In respond to this, we plot a pivot table to investigate the difference between money lost of three sectors under different categories of events that cause the power outage. This can also help us explore the pattern of money lost due to different categories of events for each sector respectively.

In this pivot table, we observe that the average money lost from industrial sector is significantly smaller under all categories of events than from the other two sectors, which is consistent with the conclusion we draw in the univariate analysis. The average money lost in residential and commercial sectors are very close. There is a slightly higher average money lost in residential sector due to equipment failure, intentional attack, public appeal, severe whether, and system operability disruption. The rest categories of events results in slightly higher average money lost in commercial sector.

The patterns of average money lost under different categories of events are consistent for all three sectors, indicating that the power outages caused by different categories of events have the same relative influence on the money lost for all three sectors.

---

## Hypothesis Testing
Null hypothesis H0: RES.MONEY.LOST = COM.MONEY.LOST

Alternate Hypothesis H1: RES.MONEY.LOST > COM.MONEY.LOST

<iframe src="assets/hypothesis_testing.html" width=800 height=600 frameBorder=0></iframe>

Summary:
Unfortunately, the pivot table cannot tell a clear difference between the overall money lost in the commercial sector and the residential sector. Given that the average money lost is higher in residential sector under more categories of events, we decide to use permutation testing to further investigate whether the money lost in residential sector is larger than the money lost in commercial sector.

The null hypothesis is that, the average money lost in the residential sector is equal to the average money lost in the commercial sector during power outages . The alternative hypothesis is that, the average money lost in the residential sector is larger than the average money lost in the commercial sector during power outages. For the test statistic, we use the difference of the mean of two samples.

The p-value calculated using the simulation is 0.144. Under significance level of 0.05, we fail to reject null hypothesis. This result suggests that, although there are slight differences in the average money lost from residential sector and from commercial sectors under different categories of events that cause the power outage, we cannot say that the average money lost from two sectors are different.