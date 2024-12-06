# Analysis of Power Outages in the U.S.

DSC 80 Final Project at UC San Diego by Therese Dao (t1dao@ucsd.edu) and Isaiah Fang (ifang@ucsd.edu)

---

## Introduction

In our project, we analyzed a dataset of major power outages in the U.S. from January 2000 to July 2016. The dataset comes from **Purdue University's Laboratory for Advancing Sustainable Critical Infrastructure** on 

> "geographical location of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages." [(link)](https://engineering.purdue.edu/LASCI/research-data/outages)

*insert brief significance of the data*

### Question: When and where do power outages occur, and what is the cause of them?

*insert step process of report*

*introduce raw data*

---

## Data Cleaning and Exploratory Data Analysis

<iframe src="assets/2_2_1.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_2_2.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_2_3.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_2_4.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_2_5.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_2_6.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_2_7.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_2_8.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_2_9.html" width="800" height="600" frameborder="0"></iframe>

<iframe src="assets/2_3_1.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_3_2.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/2_3_3.html" width="800" height="600" frameborder="0"></iframe>

---

## Assessment of Missingness

### NMAR Analysis

Among the columns from our dataset, we believe the column 'DEMAND.LOSS.MW' is most likely NMAR (Not Missing at Random). The missingness could be blammed on the data collecting process of the power measurement conditions and event type. During severe outages, the tools needed to measure power loss may have been non-functional, offline, or damaged. Based on our domain knowledge of catastrophic events, like extreme storms, we believe that missing values tend to occur more frequently.

The conditions that lead to high demand loss values are the same conditions that make it impossible to measure those values, making an NMAR pattern where the missingness is directly dependent on themself (the missing high demand loss would account for why the data is missing--as explained above).

Additional data could be used to label the column 'DEMAND.LOSS.MW' as MAR (Missing at Random), such as individually recording the power grid monitoring system status during the outages. Then, I could analyze whether the missingness of the demand loss is dependent on the power measurement tool.

### Missingness Dependency

We will focus on the column 'CAUSE.CATEGORY.DETAIL', as it has non-trivial missingness, to test missingness dependency against the columns 'MONTH' and 'CLIMATE.CATEGORY'.

First we will examine the distribution of 'MONTH' when 'CAUSE.CATEGORY.DETAIL' is missing vs not missing.

**Null Hypothesis**: The distribution of 'MONTH' is the same when 'CAUSE.CATEGORY.DETAIL' is missing vs not missing.

**Alternate Hypothesis**: The distribution of 'MONTH' is different when 'CAUSE.CATEGORY.DETAIL' is missing vs not missing.

<iframe src="assets/3_2_1.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/3_2_2.html" width="800" height="600" frameborder="0"></iframe>

**Response**: We found an observed TVD of 0.152 and a p_value of 0.007. The empirical distribution of the TVDs is shown above. At p_value = 0.007, we reject the Null Hypothesis and favor the Alternate Hypothesis that the distribution of 'MONTH' is significantly different when 'CAUSE.CATEGORY.DETAIL' is missing vs not missing. This shows that the missingness of 'CAUSE.CATEGORY.DETAIL' is dependent on 'MONTH'.

Second, we will focus on the dependency of 'CAUSE.CATEGORY.DETAIL' with 'CLIMATE.CATEGORY' when 'CAUSE.CATEGORY.DETAIL' is missing vs not missing.

**Null Hypothesis**: The distribution of 'CLIMATE.CATEGORY' is the same when 'CAUSE.CATEGORY.DETAIL' is missing vs not missing.

**Alternate Hypothesis**:  The distribution of 'CLIMATE.CATEGORY' is different when 'CAUSE.CATEGORY.DETAIL' is missing vs not missing.

<iframe src="assets/3_2_3.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/3_2_4.html" width="800" height="600" frameborder="0"></iframe>

**Response**: We found an observed TVD of 0.096 and a p_value of 0.541. The empirical distribution of the TVDs is shown above. At p_value = 0.541, we fail to reject the Null Hypothesis that the distribution of 'CLIMATE.CATEGORY' is significantly similar when 'CAUSE.CATEGORY.DETAIL' is missing vs not missing. This shows that the missingness of 'CAUSE.CATEGORY.DETAIL' is not dependent on 'CLIMATE.CATEGORY'.

---

## Hypothesis Testing

<iframe src="assets/4_1_1.html" width="800" height="600" frameborder="0"></iframe>

---



## Framing a Prediction Problem



---



## Baseline Model



---



## Final Model



---



## Fairness Analysis

