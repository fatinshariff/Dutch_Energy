# Energy Consumption in Netherlands 


![elec.jpg](elec.jpg)
                                                                                  
                                                                                    Image from www.shutterstock.com

## Introduction to Dataset

From the [dataset page](https://www.kaggle.com/lucabasa/dutch-energy):

> **Enexis, Liander, and Stedin** are the three major network administrators of the Netherlands and, together, they provide energy to nearly the entire country. Every year, they release on their websites a table with the energy consumption of the areas under their administration.

> The data are anonymized by aggregating the Zipcodes so that every entry describes at least 10 connections.

>This market is not competitive, meaning that the zones are assigned. This means that every year they roughly provide energy to the same zipcodes. Small changes can happen from year to year either for a change of management or for a different aggregation of zipcodes.

>Every file contains information about groups of zipcodes managed by one of the three companies for a specific year.

There is no particular reason why I chose this dataset. I wanted to improve my analysing skills and find that this dataset is suitable for my little project as I have never worked with multiple csv files which this dataset came with. In addition, I wanted to broaden my knowledge by analysing different dataset each time and in this case, it is about energy.

note: I used Jupyter Notebook for this whole analysis process. This is just the report of the analysis and step by step of the analysis process is saved in another notebook.([click here](https://github.com/fatinshariff/Dutch_Energy/blob/master/dutch_energy.ipynb))


## Gathering data

All the libraries and magic function use in this analysis are as listed below:

* pandas
* matplotlib.pyplot
* seaborn
* matplotlib.ticker
* %matplotlib inline

The files provided are in csv form and I merged all the files into two different dataset. One that represents electricity data;named as `eltr_df` and another one as `gas_df` that represent the gas data and both datasets span from 2010 till 2019.

Electricity dataset contains 2901043 entries with 15 columns while Gas dataset contains 2901043 entries with 15 columns. 

The overview of `eltr_df` and `gas_df` dataframe looks like below respectively when loaded.

**`eltr_df`**


```python
#overview of the electricity data
eltr_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>net_manager</th>
      <th>purchase_area</th>
      <th>street</th>
      <th>zipcode_from</th>
      <th>zipcode_to</th>
      <th>city</th>
      <th>delivery_perc</th>
      <th>num_connections</th>
      <th>perc_of_active_connections</th>
      <th>type_conn_perc</th>
      <th>type_of_connection</th>
      <th>annual_consume</th>
      <th>annual_consume_lowtarif_perc</th>
      <th>smartmeter_perc</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Sasdijk</td>
      <td>4251AB</td>
      <td>4251AB</td>
      <td>WERKENDAM</td>
      <td>100.0</td>
      <td>16.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4282.0</td>
      <td>25.0</td>
      <td>0.0</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>1</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Sasdijk</td>
      <td>4251AC</td>
      <td>4251AC</td>
      <td>WERKENDAM</td>
      <td>100.0</td>
      <td>11.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5113.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>2</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Sasdijk</td>
      <td>4251AD</td>
      <td>4251AD</td>
      <td>WERKENDAM</td>
      <td>100.0</td>
      <td>30.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4809.0</td>
      <td>34.0</td>
      <td>0.0</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>3</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Nieuweweg</td>
      <td>4251AE</td>
      <td>4251AG</td>
      <td>WERKENDAM</td>
      <td>100.0</td>
      <td>21.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5015.0</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>4</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Koppenhof</td>
      <td>4251AH</td>
      <td>4251AH</td>
      <td>WERKENDAM</td>
      <td>100.0</td>
      <td>12.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3074.0</td>
      <td>22.0</td>
      <td>0.0</td>
      <td>2010</td>
    </tr>
  </tbody>
</table>
</div>



**`gas_df`**


```python
#overview of the gas data
gas_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>net_manager</th>
      <th>purchase_area</th>
      <th>street</th>
      <th>zipcode_from</th>
      <th>zipcode_to</th>
      <th>city</th>
      <th>delivery_perc</th>
      <th>num_connections</th>
      <th>perc_of_active_connections</th>
      <th>type_conn_perc</th>
      <th>type_of_connection</th>
      <th>annual_consume</th>
      <th>annual_consume_lowtarif_perc</th>
      <th>smartmeter_perc</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Antwerpsestraat</td>
      <td>4611AC</td>
      <td>4611AD</td>
      <td>BERGEN OP ZOOM</td>
      <td>100.0</td>
      <td>17.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>496.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>1</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Antwerpsestraat</td>
      <td>4611AE</td>
      <td>4611AE</td>
      <td>BERGEN OP ZOOM</td>
      <td>100.0</td>
      <td>11.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>355.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>2</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Antwerpsestraat</td>
      <td>4611AG</td>
      <td>4611AJ</td>
      <td>BERGEN OP ZOOM</td>
      <td>100.0</td>
      <td>24.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3074.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>3</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Antwerpsestraat</td>
      <td>4611AK</td>
      <td>4611AK</td>
      <td>BERGEN OP ZOOM</td>
      <td>100.0</td>
      <td>14.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>13456.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>4</th>
      <td>enexis</td>
      <td>ENEXIS</td>
      <td>Zuid - Oostsingel</td>
      <td>4611AL</td>
      <td>4611BA</td>
      <td>BERGEN OP ZOOM</td>
      <td>100.0</td>
      <td>23.0</td>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10096.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>2010</td>
    </tr>
  </tbody>
</table>
</div>



## Assessing Data

There data are tidy and there are no duplicates found in both `eltr_df` and `gas_df` dataframes but there are missing values exist. The count for missing value for each column in both dataframe are as below:


```python
#finding the null values for columns in both dataframe
eltr_df.isna().sum(),gas_df.isna().sum()
```




    (net_manager                          0
     purchase_area                   813492
     street                               0
     zipcode_from                         0
     zipcode_to                           0
     city                                 0
     delivery_perc                      582
     num_connections                      0
     perc_of_active_connections           0
     type_conn_perc                  107512
     type_of_connection              107581
     annual_consume                       0
     annual_consume_lowtarif_perc         0
     smartmeter_perc                      0
     year                                 0
     dtype: int64, net_manager                          0
     purchase_area                   235935
     street                               0
     zipcode_from                         0
     zipcode_to                           0
     city                                 0
     delivery_perc                        0
     num_connections                      0
     perc_of_active_connections           0
     type_conn_perc                   82966
     type_of_connection               82966
     annual_consume                       0
     annual_consume_lowtarif_perc         0
     smartmeter_perc                 950202
     year                                 0
     dtype: int64)



Since the total of missing values are quite large, I leave them as they are for now because I am not going to use all the variables for this Exploratory Data Analysis(EDA). 

## Data Analysis and Visualisation

My approach for analysing this dataset is by answering a few questions that I think would be interesting based on this dataset. They are as below:

1. How are all three network administrators total connections for each year from 2010-2019?
2. How is the total consumption of Electricity (kWh) and Gas (m3) across the years?
3. What are the trend of the dutch people using renewable energy?
4. Which City has high in electricity consumption? 

### 1. How are all three network administrators total connections for each year from 2010-2019?


```python
#import library for replacing ticker
from matplotlib.ticker import FuncFormatter

#function to replace value of old tick form to new
def millions(x, pos):
    'The two args are the value and tick position'
    return '{}M'.format(x * 1e-6)

formatter = FuncFormatter(millions)

fig=plt.figure(figsize=[7,8])
plt.subplots_adjust(hspace=0.3)

plt.subplot(2,1,1)
ax1 = sns.barplot(data= na_el_conn,x='year', y= 'num_connections',hue='net_manager',alpha=0.9)
ax1.legend(bbox_to_anchor=(1.05, 1), borderaxespad=0)
ax1.yaxis.set_major_formatter(formatter)
plt.title('Electricity Total Connections for each year from 2010-2019')
plt.xlabel('')

plt.subplot(2,1,2)
ax2 = sns.barplot(data= na_gas_conn,x='year', y= 'num_connections',hue='net_manager',alpha=0.9)
ax2.yaxis.set_major_formatter(formatter)
ax2.legend(bbox_to_anchor=(1.05, 1), borderaxespad=0)
plt.title('Gas Total Connections for each year from 2010-2019')

plt.show()
```


    
![png](output_15_0.png)
    


Liander have the highest total connections for each year and the total connections increases for Liander and Enexis throughout the decade. Stedin on the other hand doesn't show any significant improvement for the electricity connections throughout the years. I assume the data for 2019 is not yet complete. Hence, I focus on the data up until 2018.

### 2. How is the total consumption of Electricity (kWh) and Gas (m3) across the years?

To see the consumption for both electricity and gas for each year, I focus on 2 parts. Firstly I calculate the total consumption for each year for each dataset. The next part I calculate the mean of consumption for each connection. I did this because I wanted to see whether they are correlated to each other or not and they are plotted as below.



```python
x1 = [2010,2011,2012,2013,2014,2015,2016,2017,2018,2019]
#x2 is the same as x1, so they will share x axis

y1 = eltr_df.groupby('year').annual_consume.sum()
y2 = gas_df.groupby('year').annual_consume.sum()
y3 = eltr_df.groupby('year').cons_per_conn.mean()
y4 = gas_df.groupby('year').cons_per_conn.mean()

plt.figure(figsize=(15,6))

ax1=plt.subplot(2, 2, 1)
plt.plot(x1, y1, 'o-')
plt.title('Yearly Total Consumption of Electricity (kWh) and Gas (m3)')
plt.ylabel('Electricity in kWh')
#change the visibility of the xtick to not visible
plt.setp(ax1.get_xticklabels(), visible=False)

ax2=plt.subplot(2, 2, 3,sharex=ax1)
plt.plot(y2,'o-')
plt.xlabel('year')
plt.ylabel('Gas in m3')

ax3=plt.subplot(2, 2, 2)
plt.plot(x1, y3, 'o-')
plt.setp(ax3.get_xticklabels(), visible=False)
plt.title('Yearly Mean Consumption of Electricity (kWh) and Gas (m3)')

ax4=plt.subplot(2, 2, 4,sharex=ax1)
plt.plot(y4, 'o-')
plt.xlabel('year')

plt.show()
```


    
![png](output_19_0.png)
    


It is interesting to see that the trend of the total energy and gas consumption increases from 2010 to 2012 then continuously drop to the lowest level in 2016 for electricity and 2017 for the gas (left plots). After that the trend increases back and year 2018 marked as the highest consumption in the past eight years for gas. The increase of gas price since 2010 could be the reason of reduction of the demand of gas energy and the demand show declination since 2012 until it reach the lowest in 2016. Weather is also a price indicator for gas because weather can alter the way people use gas which could cause the changes in the total energy consumption.

Meanwhile, the plots on the right show the mean consumption of both electricity and gas. This plot however show that the overal trend of the consumption are getting less year by year. Eventhouh the average household consumption of electricity and gas in 2018 increases than the previous year, they are still much lower than in year 2010. Which means, on average the energy used by dutch people are getting less in the past decade. Do they opt for more renewable energy? Let find out that next.

### 3. What are the trend of the dutch people using renewable energy?

The `delivery_perc` column in the datasets show the values of the net consumption of the electricity/gas and the lower the percentage mean more energy are given back. The asumption done here is that the energy given back are the solar energy produces by the dutch. I only consider the electricity dataframe for this part. What I did here is calculate the percentage of the energy given back for each zipcode and then grouped them by year and calculate their mean.

As an extra info, I want to show the Increment or decrement of renewable energy compare to it's previous year. They are as below:


```python
#find whether the mean value of energy given back increase or decrease compare to the previous year
renew_mean_diff=renew_mean.diff()
renew_mean_diff
```




    year
    2010         NaN
    2011   -0.207695
    2012   -0.108822
    2013    0.223381
    2014    3.176587
    2015    0.507646
    2016    1.166494
    2017    1.057605
    2018    1.626732
    2019    2.530314
    Name: renew_perc, dtype: float64



The highest increment of energy given back(more people produce renewable energy like solar) is in 2014 and the percentage increase since 2012 until latest. To make this easier to see, I plotted two graphs showing the mean energy given back yearly versus mean energy consume by household.


```python
fig, ax1 = plt.subplots()

color = 'tab:red'
ax1.set_xlabel('year')
ax1.set_ylabel('mean energy consumption per connection (kWh)', color=color)
ax1.plot(x1, cons_per_conn, color=color)
ax1.tick_params(axis='y', labelcolor=color)

ax2 = ax1.twinx()  # instantiate a second axes that shares the same x-axis

color = 'tab:blue'
ax2.set_ylabel('percentage energy given back (%)', color=color)  # we already handled the x-label with ax1
ax2.plot(x1,renew_mean, color=color)
ax2.tick_params(axis='y', labelcolor=color)
plt.title('Total Energy Consumption and Energy Given Back across the Year')

fig.tight_layout()
plt.show()
```


    
![png](output_26_0.png)
    


This plotted plots shows that the mean percentage energy produced increase yearly since the past decade and increase rapidly since 2014 eventhough the household mean energy consumption decreases.

### 4. Which City has high in electricity consumption? 

To answer this question, I look for the top five cities in total energy consumption for the year 2018. Assuming the total connections represent the population,I want find out whether these cities are also among the top populated cities in Netherlands.

**Top five cities in total energy consumption**


```python
top_ct2018= eltr_df[eltr_df['year']==2018].groupby('city').annual_consume.sum().nlargest(5)
top_ct2018
```




    city
    AMSTERDAM        55010230.00
    'S-GRAVENHAGE    40039600.00
    ROTTERDAM        36324022.00
    UTRECHT          22459675.00
    EINDHOVEN        17574828.49
    Name: annual_consume, dtype: float64



**Top populated cities in Netherlands**


```python
eltr_df.groupby('city').num_connections.sum().nlargest(5)
```




    city
    AMSTERDAM        4736313.0
    ROTTERDAM        3056245.0
    'S-GRAVENHAGE    2668500.0
    UTRECHT          1368767.0
    GRONINGEN         949205.0
    Name: num_connections, dtype: float64



It is predicted that the city with higher population will have higher total consumption of energy and the above data proved it. The top 4 cities with highest consumption are Amsterdam, Rotterdam, S-Gravenhage and Utrecht and they have the highest population according to order as well based on [Wikipedia](https://en.wikipedia.org/wiki/Demography_of_the_Netherlands). The total population of Eindhoven and Groningen doesn't differ much. So I would say that bigger populated cities has higher energy consumption. Now lets see how the total consumption changes for the past 5 years.    



```python
#function to replace value of old tick form to new
def millions(x, pos):
    'The two args are the value and tick position'
    return '{}'.format(int(x * 1e-6))

formatter = FuncFormatter(millions)

fig, ax = plt.subplots(figsize=[8,5])
ax.yaxis.set_major_formatter(formatter)
sns.lineplot(x='year', y='total', hue='city',  data=cities_yr)
#change the units to GWh ((1k*1M)Wh=GWh)
plt.ylabel('total consumption in GWh')
plt.title('Energy consumption for Top 5 Cities across the Years ')
plt.legend(bbox_to_anchor=(1.05,1))

plt.show()
```


    
![png](output_35_0.png)
    


As expected, Amterdam consume the highest energy in Netherlands as it is the main city in Netherlands and has the highest total city population in the country. Overall, the energy consumption for each year doesn't differ much but it is possible to see the reduction of the total energy since 2014 for The Hague (S-GRAVENHAGE) and Rotterdam. The total energy usage of Amsterdam and Utrecht doesn't changed much except there is a reduction for Amsterdam from 2014 for a year only. The data of Eindoven from 2010 to 2016 seems suspicious  as the value differ so much. This could possibly because there is incomplete or missing data for that particular years.

These are top 5 cities in total energy consumption but how about consumption per connection? To answer this part, I calculate the difference of consumption per connection between 2010 and 2018 and find the highest increment.


```python
cities_yr=eltr_df[eltr_df.city.isin(cities_top)].groupby(['city','year']).cons_per_conn.mean()
cities_yr.name='total'
cities_yr=cities_yr.reset_index()

fig, ax = plt.subplots(figsize=[8,5])
sns.lineplot(x='year', y='total', hue='city',  data=cities_yr)
plt.ylabel('total consumption in kWh')
plt.title('Cities with highest Energy Consumption per Connection in 8 Years')
plt.legend(bbox_to_anchor=(1.05,1))

plt.show()
    
```


    
![png](output_37_0.png)
    


The cities that has highest consumption per connection compared to the past eight years are Eursinge, Langedijk, Leimuiderbrug, Maastricht-Airport and Zuidveen. There are high jump in the average household energy usage value in 2015 for Langedijk and in 2018 for Maastricht-Airport and Zuidveen. I am not sure if this jump in values are trustable or something really happened that cause the value to increase more than six times in just a year. Afterall, I didn't dig deeper to find reason behind this. 

## Conclusions

Liander is the biggest network provider among those main three in Netherlands. The total connections for the electricity and gas are increasing slowly each year. The total consumption of energy for electricity has been increased since 2017 and for gas since 2016. The mean of energy consumption per connection has been decrease since 2010  consistently until 2017 and increased back in 2018. However it still recorded lower value compared to eight years ago. 

Not only that dutch people are using less energy on average, they even produced more energy back significantly since 2013. The increased in percentage of the energy given back each year for the past years show that more people are opting for solar energy.  

In terms of cities, the higher populated cities consumed more energy which we have been expected. Certain cities are showing reduction of total energy usage since 2014 like The Hague and Rotterdam and some cities's energy consumption don't differ much each year. 


*NB: I hope you enjoy reading this report. Mind that my knowledge about this dataset is limited and I am not a dutch.  Overall, I had fun analysing this dataset. If you have ideas for improvement, please let me know.*

Cheers!
 
