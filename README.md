# When Will the Power Return?
By Andrew DaCosta

## Introduction

This project uses a dataset containing major power outages across the continental United States. The data is extensive, containing records from January 2000 through July 2016. Only significant outages are recorded in the DataFrame, meaning that at least 50,000 customers were affected or at least 300 megawatts of power were lost. The DataFrame also contains information about the location of the outages, regional climate information, date and time for each outage, electricity consumption, as well as other features such as land use and economic data.

In this website, we will explore the duration of each outage. We will examine whether it is correlated with other features that would be known at the time of the outage and explore its predictability. This is a useful question to analyze, as it could aid in safety and well-being, as well as assist with operational and financial decisions.

The data contains 1,541 rows and 57 columns. The columns in the original dataset that will be useful in navigating our proposed topic are as follows:

| Column | Description |
|------|------------|
| YEAR | Year an outage occurred |
| MONTH | Month an outage occurred |
| U.S._STATE | State the outage occurred in |
| POSTAL.CODE | Two-letter postal abbreviation of the state |
| NERC.REGION | North American Electric Reliability Corporation (NERC) region involved in the outage event |
| CLIMATE.REGION | U.S. climate region as specified by NOAA |
| ANOMALY.LEVEL | Oceanic Niño/La Niña (ONI) index referring to climate abnormality |
| OUTAGE.START.DATE | Day of the year when the outage event started |
| OUTAGE.START.TIME | Time of the day when the outage event started |
| OUTAGE.RESTORATION.DATE | Day of the year when power was restored to all customers |
| OUTAGE.RESTORATION.TIME | Time of the day when power was restored to all customers |
| CAUSE.CATEGORY | Categories of events causing the major power outages |
| CAUSE.CATEGORY.DETAIL | Detailed description of the outage cause |
| OUTAGE.DURATION (mins) | Duration of outage events measured in minutes |
| CUSTOMERS.AFFECTED | Number of customers affected by the power outage event |
| POPULATION | Population of the U.S. state in the given year |
| PCT_WATER_TOT (%) | Percentage of the state’s total area covered by water |





## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The raw data was downloade as an Excel file with rows of headers and notes before the outage records. Since each row of interest represents a single outage event, so the first thing we did was remove everything above when the data was provided.

The dataset included a row describing the units of each column. We kept this by appending the units to the column names, such as `OUTAGE.DURATION (mins)` and `DEMAND.LOSS.MW (Megawatt)`. This will help with interpretation later.

Outage timing is important for out question, so the separate date and time columns—`OUTAGE.START.DATE`, `OUTAGE.START.TIME`, `OUTAGE.RESTORATION.DATE`, and `OUTAGE.RESTORATION.TIME`—were combined into two datetime columns: `OUTAGE_START_DT` and `OUTAGE_RESTORATION_DT` to make timing easier to work with. At this point we also removed `YEAR` and `MONTH` since they were already in out new dateime type columns.

We also made sure the types of any columns that we would be working with were in line, so `OUTAGE.DURATION (mins)`, `CUSTOMERS.AFFECTED`, `DEMAND.LOSS.MW (Megawatt)`, and `ANOMALY.LEVEL (numeric)` were typcasted to floats.

For categorical columns such as `U.S._STATE`, `POSTAL.CODE`, `NERC.REGION`, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, `CAUSE.CATEGORY`, and `CAUSE.CATEGORY.DETAIL`, values were cleaned by trimming whitespace and replacing placeholder entries like `"NA"`, `"nan"`, and empty strings with missing values.

Since our aim later on is to explore what correlates with the length of an outage, we will keep most rows around even though they might not be used.

### Head of Cleaned DataFrame

Below are the first five rows of the cleaned DataFrame:

| U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL (numeric) | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION (mins) |   DEMAND.LOSS.MW (Megawatt) |   CUSTOMERS.AFFECTED |   RES.PRICE (cents / kilowatt-hour) |   COM.PRICE (cents / kilowatt-hour) |   IND.PRICE (cents / kilowatt-hour) |   TOTAL.PRICE (cents / kilowatt-hour) |   RES.SALES (Megawatt-hour) |   COM.SALES (Megawatt-hour) |   IND.SALES (Megawatt-hour) |   TOTAL.SALES (Megawatt-hour) |   RES.PERCEN (%) |   COM.PERCEN (%) |   IND.PERCEN (%) |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT (%) |   COM.CUST.PCT (%) |   IND.CUST.PCT (%) |   PC.REALGSP.STATE (USD) |   PC.REALGSP.USA (USD) |   PC.REALGSP.REL (fraction) |   PC.REALGSP.CHANGE (%) |   UTIL.REALGSP (USD) |   TOTAL.REALGSP (USD) |   UTIL.CONTRI (%) |   PI.UTIL.OFUSA (%) |   POPULATION |   POPPCT_URBAN (%) |   POPPCT_UC (%) |   POPDEN_URBAN (persons per square mile) |   POPDEN_UC (persons per square mile) |   POPDEN_RURAL (persons per square mile) |   AREAPCT_URBAN (%) |   AREAPCT_UC (%) |   PCT_LAND (%) |   PCT_WATER_TOT (%) |   PCT_WATER_INLAND (%) | OUTAGE_START_DT     | OUTAGE_RESTORATION_DT   |
|:-------------|:--------------|:--------------|:-------------------|--------------------------:|:-------------------|:-------------------|:------------------------|------------------:|-------------------------:|----------------------------:|---------------------:|------------------------------------:|------------------------------------:|------------------------------------:|--------------------------------------:|----------------------------:|----------------------------:|----------------------------:|------------------------------:|-----------------:|-----------------:|-----------------:|----------------:|----------------:|----------------:|------------------:|-------------------:|-------------------:|-------------------:|-------------------------:|-----------------------:|----------------------------:|------------------------:|---------------------:|----------------------:|------------------:|--------------------:|-------------:|-------------------:|----------------:|-----------------------------------------:|--------------------------------------:|-----------------------------------------:|--------------------:|-----------------:|---------------:|--------------------:|-----------------------:|:--------------------|:------------------------|
| Minnesota    | MN            | MRO           | East North Central |                      -0.3 | normal             | severe weather     | <NA>                    |               nan |                     3060 |                         nan |                70000 |                               11.6  |                                9.18 |                                6.81 |                                  9.28 |                     2332915 |                     2114774 |                     2113291 |                       6562520 |          35.5491 |          32.225  |          32.2024 |         2308736 |          276286 |           10673 |           2595696 |            88.9448 |            10.644  |           0.411181 |                    51268 |                  47586 |                     1.07738 |                     1.6 |                 4802 |                274182 |           1.75139 |                 2.2 |      5348119 |              73.27 |           15.28 |                                     2279 |                                1700.5 |                                     18.2 |                2.14 |              0.6 |        91.5927 |             8.40733 |                5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00     |
| Minnesota    | MN            | MRO           | East North Central |                      -0.1 | normal             | intentional attack | vandalism               |               nan |                        1 |                         nan |                  nan |                               12.12 |                                9.71 |                                6.49 |                                  9.28 |                     1586986 |                     1807756 |                     1887927 |                       5284231 |          30.0325 |          34.2104 |          35.7276 |         2345860 |          284978 |            9898 |           2640737 |            88.8335 |            10.7916 |           0.37482  |                    53499 |                  49091 |                     1.08979 |                     1.9 |                 5226 |                291955 |           1.79    |                 2.2 |      5457125 |              73.27 |           15.28 |                                     2279 |                                1700.5 |                                     18.2 |                2.14 |              0.6 |        91.5927 |             8.40733 |                5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00     |
| Minnesota    | MN            | MRO           | East North Central |                      -1.5 | cold               | severe weather     | heavy wind              |               nan |                     3000 |                         nan |                70000 |                               10.87 |                                8.19 |                                6.07 |                                  8.15 |                     1467293 |                     1801683 |                     1951295 |                       5222116 |          28.0977 |          34.501  |          37.366  |         2300291 |          276463 |           10150 |           2586905 |            88.9206 |            10.687  |           0.392361 |                    50447 |                  47287 |                     1.06683 |                     2.7 |                 4571 |                267895 |           1.70627 |                 2.1 |      5310903 |              73.27 |           15.28 |                                     2279 |                                1700.5 |                                     18.2 |                2.14 |              0.6 |        91.5927 |             8.40733 |                5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00     |
| Minnesota    | MN            | MRO           | East North Central |                      -0.1 | normal             | severe weather     | thunderstorm            |               nan |                     2550 |                         nan |                68200 |                               11.79 |                                9.25 |                                6.71 |                                  9.19 |                     1851519 |                     1941174 |                     1993026 |                       5787064 |          31.9941 |          33.5433 |          34.4393 |         2317336 |          278466 |           11010 |           2606813 |            88.8954 |            10.6822 |           0.422355 |                    51598 |                  48156 |                     1.07148 |                     0.6 |                 5364 |                277627 |           1.93209 |                 2.2 |      5380443 |              73.27 |           15.28 |                                     2279 |                                1700.5 |                                     18.2 |                2.14 |              0.6 |        91.5927 |             8.40733 |                5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00     |
| Minnesota    | MN            | MRO           | East North Central |                       1.2 | warm               | severe weather     | <NA>                    |               nan |                     1740 |                         250 |               250000 |                               13.07 |                               10.16 |                                7.74 |                                 10.43 |                     2028875 |                     2161612 |                     1777937 |                       5970339 |          33.9826 |          36.2059 |          29.7795 |         2374674 |          289044 |            9812 |           2673531 |            88.8216 |            10.8113 |           0.367005 |                    54431 |                  49844 |                     1.09203 |                     1.7 |                 4873 |                292023 |           1.6687  |                 2.2 |      5489594 |              73.27 |           15.28 |                                     2279 |                                1700.5 |                                     18.2 |                2.14 |              0.6 |        91.5927 |             8.40733 |                5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00     |







## Assessment of Missingness
(text goes here)

## Hypothesis Testing
(text goes here)

## Framing a Prediction Problem
(text goes here)

## Baseline Model
(text goes here)

## Final Model
(text goes here)

## Fairness Analysis
(text goes here)
