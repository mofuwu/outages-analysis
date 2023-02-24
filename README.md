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

The column `CUSTOMERS.AFFECTED` may be NMAR. The electricity provider may choose not to report the number of customers affected if there are too few people influenced by the power outage. Additional data about the range of area under the influence of the power outage may be useful to explain the missingness of the number of customers affected. 

---

### Missingness Dependency:

<iframe src="assets/MAR_duration_state.html" width=800 height=600 frameBorder=0></iframe>

The permutation test above checks the missingness dependency of power outage duration on the US states. A p-value of 0.252 indicats that the missingness of power outage duration is independent of the US states.

<iframe src="assets/MAR_duration_category.html" width=800 height=600 frameBorder=0></iframe>

The permutation test above checks the missingness dependency of power outage duration on the category of events that cause the power outage. A p-value of 0.002 confirms this dependency.

---

## Cleaning and EDA

Head of Cleaned Data:

<iframe src="assets/cleaned_table.html" width=800 height=600 frameBorder=0></iframe>


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

The histogram shows the distribution of the total money lost in residential sector during each power outage. More than half of the power outage events lead to money lost less than 10000 dollars. More than 25% of cases cause money lost between 10000 dollars and 30000 dollars. Only about 10% of the cases lead to more than 300000 dollars. The highest money lost in residential sector is about 2 million dollars.

<iframe src="assets/com_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

The histogram shows the distribution of the total money lost in commercial sector during each power outage. Similar to the distribution for residential sector, over 60% of the power outage events lead to money lost less than 10000 dollars. About 25% of cases, the money lost is between 10000 dollars and 30000 dollars. Only about 10% of the cases lead to more than 300000 dollars. The highest money lost in commercial sector is about 1.5 million dollars, slightly smaller than the highest money lost in residencial sector.

<iframe src="assets/ind_money_lost_hist.html" width=800 height=600 frameBorder=0></iframe>

The histogram shows the distribution of the total money lost in industrial sector during each power outage. The shape of the distribution is similar to the other two sectors, but the money lost is in general significantly smaller. About 50% of the power outage events lead to money lost less than 2500 dollars. About 25% of cases, the money lost is between 2500 dollars and 7500 dollars. The rest 25% of the cases lead to more than 7500 dollars. The highest money lost in industrial sector is about 935000 dollars. 

---
### Bivariate Analysis:
<iframe src="assets/total_money_lost_geospatial.html" width=800 height=600 frameBorder=0></iframe>

The barchart shows that there are remarkable differences between the average money lost under different categories of events that cause the power outage. Power outage that is induced by fuel supply emergency leads to significantly higher money lost during a single power outage than other categories of events. Based on this plot, we can conclude that categories of events is a key characteristic that determines the financial severity of the power outage for the electricity provider. Among all categories, fuel supply emergency is worth particular attention for its tendency to cause huge amount of money lost.

<iframe src="assets/total_money_lost_vs_cause_category.html" width=800 height=600 frameBorder=0></iframe>

Besides the categories of events that cause the power outage, we also look into the geographical factors. The choropleth graph depicts the total money lost due to power outage in each US state. Over the different states, we can observe some variations in the average money lost. The total financial lost is significantly higher in New York and North Dakota. Texas and Florida also have relatively high money lost. Many areas on the west coast show greater money lost than east coast and the middle states. In general, there are some variations in the money lost across different states, so US states may be another factor that influence the severity of financial lost.

---

### Pivot Table Analysis:

| CAUSE.CATEGORY                |   RES.MONEY.LOST |   COM.MONEY.LOST |   IND.MONEY.LOST |
|:------------------------------|-----------------:|-----------------:|-----------------:|
| equipment failure             |          8238.73 |          7363.88 |          3958.64 |
| fuel supply emergency         |         81473.2  |         93256.9  |         12059.6  |
| intentional attack            |          5803.57 |          4270.43 |          1242.39 |
| islanding                     |         78455.6  |         80738.6  |         28786.4  |
| public appeal                 |         25613.4  |         28884.1  |         11478.6  |
| severe weather                |         19796.2  |         17231.3  |          7812.71 |
| system operability disruption |         24304.4  |         29013.8  |          7424.34 |

The pivot table shows how much money is lost from the residential, commercial, and industrial sector from different outage causes.

Given that the distributions of money lost in residential, commercial, and industrial sector are very similar, it is hard to tell whether the money lost in one sector is different from the other. In respond to this, we plot a pivot table to investigate the difference between money lost of three sectors under different categories of events that cause the power outage. This can also help us explore the pattern of money lost due to different categories of events for each sector respectively.

In this pivot table, we observe that the average money lost from industrial sector is significantly smaller under all categories of events than from the other two sectors, which is consistent with the conclusion we draw in the univariate analysis. The average money lost in residential and commercial sectors are very close. There is a slightly higher average money lost in residential sector due to equipment failure, intentional attack, public appeal, severe whether, and system operability disruption. The rest categories of events results in slightly higher average money lost in commercial sector.

The patterns of average money lost under different categories of events are consistent for all three sectors, indicating that the power outages caused by different categories of events have the same relative influence on the money lost for all three sectors.

---

## Hypothesis Testing
Null hypothesis H0: RES.MONEY.LOST = COM.MONEY.LOST

Alternate Hypothesis H1: RES.MONEY.LOST > COM.MONEY.LOST

<iframe src="assets/hypothesis_testing.html" width=800 height=600 frameBorder=0></iframe>

The graph above shows the distribution of our permutation test between residential money loss and commercial money loss.

Unfortunately, the pivot table cannot tell a clear difference between the overall money lost in the commercial sector and the residential sector. Given that the average money lost is higher in residential sector under more categories of events, we decide to use permutation testing to further investigate whether the money lost in residential sector is larger than the money lost in commercial sector. 

The null hypothesis is that, the average money lost in the residential sector is equal to the average money lost in the commercial sector during power outages . The alternative hypothesis is that, the average money lost in the residential sector is larger than the average money lost in the commercial sector during power outages. For the test statistic, we use the difference of the mean of two samples.

The p-value calculated using the simulation is 0.367. Under significance level of 0.05, we fail to reject null hypothesis. This result suggests that, although there are slight differences in the average money lost from residential sector and from commercial sectors under different categories of events that cause the power outage, we cannot say that the average money lost from two sectors are different.