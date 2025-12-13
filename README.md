# Power Outage Analysis
Andrew DaCosta

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
