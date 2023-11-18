# Power-Outage-Analysis
### Name: Jason Zhang
## Introduction

Power outages, often unforeseen disruptions in electrical supply, underscore the significance of electricity in modern
life. They expose our dependency on power for daily routines, from lighting our homes to sustaining essential services.
Beyond inconvenience, outages highlight vulnerabilities in critical infrastructure, impacting healthcare and communication.
and public safety. Understanding the causes is crucial for resilience and preparedness.

In this project, I will find all possible characteristics of major power outages with higher severity and factors of
outage duration.
There are 1534 rows and 55 columns in the original dataset. It contained the major outages witnessed by different states 
in the continental U.S. during January 2000–July 2016.

| Column             | Description                                                                                     |
|--------------------|-------------------------------------------------------------------------------------------------|
| `MONTH`            | Indicates the month when the outage event occurred                                              |
| `POSTAL.CODE`      | Represents the postal code of the U.S. states                                                   |
| `CLIMATE.REGION`   | U.S. Climate regions as specified by National Centers for Environmental Information             |
| `CAUSE.CATEGORY`   | Categories of all the events causing the major power outages                                    |
| `OUTAGE.DURATION`  | Duration of outage events (in minutes)                                                          |
| `CUSTOMERS.AFFECTED` | Number of customers affected by the power outage event                                          |
|`Overall Effect`| `OUTAGE.DURATION` times `CUSTOMERS.AFFECTED` for measurement of how severe the power outage is. |
|`ANOMALY.LEVEL`| The oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season.      |
|`RES.PRICE`|	Monthly electricity price in the residential sector (cents/kilowatt-hour)|

---
## Data Cleaning and Exploratory Data Analysis
First, I select all the columns that relate to my research, which is shown above.
Then, since `OUTAGE.DURATION` is one of the key measurements for the effect of a power outage on residents, I decided to fill
the nan values and keep only valid rows.
For the rows that contain `OUTAGE.DURATION` is nan, if the `MONTH` value is also nan, we assume the power outage of the row is not
servere since otherwise the month it happened should be recored even though people do not know the `OUTAGE.DURATION`.
Then, I fill in all the nan values in `OUTAGE.DURATION` by taking the median of other power outage records for the same year, month, cause, and state.
which is reasonable if they share these categories; the duration should be the same. Do the same process to
nan value of `CUSTOMERS.AFFECTED`.
Next, create a column `Overall Effect` which is defined by 'outage duration' times 'customers affected' for measurement of
how severe the power outage is.

| MONTH   | POSTAL.CODE  | CAUSE.CATEGORY|	CLIMATE.REGION|	CUSTOMERS.AFFECTED|	OUTAGE.DURATION| Overall Effect  |
|-------- |--------------|---------------|----------------|-------------------|----------------|-----------------|
|7.0	|MN	|severe weather|East North Central|	70000.0|	3060.0|	214200000.0|
|5.0	|MN	|intentional attack|East North Central|	2970.5|	1.0|	2970.5|
|10.0	|MN|	severe weather|	East North Central|	70000.0|	3000.0|	210000000.0|
|6.0	|MN|	severe weather|	East North Central|	68200.0|	2550.0|	173910000.0|
|7.0	|MN|	severe weather|	East North Central|	250000.0|	1740.0|	435000000.0|

### Univariate Analysis:
<iframe src="pics/Pie of Cause Category.html" width=800 height=600 frameBorder=0></iframe>

The pie chart shows the percentage of each cause category that responded to the power outage.
As you can see, severe weather and intentional attacks are two primary factors.

### Bivariate Analysis:
<iframe src="pics/Choropleth_Us_outage.html" width=800 height=600 frameBorder=0></iframe>
The Choropleth map of the U.S. states and the number of power outages that happened in that state
Many of the power outages happened in CA and TX, as well as in areas near the Northwest, South, and West.

### Interesting Aggregates:
The pivot table takes cause categories as columns and displays the median overall effect value related to each climate.
region. For example, although we know the intentional attack is the second reason for the occurrence of a power outage,
Its overall effects are way less than those of equipment failure and severe weather.

|   equipment failure |   fuel supply emergency |   intentional attack |        islanding |    public appeal |   severe weather |   system operability disruption |
|--------------------:|------------------------:|---------------------:|-----------------:|-----------------:|-----------------:|--------------------------------:|
|         6.52e+07    |                  7500.5 |                  285 |           931200 |             1410 |      1.78962e+08 |                      1.7325e+06 |
|         7.61e+07    |                    8468 |                 1322 |              nan |      1.47494e+06 |      3.8615e+08  |                     2.72094e+08 |
|         1.23878e+07 |                   12240 |                   92 |              881 |       5.1336e+07 |      3.26609e+08 |                     2.17858e+07 |
|         5.61669e+07 |                       1 |                  189 |           795000 |           992774 |      4.34261e+08 |                      5.5125e+06 |
|         1.05e+07    |                    1440 |                  410 |      7.13798e+06 |            473.5 |      1.9047e+08  |                       1.119e+07 |
|         2.84479e+07 |                     nan |                137.5 |              nan |             4320 |      9.8209e+07  |                     2.51766e+06 |
|         1.645e+06   |                      76 |                  117 |            70460 |             2275 |      1.86159e+08 |                     1.34727e+07 |
|         3.228e+07   |                     455 |                  464 |            33175 |              420 |      8.02369e+07 |                     2.05364e+07 |
|         6.1e+06     |                     nan |                   47 |           148400 |      1.24201e+07 |      5.6615e+06  |                             nan |

Here is the Climate Region:

![Climate Region]('pics/climate_regions.gif')

## Assessment of Missingness
### NMAR Analysis
From my point of view, I believe the column `CUSTOMERS.AFFECTED` is NMAR. The reason is if the power outage only affects
small number of people The data would be easier to collect compared to a large number of people. Thus, the missing value of
`CUSTOMERS.AFFECTED` tends to be large due to the difficulty of recording precise data from a large population.
Also, if I can get the approximate scope of the impact area, it would be easy to tell `CUSTOMERS.AFFECTED` is NMAR or MAR. Since
We could estimate the population of affected people from that.


### Missingness Dependency:
**Missingness of `CUSTOMERS.AFFECTED` does depend on `ANOMALY.LEVEL`**
We wanted to determine if `CUSTOMERS.AFFECTED` and `ANOMALY.LEVEL` were missing at random or missing completely at random.
Here is the observed distribution of `ANOMALY.LEVEL` when `CUSTOMERS.AFFECTED` was missing and not missing:
<iframe src="pics/Dist_Anomaly_Index.html" width=800 height=600 frameBorder=0></iframe>

Our observed statistic was: 0.11320987189463728

Our p-value was: 0.0068

Here is the empirical distribution of the test statistic:
<iframe src="pics/Mean_Diff_Anomaly_Index.html" width=800 height=600 frameBorder=0></iframe>

**Missingness of `CUSTOMERS.AFFECTED` does not depend on `RES.PRICE`**
We wanted to determine if `CUSTOMERS.AFFECTED` and `RES.PRICE` were Missing at Random or Missing Completely at Random.
Here is the observed distribution of `RES.PRICE` when `CUSTOMERS.AFFECTED` was missing and not missing:
<iframe src="pics/Dist_ElectricityPrice.html" width=800 height=600 frameBorder=0></iframe>

Our observed statistic was: 0.10206379649832442 
Our p-value was: 0.5622
Here is the empirical distribution of the test statistic:
<iframe src="pics/Mean_Diff_Electricity_Price.html" width=800 height=600 frameBorder=0></iframe>
Thus, `CUSTOMERS.AFFECTED` does not depend on `RES.PRICE`
---
## Hypothesis Testing
The question we are going to research is: Does the most common factor, severe weather, have a higher `Overall Effect` compared to other factors of power outages?
**Null Hypothesis:** The mean `Overall Effect` of severe weather in `CAUSE.CATEGORY` is equal to the mean `Overall Effect` with other factors in the same `CAUSE.CATEGORY`.

**Alternate Hypothesis:** The mean `Overall Effect` of severe weather in `CAUSE.CATEGORY` is greater than the mean `Overall Effect` with other factors in the same CAUSE. CATEGORY.

**Test Statistic:** Use the mean difference of `Overall Effect` with severe weather in `CAUSE.CATEGORY` and
`Overall Effect` with other factors in `CAUSE.CATEGORY`.

We chose to use the mean difference since `Overall Effect` is the numerical data.
Significance Level: 1%
p-value: 0
We did 10,000 simulations.
Our observed statistic was: 969391931.656144.
Conclusion: We reject the null hypothesis.

Since we reject the null hypothesis, we believe the mean `Overall Effect` of severe weather in `CAUSE.CATEGORY` is greater than the mean `Overall Effect` with other factors in the same CAUSE. CATEGORY.
That means we have strong evidence that severe weather has a larger effect on power outages compared to other factors that caused power outages.
