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

### Interesting Aggregates

<div style="overflow-x: auto; max-width: 100%;">
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>NERC.REGION</th>
      <th colspan="3" halign="left">OUTAGE.DURATION (mins)</th>
      <th colspan="3" halign="left">CUSTOMERS.AFFECTED</th>
    </tr>
    <tr>
      <th></th>
      <th>count</th>
      <th>median</th>
      <th>mean</th>
      <th>count</th>
      <th>median</th>
      <th>mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>ECAR</td>
      <td>32</td>
      <td>5475.0</td>
      <td>5603.312500</td>
      <td>32</td>
      <td>142500.0</td>
      <td>256354.187500</td>
    </tr>
    <tr>
      <td>FRCC</td>
      <td>43</td>
      <td>1419.0</td>
      <td>4271.116279</td>
      <td>44</td>
      <td>51982.5</td>
      <td>289778.181818</td>
    </tr>
    <tr>
      <td>RFC</td>
      <td>416</td>
      <td>1672.0</td>
      <td>3477.956731</td>
      <td>349</td>
      <td>95000.0</td>
      <td>127894.232092</td>
    </tr>
    <tr>
      <td>NPCC</td>
      <td>147</td>
      <td>494.0</td>
      <td>3262.170068</td>
      <td>108</td>
      <td>50371.0</td>
      <td>108726.037037</td>
    </tr>
    <tr>
      <td>TRE</td>
      <td>108</td>
      <td>1095.0</td>
      <td>2960.574074</td>
      <td>81</td>
      <td>96000.0</td>
      <td>226468.654321</td>
    </tr>
    <tr>
      <td>MRO</td>
      <td>44</td>
      <td>1245.5</td>
      <td>2933.590909</td>
      <td>29</td>
      <td>63000.0</td>
      <td>88984.965517</td>
    </tr>
    <tr>
      <td>SPP</td>
      <td>62</td>
      <td>1134.0</td>
      <td>2693.774194</td>
      <td>43</td>
      <td>57531.0</td>
      <td>188513.000000</td>
    </tr>
    <tr>
      <td>SERC</td>
      <td>194</td>
      <td>747.0</td>
      <td>1737.989691</td>
      <td>154</td>
      <td>70567.5</td>
      <td>107854.038961</td>
    </tr>
    <tr>
      <td>WECC</td>
      <td>424</td>
      <td>219.5</td>
      <td>1481.490566</td>
      <td>245</td>
      <td>32000.0</td>
      <td>133833.065306</td>
    </tr>
    <tr>
      <td>HI</td>
      <td>1</td>
      <td>1367.0</td>
      <td>1367.000000</td>
      <td>1</td>
      <td>294000.0</td>
      <td>294000.000000</td>
    </tr>
    <tr>
      <td>HECO</td>
      <td>3</td>
      <td>543.0</td>
      <td>895.333333</td>
      <td>3</td>
      <td>59886.0</td>
      <td>126728.666667</td>
    </tr>
    <tr>
      <td>FRCC, SERC</td>
      <td>1</td>
      <td>372.0</td>
      <td>372.000000</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>PR</td>
      <td>1</td>
      <td>174.0</td>
      <td>174.000000</td>
      <td>1</td>
      <td>62000.0</td>
      <td>62000.000000</td>
    </tr>
    <tr>
      <td>ASCC</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>14273.0</td>
      <td>14273.000000</td>
    </tr>
  </tbody>
</table>
</div>

This table summarizes outage duration and customer impact across NERC regions. It shows clear differences in the severity of outages across regions, which tells us how grid structure and regional characteristics are related to outage outcomes.



## Assessment of Missingness

### NMAR Analysis

It is possible that the column `CAUSE.CATEGORY.DETAIL` is NMAR. This column records a more specific description of the outage cause, and its missingness likely depends on the detail itself. In particular, outages with vague or minor causes are less likely to have detailed descriptions recorded and might be omitted entirely, while more severe or common events are more likely to include them. Because the probability of missingness depends on the unobserved value of `CAUSE.CATEGORY.DETAIL`, the missingness mechanism is at least partly not missing at random.

It would be helpful if we could attain information about reporting standards and whether or not detailed cause descriptions are required for certain outage types or in certain states. Then, if there was a correlation between this information and the missingness, the mechanism could be considered MAR. We go on to test this.

### Missingness Dependency

We tested whether the missingness of `CAUSE.CATEGORY.DETAIL` depends on other observed variables via permutation tests. For categorical variables, we looked at the distributions of categories when the column is missing versus not missing using total variation distance (TVD). For numerical variables, we compared the difference in means between the missing and non-missing groups.

We find strong evidence that missingness depends on `CAUSE.CATEGORY`, `U.S._STATE`, and `POPULATION`, since all three tests yield p-values near zero. On the other hand, there is no statistically significant evidence that missingness depends on `PCT_WATER_TOT (%)`. This suggests that whether cause details are missing is related to outage context and reporting practices, likely varying by state. As expected, it has nothing to do with the percentage of water area in that state.

<iframe
  src="assets/missingness_outage_duration.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

This plot compares the distribution of outage durations when `CAUSE.CATEGORY.DETAIL` is missing versus when it is present. Outages with missing cause details tend to be shorter, hinting at the fact that detailed cause information is less likely to be recorded for shorter or less severe outages.



## Hypothesis Testing

**Null Hypothesis:** Outage duration has the same distribution across all climate regions.

**Alternative Hypothesis:** At least one climate region has a different outage duration distribution.

**Test Statistic:** The difference between the maximum and minimum median outage duration across climate regions. The median is used since outage duration is highly skewed.

**Significance Level:** α = 0.05.

**Method:** We use a permutation test by shuffling `CLIMATE.REGION` labels to approximate the null distribution. This tests whether climate region is associated with `OUTAGE.DURATION (mins)`.

**Results and Conclusion:** The observed statistic is 3060.5 minutes with a p-value of 0.0015. Since the p-value is below 0.05, we reject the null hypothesis in favor of the alternative. This suggests that outage duration differs across climate regions, though this result reflects association only.

## Framing a Prediction Problem

Our prediction task is a regression problem, where the aim is to predict the duration of a power outage (`OUTAGE.DURATION (mins)`). Naturally, this will be our target to bring us toward the goal stated in the introduction, and furthermore we will have a lot of features available at the time of prediction.

We evaluate model performance using mean absolute error (MAE) mainly, although we also consider RMSE and R². MAE is appropriate here because it is easy to interpret in the same units as the response variable and is less sensitive to outliers than mean squared error, which is important given that the distribution of outage duration is very skewed.

## Baseline Model

We begin with a linear regression baseline model to predict outage duration (`OUTAGE.DURATION (mins)`) using two features.

**Features used:** `CLIMATE.REGION`, which is a nominal categorical feature, and `MONTH`, which is a quantitative numeric feature.

**Encoding:** `CLIMATE.REGION` is encoded using one-hot encoding to convert categories into binary indicator variables. `MONTH` is passed through unchanged as a numeric feature in case there is some sort of seasonal trend that might appear, for example outages increasing throughout the year. Transformations are applied using a `ColumnTransformer`.

**Data preprocessing decisions:** To reduce the influence of extreme outliers, the top 2% of outage durations were trimmed before training. There were 2–3 extremely long outage durations; given their rarity, our model would not generalize well to predict these outliers, and they would also skew the model.

**Performance (test set):**  
- RMSE: 3134.4 minutes  
- MAE: 2041.1 minutes  
- R²: 0.035

**Evaluation:** This model is not good in terms of prediction. It explains very little variance in outage duration and has large prediction errors. This is as expected, since the only feature really adding some predictive power is the region; however, within each region the variance of outage duration is high. Hence, we need to add more features to try and lower our errors and improve correlation.



## Final Model

We added 5 new features to the model:

- **`U.S._STATE`** captures state-level differences in grid infrastructure, emergency response, or other state-level influences that can affect restoration time. It is a categorical variable and we use one-hot encoding so the model can learn separate effects for each state.

- **`CLIMATE.CATEGORY`** represents broad weather regimes that influence the types of events occurring at the outage, which may be related to response time as well as to the cause of the outage itself. This variable is used as a categorical feature and one-hot encoded.

- **`CAUSE.CATEGORY`** reflects the primary reason for an outage. It is natural to assume that different causes might lead to different durations. For example, an outage caused by a simple computer glitch is likely going to be shorter than one caused by a hurricane or natural disaster. It is included as a categorical feature and one-hot encoded.

- **`ANOMALY.LEVEL (numeric)`** was transformed using its absolute value to capture the severity of abnormal climate conditions regardless of direction. This might help our model by adding more context to the climate, since anything particularly abnormal for a region might cause more damage or slower response given its unexpectedness.

- **`HOUR_TYPE` (working vs. off hours)** was added to the DataFrame to capture operational constraints, as outages starting outside standard working hours may have delayed responses. This feature was engineered by extracting the hour from the outage start time and binning it into working hours (8–18) versus off hours, then treated as a categorical variable.

Our final model is a **Random Forest Regressor**, selected to capture the non-linear relationships that are present in the data.

**Hyperparameter selection:**  
Hyperparameters were chosen using grid search with 5-fold cross-validation, optimizing RMSE. The best-performing model used:
- `n_estimators = 100`
- `max_depth = 10`
- `min_samples_leaf = 5`

Interestingly, these settings limit tree depth and enforce larger leaf sizes.

**Performance (test set):**
- RMSE: 2666.6 minutes  
- MAE: 1592.3 minutes  
- R²: 0.302  

Whereas the baseline linear regression model had a test MAE of approximately 2042 minutes and an R² of 0.035, the final model offers a large improvement. However, we are still a long way from accurately predicting outage duration, a task with a lot of nuance since it depends on many variables both in and out of the dataset.


## Fairness Analysis

Group X and Group Y:

Group X: Outages that start during working hours

Group Y: Outages that start during off hours

Evaluation Metric:
We use mean absolute error (MAE), since it measures prediction error in minutes and is consistent with how model performance was evaluated.

Null Hypothesis:
The model is fair. Its error (MAE) is the same for outages that start during working hours and off hours, and any observed difference is due to randomness.

Alternative Hypothesis:
The model is unfair. Its error (MAE) is higher for off-hour outages than for working-hour outages.

Test Statistic:
The difference in MAE between off-hour outages and working-hour outages.

Significance Level:
We use significance level α = 0.05.

Method:
We perform a permutation test by keeping the trained model fixed, shuffling the HOUR_TYPE labels, recomputing the MAE difference each time, and comparing the observed difference to the resulting null distribution.

Results and Conclusion:
The observed difference in MAE between off-hour and working-hour outages is approximately 9.6 minutes, with a p-value of 0.476. Since this p-value is well above the significance level, we fail to reject the null hypothesis. There is no statistical evidence that the model performs worse for outages occurring during off hours.

<iframe
  src="assets/fairness_permutation_test.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

This plot shows the null distribution of the MAE difference under random assignment of hour type, and the observed difference falls well within this distribution, suggesting no meaningful performance difference between working-hour and off-hour outages.
