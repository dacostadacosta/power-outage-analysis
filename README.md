# Power Outage Analysis
Andrew DaCosta

## Introduction
This project uses a dataset containing major power outages across the continental United States. The data is extensive, containing records from January 2000 through July 2016. Only significant outages are recorded in the DataFrame, meaning that at least 50,000 customers were affected or at least 300 megawatts of power were lost. The DataFrame also contains information about the location of the outages, regional climate information, date and time for each outage, electricity consumption, as well as other features such as land use and economic data.

In this website, we will explore the duration of each outage. We will examine whether it is correlated with other features that would be known at the time of the outage and explore its predictability. This is a useful question to analyze, as it could aid in safety and well-being, as well as assist with operational and financial decisions.

The data contains 1,541 rows and 57 columns. The columns in the original dataset that will be useful in navigating our proposed topic are as follows:

**Outcome**
- OUTAGE.DURATION (mins): Total length of the power outage measured in minutes.

**Time & Derived Time**
- OUTAGE.START.DATE: Calendar date when the outage began.
- OUTAGE.START.TIME: Time of day when the outage began.
- OUTAGE.RESTORATION.DATE: Calendar date when power was fully restored.
- OUTAGE.RESTORATION.TIME: Time of day when power was fully restored.
- MONTH: Month of the year in which the outage started.
- YEAR: Year in which the outage occurred.

**Geography & Regions**
- U.S._STATE: U.S. state where the outage took place.
- POSTAL.CODE: Two-letter postal abbreviation for the state.
- NERC.REGION: Power grid reliability region defined by NERC.
- CLIMATE.REGION: NOAA-defined climate region of the state.

**Climate**
- ANOMALY.LEVEL (numeric): Oceanic Niño Index value measuring climate abnormality.
- CLIMATE.CATEGORY: Broad climate regime (Warm, Cold, or Normal).

**Outage Cause & Severity**
- CAUSE.CATEGORY: High-level classification of the outage cause.
- CAUSE.CATEGORY.DETAIL: More detailed description of the specific outage cause.
- CUSTOMERS.AFFECTED: Number of customers impacted by the outage.

**Population & Land Use**
- POPULATION: Total population of the state in the given year.
- PCT_WATER_TOT (%): Percentage of the state’s total area covered by water.




## Data Cleaning and Exploratory Data Analysis
(text goes here)

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
