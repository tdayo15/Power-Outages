# Analysis of Power Outages in the U.S.

DSC 80 Final Project at UC San Diego by Therese Dao (t1dao@ucsd.edu) and Isaiah Fang (ifang@ucsd.edu)

---

## Introduction

In our project, we analyzed a dataset of major power outages in the U.S. from January 2000 to July 2016. The dataset comes from **Purdue University's Laboratory for Advancing Sustainable Critical Infrastructure** on 

> "geographical location of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages." [(link)](https://engineering.purdue.edu/LASCI/research-data/outages)

Before we begin the advanced analysis, we will first be cleaning the data, conducting exploratory data analysis, and analyzing the missingness mechanisms and dependency of the data. Then, we will explore our research question: **When and where do power outages occur, and what is the cause of them?** We will formulate a model that predicts the causes of power outages, and both when and where. The outcome will provide pertinent information that communities would be able to use to prevent potential power outages.

The raw data contains 1534 rows, aka 1534 unique outages. We will be focusing on the following columns:

|Column             |Description|
|---                |---        |
|`'YEAR'`               |Year the outage occurred|
|`'MONTH'`              |Month the outage occurred|
|`'U.S._STATE'`             |State where the outage occurred|
|`'CLIMATE.REGION'`             |Region of the U.S. (North/South/East/West/Central)|
|`'CLIMATE.CATEGORY'`               |Category of the Climate (cold, normal, warm)|
|`'OUTAGE.START.DATE'`              |Start date of outage|
|`'OUTAGE.START.TIME'`              |Start time of outage|
|`'OUTAGE.RESTORATION.DATE'`                |Date when power was restored|
|`'OUTAGE.RESTORATION.TIME'`                |Time when power was restored|
|`'CAUSE.CATEGORY'`             |Category of event that caused outage|
|`'CAUSE.CATEGORY.DETAIL'`              |Detailed Category of event that caused outage|
|`'OUTAGE.DURATION'`                |Duration of outage (Minute)|
|`'DEMAND.LOSS.MW'`             |Demand lost during the outage (Megawatt)|
|`'CUSTOMERS.AFFECTED'`             |Number of customers affected by outage|
|`'TOTAL.SALES'`                |Total power consumption (Megawatt per hour)|
|`'TOTAL.CUSTOMERS'`                |Annual number of customers|
|`'POPULATION'`             |Number of people|
|`'PCT_LAND'`               |Percent of Land|
|`'PCT_WATER_TOT'`              |Percent of Water|

---

## Data Cleaning and Exploratory Data Analysis

While the raw dataset is very comprehensive, it was necessary to perform data cleaning to increase efficiency in our analysis, handle missing data, and ensure conistency throughout the dataset. To fit the dataframe to our needs, it was reformatted so that the variables were properly displayed and all values were of the correct type. After this, the dataframe was reduced to only contain the variables needed for our analysis (listed above). These steps made the data more readable, concise, and easier to access.

**just pick 1-2 plots from univariate and 1-2 plots from bivariate and write a quick description for each**

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

Additionally, we performed various aggregations on the dataset to get an idea of how the variables might relate to each other.
**add 1-2 plots from aggregate section and write a brief description for each**



---

## Assessment of Missingness

### NMAR Analysis

Among the columns from our dataset, we believe the column `'DEMAND.LOSS.MW'` is most likely NMAR (Not Missing at Random). The missingness could be blammed on the data collecting process of the power measurement conditions and event type. During severe outages, the tools needed to measure power loss may have been non-functional, offline, or damaged. Based on our domain knowledge of catastrophic events, like extreme storms, we believe that missing values tend to occur more frequently.

The conditions that lead to high demand loss values are the same conditions that make it impossible to measure those values, making an NMAR pattern where the missingness is directly dependent on themself (the missing high demand loss would account for why the data is missing--as explained above).

Additional data could be used to label the column `'DEMAND.LOSS.MW'` as MAR (Missing at Random), such as individually recording the power grid monitoring system status during the outages. Then, I could analyze whether the missingness of the demand loss is dependent on the power measurement tool.

### Missingness Dependency

We will focus on the column `'CAUSE.CATEGORY.DETAIL'`, as it has non-trivial missingness, to test missingness dependency against the columns `'MONTH'` and `'CLIMATE.CATEGORY'`.

First we will examine the distribution of `'MONTH'` when `'CAUSE.CATEGORY.DETAIL'` is missing vs not missing.

**Null Hypothesis**: The distribution of `'MONTH'` is the same when `'CAUSE.CATEGORY.DETAIL'` is missing vs not missing.

**Alternate Hypothesis**: The distribution of `'MONTH'` is different when `'CAUSE.CATEGORY.DETAIL'` is missing vs not missing.

<iframe src="assets/3_2_1.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/3_2_2.html" width="800" height="600" frameborder="0"></iframe>

**Response**: We found an observed TVD of 0.152 and a p_value of 0.007. The empirical distribution of the TVDs is shown above. At p_value = 0.007, we reject the Null Hypothesis and favor the Alternate Hypothesis that the distribution of `'MONTH'` is significantly different when `'CAUSE.CATEGORY.DETAIL'` is missing vs not missing. This shows that the missingness of `'CAUSE.CATEGORY.DETAIL'` is dependent on `'MONTH'`.

Second, we will focus on the dependency of `'CAUSE.CATEGORY.DETAIL'` with `'CLIMATE.CATEGORY'` when `'CAUSE.CATEGORY.DETAIL'` is missing vs not missing.

**Null Hypothesis**: The distribution of `'CLIMATE.CATEGORY'` is the same when `'CAUSE.CATEGORY.DETAIL'` is missing vs not missing.

**Alternate Hypothesis**:  The distribution of `'CLIMATE.CATEGORY'` is different when `'CAUSE.CATEGORY.DETAIL'` is missing vs not missing.

<iframe src="assets/3_2_3.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/3_2_4.html" width="800" height="600" frameborder="0"></iframe>

**Response**: We found an observed TVD of 0.096 and a p_value of 0.541. The empirical distribution of the TVDs is shown above. At p_value = 0.541, we fail to reject the Null Hypothesis that the distribution of `'CLIMATE.CATEGORY'` is significantly similar when `'CAUSE.CATEGORY.DETAIL'` is missing vs not missing. This shows that the missingness of `'CAUSE.CATEGORY.DETAIL'` is not dependent on `'CLIMATE.CATEGORY'`.

---

## Hypothesis Testing

To aid in identifying which regions are more prone to power outages, we want to study the relationship between state populations and power outage occurences.

<iframe src="assets/4_1_1.html" width="800" height="600" frameborder="0"></iframe>
Based on the observations above, it seems that states with smaller populations might experience a higher occurence rate of power outages. This could be due to a multitude of reasons such as sparse infrastructure investment, geographical/environmental factors, limited emergency response resources, etc.

**Question**: Do states with smaller populations experience power outages at a higher rate compared to other regions of the United States?

**Null Hypothesis**: Power outages in states with smaller populations occur at the same rate as all other regions of the US.

**Alternative Hypothesis**: Power outages in states with smaller populations occur at a higher rate than all other regions of the US.

**insert image of test-stat and p-value (output of the code for hypothesis test)**

Due to the result of the hypothesis test not being significant enough, we cannot conclude that states with smaller populations have higher rates of power outage occurences compared to other regions in the United States. However, this does not absolutely conclude that regions with differing populations have the same rates of power outage occurences either; the findings of this hypothesis tests merely show that higher rates of power outages are not solely due to population.

---



## Framing a Prediction Problem

**Prediction Problem:** What are the main causes of power outages, and is there a way to accurately predict them?

To answer this, we will build a **multi-class classification model** that utilizes information from the dataframe to predict the cause of a major power outage. Our response variable is the 'cause.category' column. To evaluate the performance of our model, we will be looking at **f1-score** since it will account for both the precision and recall of the results and provides more in-depth analysis than accuracy. Since our model is multi-class, accuracy would not be able to properly evaluate all classes, thus giving an inaccurate conclusion regarding how well the model performed.

---



## Baseline Model

The baseline model predicts the cause of major power outages ('cause.category') using two features:

**population** (numerical): The population of the affected state.
<br> **climate.region** (categorical): The climate classification of the region.

<br>The model uses a pipeline to preprocess these features:

**Numerical feature:** Standardized using StandardScaler for uniform scaling.
<br> **Categorical feature:** Encoded using OneHotEncoder to convert categories into binary format.

<br> The pipeline integrates these preprocessing steps with a Random Forest Classifier to learn patterns in the data and make predictions.

**insert image of classification report here**

As we can see from the classification report above, the baseline model is not good as it does not accurately identify the correct cause of the power outages most of the time. This could be due to several reasons, for example, the chosen features not being a good measure for prediction, the training set not being an ideal size, not enough data, etc.


---



## Final Model

The improved model predicts the cause of major power outages ('cause.category') based on features like population, affected customers, outage duration, and engineered features such as 'affected_rate' (affected customers relative to population) and 'duration_per_customer' (outage duration per affected customer).

Features such as 'population' and 'climate.region' were kept as they still provide essential information about how power outages occur. However, we created new features to depict how the variables affect each other, thus allowing the model to capture more complex relationships between the data. The final model also incorporates more detailed preprocessing with the use of one-hot encoding, and the addition of cross-validation helped generalize the model in order to make it more equipped to handle unseen data.

<br>**Feature Engineering:**

'affected_rate': ratio of affected customers to the population.
<br>'duration_per_customer': average duration of the outage per affected customer.

<br>**Preprocessing:**
Standard scaling for numerical features.
<br>One-hot encoding for categorical features (e.g., 'climate.region').

<br>**Cross-Validation:**

Used Stratified K-Fold cross-validation (5 splits) to assess the model’s performance and ensure robust evaluation.

<br>**Model Training:**

A Random Forest classifier was trained on the features to predict the outage cause. 

To briefly summarize how it works,  Random Forest Classifier is an ensemble learning method that combines multiple decision trees to make predictions by aggregating their outputs through majority voting. It builds each tree using a random subset of the data (bootstrapping) and selects a random subset of features for each split, which reduces overfitting and increases robustness. In this model, Random Forest effectively predicts the cause of power outages by capturing complex relationships between features like population and climate.region. Its ability to handle mixed data types, measure feature importance, and generalize well to unseen data makes it a strong choice for this classification task.

Cross-Validation Accuracy Scores: [0.79 0.77 0.79 0.79 0.79]
Mean CV Accuracy: 0.785

Classification Report:


|           asdf                |   Precision   |   Recall  |   F1 score    |   Support |
|:------------------------------|:--------------|:----------|--------------:|----------:|
| equipment failure             |     1.00      |    0.98   |     0.99      |     60    |
| fuel supply emergency         |     1.00      |    1.00   |     1.00      |     51    |
| intentional attack            |     0.99      |    1.00   |     1.00      |    418    |
| islanding                     |     1.00      |    0.96   |     0.98      |     46    |
| public appeal                 |     1.00      |    0.99   |     0.99      |     69    |
| severe weather                |     1.00      |    1.00   |     1.00      |    763    |
| system operability disruption |     0.99      |    0.99   |     0.99      |    127    |
|                               |               |           |               |           |
| accuracy                      |               |           |     1.00      |    1534   |
| macro avg                     |     1.00      |    0.99   |     0.99      |    1534   |
| weighted avg                  |     1.00      |    1.00   |     1.00      |    1534   |

Looking at our chosen metrics for determining the performance of the model, the f-1 score here is a significant increase compared to the baseline model. The changes made to the model made it more consistent and provide more informative features, thus allowing for better prediction. The final model was better because it effectively leveraged meaningful features (population, climate.region) alongside robust preprocessing and evaluation methods. These changes ensured that the model not only performed well but also generalized effectively, making it a strong predictor of power outage causes across diverse conditions.

---



## Fairness Analysis

We will perform a Fairness Analysis on our model to see its performance on bigger and smaller populations. The definition of these groups are populations greater than 15_000_000 vs populations less than 15_000_000.

We decided on these groups because population is a heavy factor in determining the cause of major power outages (as predicted by our model). Performing a Fairness Analysis on our model would be beneficial for communities (across a range of population densities) on how to prevent different causes of power outages.

Our evaluation metric will be the difference in accuracy because there is an inbalance of bigger and smaller populations.

**Null Hypothesis**: The model is fair. The difference in accuracy for bigger and smaller populations are similar (any differences are due to random chance).

**Alternate Hypothesis**: The model is unfair. The difference in accuracy for bigger and smaller populations are different.

We performed a permutation test with 1000 trials and got a p_value of 1. Because our p_value = 1, we fail to reject the null hypothesis that our model is fair. The model is significantly similar in terms of difference in accuracy for bigger and smaller populations. The figure below is a distribution of our test statistic:

<iframe src="assets/8_1_1.html" width="800" height="600" frameborder="0"></iframe>
