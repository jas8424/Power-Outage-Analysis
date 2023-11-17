# Power-Outage-Analysis

---

## Introduction

Power outages, often unforeseen disruptions in electrical supply, underscore the significance of electricity in modern
life. They expose our dependency on power for daily routines, from lighting our homes to sustaining essential services.
Beyond inconvenience, outages highlight vulnerabilities in critical infrastructure, impacting healthcare and communication.
and public safety. Understanding the causes is crucial for resilience and preparedness.

In this project, I will find all possible characteristics of major power outages with higher severity and factors of
outage duration.
There are 1534 rows and 55 columns in the original dataset. It contained the major outages witnessed by different states 
in the continental U.S. during January 2000–July 2016.

| Column             | Description                                                                          |
|--------------------|--------------------------------------------------------------------------------------|
| `YEAR`             | Indicates the year when the outage event occurred                                    |
| `MONTH`            | Indicates the month when the outage event occurred                                   |
| `POSTAL.CODE`      | Represents the postal code of the U.S. states                                        |
| `CLIMATE.REGION`   | U.S. Climate regions as specified by National Centers for Environmental Information  |
| `CAUSE.CATEGORY`   | Categories of all the events causing the major power outages                         |
| `OUTAGE.DURATION`  | Duration of outage events (in minutes)                                               |
| `CUSTOMERS.AFFECTED` | Number of customers affected by the power outage event                               |
|`Overall Effect`| `OUTAGE.DURATION` times `CUSTOMERS.AFFECTED` for measurement of how severe the power outage is.|

## Data Cleaning and Exploratory Data Analysis
First, I select all the columns that relate to my research which is shown above.
Then since `OUTAGE.DURATION` is one of key measurement for the affect of power outage on residents, I desiced to fill
the nan values and keep only valid rows.
For the rows that contain `OUTAGE.DURATION` is nan, if the `MONTH` value is also nan, we assume the power outage of the row is not
servere since otherwise the month it happened should be recored even people do not know the `OUTAGE.DURATION`.
Then, I fill all the nan value in `OUTAGE.DURATION` by take median of other power outage reacord of same year, month, cause and state
which is reasonable if they share these categroys, the duration time should be same. Do the same process to
nan value of `CUSTOMERS.AFFECTED`.
Next, creat a column `Overall Effect` which is defined by `OUTAGE.DURATION` times `CUSTOMERS.AFFECTED` for measurement of 
how severe the power outage is.

| MONTH   | POSTAL.CODE  | CAUSE.CATEGORY|	CLIMATE.REGION|	CUSTOMERS.AFFECTED|	OUTAGE.DURATION| Overall Effect  |
|-------- |--------------|---------------|----------------|-------------------|----------------|-----------------|
|7.0	|MN	|severe weather|	East North Central|	70000.0|	3060.0|	214200000.0|
|5.0	|MN	|intentional attack|East North Central|	2970.5|	1.0|	2970.5|
|10.0	|MN|	severe weather|	East North Central|	70000.0|	3000.0|	210000000.0|
|6.0	|MN|	severe weather|	East North Central|	68200.0|	2550.0|	173910000.0|
|7.0	|MN|	severe weather|	East North Central|	250000.0|	1740.0|	435000000.0|


<iframe src="Pie of Cause Category.html" width=800 height=600 frameBorder=0></iframe>

```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```
---