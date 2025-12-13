# When Will Power Return?
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
<div style="overflow-x: auto; max-width: 100%;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>U.S._STATE</th>
      <th>POSTAL.CODE</th>
      <th>NERC.REGION</th>
      <th>CLIMATE.REGION</th>
      <th>ANOMALY.LEVEL (numeric)</th>
      <th>CLIMATE.CATEGORY</th>
      <th>CAUSE.CATEGORY</th>
      <th>CAUSE.CATEGORY.DETAIL</th>
      <th>HURRICANE.NAMES</th>
      <th>OUTAGE.DURATION (mins)</th>
      <th>DEMAND.LOSS.MW (Megawatt)</th>
      <th>CUSTOMERS.AFFECTED</th>
      <th>RES.PRICE (cents / kilowatt-hour)</th>
      <th>COM.PRICE (cents / kilowatt-hour)</th>
      <th>IND.PRICE (cents / kilowatt-hour)</th>
      <th>TOTAL.PRICE (cents / kilowatt-hour)</th>
      <th>RES.SALES (Megawatt-hour)</th>
      <th>COM.SALES (Megawatt-hour)</th>
      <th>IND.SALES (Megawatt-hour)</th>
      <th>TOTAL.SALES (Megawatt-hour)</th>
      <th>RES.PERCEN (%)</th>
      <th>COM.PERCEN (%)</th>
      <th>IND.PERCEN (%)</th>
      <th>RES.CUSTOMERS</th>
      <th>COM.CUSTOMERS</th>
      <th>IND.CUSTOMERS</th>
      <th>TOTAL.CUSTOMERS</th>
      <th>RES.CUST.PCT (%)</th>
      <th>COM.CUST.PCT (%)</th>
      <th>IND.CUST.PCT (%)</th>
      <th>PC.REALGSP.STATE (USD)</th>
      <th>PC.REALGSP.USA (USD)</th>
      <th>PC.REALGSP.REL (fraction)</th>
      <th>PC.REALGSP.CHANGE (%)</th>
      <th>UTIL.REALGSP (USD)</th>
      <th>TOTAL.REALGSP (USD)</th>
      <th>UTIL.CONTRI (%)</th>
      <th>PI.UTIL.OFUSA (%)</th>
      <th>POPULATION</th>
      <th>POPPCT_URBAN (%)</th>
      <th>POPPCT_UC (%)</th>
      <th>POPDEN_URBAN (persons per square mile)</th>
      <th>POPDEN_UC (persons per square mile)</th>
      <th>POPDEN_RURAL (persons per square mile)</th>
      <th>AREAPCT_URBAN (%)</th>
      <th>AREAPCT_UC (%)</th>
      <th>PCT_LAND (%)</th>
      <th>PCT_WATER_TOT (%)</th>
      <th>PCT_WATER_INLAND (%)</th>
      <th>OUTAGE_START_DT</th>
      <th>OUTAGE_RESTORATION_DT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.3</td>
      <td>normal</td>
      <td>severe weather</td>
      <td>&lt;NA&gt;</td>
      <td>NaN</td>
      <td>3060.0</td>
      <td>NaN</td>
      <td>70000.0</td>
      <td>11.6</td>
      <td>9.18</td>
      <td>6.81</td>
      <td>9.28</td>
      <td>2332915</td>
      <td>2114774</td>
      <td>2113291</td>
      <td>6562520</td>
      <td>35.549073</td>
      <td>32.225029</td>
      <td>32.202431</td>
      <td>2308736</td>
      <td>276286</td>
      <td>10673</td>
      <td>2595696</td>
      <td>88.944776</td>
      <td>10.644005</td>
      <td>0.411181</td>
      <td>51268</td>
      <td>47586</td>
      <td>1.077376</td>
      <td>1.6</td>
      <td>4802</td>
      <td>274182</td>
      <td>1.751391</td>
      <td>2.2</td>
      <td>5348119</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.592666</td>
      <td>8.407334</td>
      <td>5.478743</td>
      <td>2011-07-01 17:00:00</td>
      <td>2011-07-03 20:00:00</td>
    </tr>
    <tr>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>intentional attack</td>
      <td>vandalism</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.12</td>
      <td>9.71</td>
      <td>6.49</td>
      <td>9.28</td>
      <td>1586986</td>
      <td>1807756</td>
      <td>1887927</td>
      <td>5284231</td>
      <td>30.032487</td>
      <td>34.210389</td>
      <td>35.727564</td>
      <td>2345860</td>
      <td>284978</td>
      <td>9898</td>
      <td>2640737</td>
      <td>88.833534</td>
      <td>10.791609</td>
      <td>0.37482</td>
      <td>53499</td>
      <td>49091</td>
      <td>1.089792</td>
      <td>1.9</td>
      <td>5226</td>
      <td>291955</td>
      <td>1.790002</td>
      <td>2.2</td>
      <td>5457125</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.592666</td>
      <td>8.407334</td>
      <td>5.478743</td>
      <td>2014-05-11 18:38:00</td>
      <td>2014-05-11 18:39:00</td>
    </tr>
    <tr>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-1.5</td>
      <td>cold</td>
      <td>severe weather</td>
      <td>heavy wind</td>
      <td>NaN</td>
      <td>3000.0</td>
      <td>NaN</td>
      <td>70000.0</td>
      <td>10.87</td>
      <td>8.19</td>
      <td>6.07</td>
      <td>8.15</td>
      <td>1467293</td>
      <td>1801683</td>
      <td>1951295</td>
      <td>5222116</td>
      <td>28.097672</td>
      <td>34.501015</td>
      <td>37.365983</td>
      <td>2300291</td>
      <td>276463</td>
      <td>10150</td>
      <td>2586905</td>
      <td>88.920583</td>
      <td>10.687018</td>
      <td>0.392361</td>
      <td>50447</td>
      <td>47287</td>
      <td>1.066826</td>
      <td>2.7</td>
      <td>4571</td>
      <td>267895</td>
      <td>1.706266</td>
      <td>2.1</td>
      <td>5310903</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.592666</td>
      <td>8.407334</td>
      <td>5.478743</td>
      <td>2010-10-26 20:00:00</td>
      <td>2010-10-28 22:00:00</td>
    </tr>
    <tr>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>severe weather</td>
      <td>thunderstorm</td>
      <td>NaN</td>
      <td>2550.0</td>
      <td>NaN</td>
      <td>68200.0</td>
      <td>11.79</td>
      <td>9.25</td>
      <td>6.71</td>
      <td>9.19</td>
      <td>1851519</td>
      <td>1941174</td>
      <td>1993026</td>
      <td>5787064</td>
      <td>31.994099</td>
      <td>33.54333</td>
      <td>34.439329</td>
      <td>2317336</td>
      <td>278466</td>
      <td>11010</td>
      <td>2606813</td>
      <td>88.895368</td>
      <td>10.682239</td>
      <td>0.422355</td>
      <td>51598</td>
      <td>48156</td>
      <td>1.071476</td>
      <td>0.6</td>
      <td>5364</td>
      <td>277627</td>
      <td>1.932089</td>
      <td>2.2</td>
      <td>5380443</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.592666</td>
      <td>8.407334</td>
      <td>5.478743</td>
      <td>2012-06-19 04:30:00</td>
      <td>2012-06-20 23:00:00</td>
    </tr>
    <tr>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>1.2</td>
      <td>warm</td>
      <td>severe weather</td>
      <td>&lt;NA&gt;</td>
      <td>NaN</td>
      <td>1740.0</td>
      <td>250.0</td>
      <td>250000.0</td>
      <td>13.07</td>
      <td>10.16</td>
      <td>7.74</td>
      <td>10.43</td>
      <td>2028875</td>
      <td>2161612</td>
      <td>1777937</td>
      <td>5970339</td>
      <td>33.982576</td>
      <td>36.20585</td>
      <td>29.779498</td>
      <td>2374674</td>
      <td>289044</td>
      <td>9812</td>
      <td>2673531</td>
      <td>88.821637</td>
      <td>10.81132</td>
      <td>0.367005</td>
      <td>54431</td>
      <td>49844</td>
      <td>1.092027</td>
      <td>1.7</td>
      <td>4873</td>
      <td>292023</td>
      <td>1.668704</td>
      <td>2.2</td>
      <td>5489594</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.592666</td>
      <td>8.407334</td>
      <td>5.478743</td>
      <td>2015-07-18 02:00:00</td>
      <td>2015-07-19 07:00:00</td>
    </tr>
  </tbody>
</table>
</div>


### Univariate Analysis

<iframe
  src="assets/outages_by_climate_region.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>
Outages are not evenly distributed across regions, with the Northeast, South, and West have the highest counts. This could mean that regional factors such as climate patterns, infrastructure, etc likely are associated with the frequemcy of outages. This could for example hint that some regions have weaker infrustructer that is perhaps more dated, or the location could get more extreme weather which could cause an outage more often then elsewhere.

### Bivariate Analysis

<iframe
  src="assets/outage_duration_by_cause.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

This plot compares outage duration across different cause categories. While most outage durations are relatively short, certain causes show much larger variability and longer extreme durations, showing that the underlying cause of an outage is related to how long it takes to restore power in some cases.




## Assessment of Missingness

### NMAR Analysis

It is possible that the column CAUSE.CATEGORY.DETAIL is NMAR. This column records a more specific description of the outage cause, and its missingness likely depends on the detail itself. In particular, outages with vague of minof causes are less likely to have detailed descriptions recorded and might be omitted entirely, while more severe or common events are more likely to include them. Because the probability of missingness depends on the unobserved value of CAUSE.CATEGORY.DETAIL, the missingness mechanism is at least partly not missing at random.

It would be helpful if we could attain infomration about reporting standard and whether or not they are required for certain outage types or in certain states. Then if there was a correlation between this and the missingness it would be MAR. We go on to test this.

### Missingness Dependency

We tested whether the missingness of CAUSE.CATEGORY.DETAIL depends on other observed variables via permutation tests. For categorical variables, we looked at the distributions of categories when the column is missing versus not missing using total variation distance (TVD). For numerical variables we compared the difference in means between the missing and non-missing groups.

We find strong evidence that missingness depends on CAUSE.CATEGORY, U.S._STATE, and POPULATION since all three tests yield p-values near zero. On the ohter hand there is no statistically significant evidence that missingness depends on PCT_WATER_TOT (%). This suggests that whether cause details are missing is related to outage context and reporting practices likley in each state. As expected it has nothing to do with percentage of water area in that state.

<iframe
  src="assets/missingness_outage_duration.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

This plot compares the distribution of outage durations when `CAUSE.CATEGORY.DETAIL` is missing versus when it is present. Outages with missing cause details tend to be shorter hint at the fact detailed cause information is less likely to be recorded for shorter or less severe outages.


## Hypothesis Testing

Null Hypothesis:
Outage duration has the same distribution across all climate regions.

Alternative Hypothesis:
At least one climate region has a different outage duration distribution.

Test Statistic:
The difference between the maximum and minimum median outage duration across climate regions. The median is used since outage duration is highly skewed.

Significance Level:
α = 0.05.

Method:
We use a permutation test by shuffling climate region labels to approximate the null distribution. This tests whether climate region is associated with outage duration.

Results and Conclusion:
The observed statistic is 3060.5 minutes with a p-value of 0.0015. Since the p-value is below 0.05, we reject the null hypothesis in favor of the alternate. This suggests that outage duration differs across climate regions, though this result reflects association only.

## Framing a Prediction Problem

Our prediction task is a regression problem, where the aim is to predict the duration of a power outage (`OUTAGE.DURATION (mins)`). Naturally this will be our target to bring us toward the goal stated in the introductoion and furthermore we will have a lot of features available at the time of prediction.

We evaluate model performance using mean absolute error (MAE) mainly although we also consier RSME and R^2. MAE is appropriate here because it is easy to interpret in the same units as the response variable and is less sensitive to outliers than mean squared error, which is important given the distribution of duration is very skewed.

## Baseline Model
(text goes here)

## Final Model
(text goes here)

## Fairness Analysis
(text goes here)
