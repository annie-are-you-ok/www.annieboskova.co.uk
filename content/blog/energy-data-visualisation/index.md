---
title: Analysis of Energy Demand - Data Visualisation using Pandas 
summary: L4 Data Analysis Bootcamp with Cambridge Spark - Data Visualisation assignment. #shows on the homepage
date: 2024-04-26

# Place an image named `featured.jpg/png/gif` in this page's folder and customize its options here.
image:
  #placement: 1 = Full column width, 2 = Out-set, 3 = Screen-width 
  placement: 1
  caption: 'Image credit: [**giphy**](https://www.giphy.com/)'
  #focal point = Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, or BottomRight
  focal_point: 'Smart'
  preview_only: false

authors:
  - admin

tags:
  - Pandas
  - Matplotlib
  - Data Visualisation
  - Assignment
---
This assignment focused on datasets from the UK government, detailing energy consumption across various sectors of industry. This included Electricity, Natural
Gas, and Oil energy consumptions (measured in kwh).

<!-- - intro/summary -- **WHY**

Within this assignment I...

Before we were able to perform any data analysis, some data cleansing was required which was the focus of this project. What do I mean by data cleansing? That is the removal of missing or incorrect values, and superfluous data. Transforming some of the data involved changing data types which allowed me to utilise arithmetic operators to create new columns within the data set before analysis took place.
 
### Importing Libraries

To begin with, I first need to import some python libraries; **Pandas** and **Matplotlib** (or more specifically, pyplot):
```python
import pandas as pd
from matplotlib import pyplot as plt
```
I also added the following code which sets the logging level to 'ERROR', meaning that only errors and critical messages will be logged and I won't receive debug, info, or warning log messages. 

```python
from matplotlib.axes._axes import _log as matplotlib_axes_logger
matplotlib_axes_logger.setLevel('ERROR')
```

### Data Collection

Using the following code, I was able to combine some CSV files into one dataframe (called data) using the **.concat()** function. I did this so that I could process the data from 3 datasets (heating_2018, hot_water_2018, and catering_2018) all at once and carry out some analysis. 
```python
df1 = pd.read_csv('data/heating_2018.csv')
df2 = pd.read_csv('data/hot_water_2018.csv')
df3 = pd.read_csv('data/catering_2018.csv')
data = pd.concat([df1, df2, df3], keys=['Heating', 'Hot Water', 'Catering']).reset_index(level=[0])
```
I assigned a key to each file to help differentiate between them (this generated a new column called *level_0* in which the values would be the coresponding key for the data) and reset the index using **reset_index(level=[0])** so that the new dataframe would start at index 0. This would mean that each row would have a unique index number. 
Concatenating dataframes would usually result in a *index* column of the original index values, however, as I used the **keys** parameter of **.concat()**, this will also generate an index column - this will result in 2 columns, *level_0* and *level_1*. This is why I have added **level=[0]** when **reset_index()** is called. This allowed me to drop level_1 which was no longer needed as a new index (starting at 0) had been set.  

The resulting dataframe looked like this - I have included the top 5 rows (.head()) as well as 5 random rows (.sample(5)) - this is because, whilst the top 5 rows show that my code worked, **.sample()** shows that my keys (['Heating', 'Hot Water', 'Catering']) are working as intended:
![screen reader text](screenshot-1.png "screenshot-1")












```python 

``` 

### 



###


###


###


###


### 

###


### Aftermath - What did I learn? --> -->
 -->
