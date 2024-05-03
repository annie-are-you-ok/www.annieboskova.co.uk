---
title: Analysis of Energy Demand
date: '2024-05-03'
---


The assignment will focus on data visualisation using pandas library.

KATE expects your code to define variables with specific names that correspond to certain things we are interested in.

KATE will run your notebook from top to bottom and check the latest value of those variables, so make sure you don't overwrite them.

* Remember to uncomment the line assigning the variable to your answer and don't change the variable or function names.
* Use copies of the original or previous DataFrames to make sure you do not overwrite them by mistake.

You will find instructions below about how to define each variable.

Once you're happy with your code, upload your notebook to KATE to check your feedback.

### Importing Libraries
Run the cell below first, to import `pandas` and `matplotlib` `pyplot`. The `matplotlib_axes_logger.setLevel('ERROR')` code prevents some unnecessary warnings from showing.

```python
import pandas as pd

from matplotlib import pyplot as plt
from matplotlib.axes._axes import _log as matplotlib_axes_logger
matplotlib_axes_logger.setLevel('ERROR')
```

### About the Datasets

In the following section, you will be analysing a datasets from the UK government detailing energy consumption across various sectors of industry. 

The datasets include information about:

- Sector - sectors of industry         
- Sub-Sector - a sector that is part of a larger sector       
- Electricity - energy consumption measured in kilowatt hour (kWh)     
- Natural Gas - energy consumption measured in kilowatt hour (kWh)     
- Oil - energy consumption measured in kilowatt hour (kWh)              
- District Heating - is a system for distributing heat generated in a centralised location through a system of insulated pipes, measured in kilowatt hour (kWh)
- Other - other energy sources measured in kilowatt hour (kWh) 

### Data Collection

We have written some code below to do some initial collation and cleaning of the datasets we'll be working with - see if you can follow along and understand what each line is doing.

Run the following cell to import and concatenate the datasets, assigning the result to DataFrame `data`:

```python
df1 = pd.read_csv('data/heating_2018.csv')
df2 = pd.read_csv('data/hot_water_2018.csv')
df3 = pd.read_csv('data/catering_2018.csv')
data = pd.concat([df1, df2, df3], keys=['Heating', 'Hot Water', 'Catering']).reset_index(level=[0])
#combing df1, df2, and df3 into 1 dataframe, setting the keys for df1, df2, df3, and resetting the index so it starts at 0
# reset_index(level=[0]) - reset index to start at 0, keep level_0 and leave out level_1
```

**level:** int, str, tuple, or list, default None
- Only remove the given levels from the index. Removes all levels by default.
- level = index value identifying every row
- here we have 2 levels, key ('Heating', 'Hot Water', 'Catering') and numbers starting at 0

```python
# df1.head()
```

Running `data.head()`, `data.sample()` and  `data.info()` will show us how the DataFrame is structured:

```python
data.head()
```

```python
# # data = pd.concat([df1, df2, df3], keys=['Heating', 'Hot Water', 'Catering'])
# data = pd.concat([df1, df2, df3], keys=['Heating', 'Hot Water', 'Catering']).reset_index()
# data.head()
```

```python
data.sample(5)
```

```python
data.info()
```

### Data Processing

**Q1.** First of all, let's tidy up the `data` DataFrame:

- Use the `.rename()` method to change the name of the `level_0` column to `Use`
- Use the `.fillna()` method to update all `NaN` values to `0`
- Use the `.astype()` method to convert all numerical columns `['Electricity', 'Natural Gas', 'Oil', 'District Heating', 'Other']` to integers
- Use the `.sum(axis=1)` method to create a new column `Total` which contains the sum of all numerical columns

KATE will evaluate your updated version of `data` to check these changes have been made.

```python
#add your code to update the `data` DataFrame below
numerical_columns = ['Electricity', 'Natural Gas', 'Oil', 'District Heating', 'Other'] 

data.rename(columns = {'level_0':'Use'}, inplace = True) #rename level_0 to use and keep it (inplace=True)
data[numerical_columns] = data[numerical_columns].fillna(0).astype(int) 
#numerical_columns are within data df - replace NaN in those columns to 0 and cast to int

data['Total'] = data.sum(axis=1) #axis 1 = columns; create new column - vlues = sum of numerical columns

data
```

### Data Grouping

```python
data.info()
```

**Q2.** Create a new DataFrame called `ss`, using `.groupby()` to group DataFrame `data` by column `'Sub-Sector'`, which contains the `.sum()` for each of the numerical (energy type) columns for each group:

See below code syntax for some guidance:
```python
ss = DataFrame_Name.groupby(by=...).sum() 
```

**Groupby()** Group DataFrame using a mapper or by a Series of columns. 

```python
#add your code below
ss = data.groupby(by = 'Sub-Sector').sum()
ss.head()
```

**Q3.** Create a new DataFrame called `use`, using `.groupby()` to group DataFrame `data` by column `'Use'`, which contains the `.sum()` for each of the numerical (energy type) columns for each group:

See below code syntax for some guidance:
```python
use = DataFrame_Name.groupby(by=...).sum() 
```

```python
#add your code below
use = data.groupby(by = 'Use').sum() 
#group by types of values in Use (catering, heating, hot water) & sum up the values per column, per row
use
```

**Q4.** Create a new DataFrame called `sector`, using `.groupby()` to group DataFrame `data` by column `'Sector'`, and make use of `.agg()` method on the `Total` column such that the new DataFrame has columns for `'sum'`, `'mean'`, and `'count'` of the values in `'Total'`:

- Use the `.sort_values()` method to sort the resulting DataFrame by `'sum'`, in *descending* order

- You may find [this documentation page](https://pandas.pydata.org/pandas-docs/version/0.23.1/generated/pandas.core.groupby.DataFrameGroupBy.agg.html) useful

See below code syntax for some guidance:
```python
sector = DataFrame_Name.groupby(by=...)['Total'].agg([...]) 
sector = sector.sort_values(by=..., ascending=...)
```

The **agg()** method allows you to apply a function or a list of function names (eg 'sum', 'mean', 'count') to be executed along one of the axis of the DataFrame, default 0, which is the index (row) axis.

```python
# data[['Sector','Total']]
# x = data.groupby(by = 'Sector').sum()
# x.sort_values(by = 'Total', ascending = False)
```

```python
#add your code below

sector = data.groupby(by = 'Sector')['Total'].agg(['sum', 'mean', 'count']) #.agg lets you apply functions
#['Total'] telling python to look at that column once grouped by sector
sector = sector.sort_values(by = 'sum', ascending = False)

sector
```

*You may want to submit your notebook to KATE to ensure your `data`, `ss`, and `use` and `sector` DataFrames are as expected before moving on to the visualisations.*

### Data Visualisation

**Q5.** Refer to the `ss` DataFrame.

Create a **histogram** from the `Electricity` column of `ss` DataFrame using the `.plot()` method: `ss['Electricity']`
- The histogram should have 5 `bins`
- Assign the plot to the variable `elec_hist`
- Ensure your code cell starts with `plt.figure()`

See below code syntax for some guidance:
```python
plt.figure() 
elec_hist = DataFrame_Name.plot(kind='hist', bins=...);
```

*We need to execute `plt.figure()` before creating each new plot in the notebook, otherwise the properties of previous plots will be overwritten in memory and KATE will not evaluate them correctly.*

**pyplot .figure()** method to set the figsize for the figure (and make it larger than the default)

```python
#add your code below
plt.figure(figsize=(10,6))
elec_hist = ss['Electricity'].plot(kind = 'hist', bins = 5);
```

**Q6.** Refer to the `ss` DataFrame.

Create a **scatter plot** of `Natural Gas` vs `Total`, to see the relationship between the two columns of `ss` DataFrame.

- Use the `.plot()` method on `ss` DataFrame
- Have `Natural Gas` on the `x-axis` and `Total` on the `y-axis`
- Assign the plot to the variable `gas_total`
- Ensure your code cell starts with `plt.figure()`

See below code syntax for some guidance:
```python
plt.figure() 
gas_total = DataFrame_Name.plot(x=..., y=..., kind='scatter');
```

```python
#add your code below
plt.figure(figsize=(10,6))
gas_total = ss.plot(x = 'Natural Gas', y = 'Total', kind = 'scatter');
```

**Q7.** Refer to the `sector` DataFrame. 

Create a **vertical bar chart** of the `'sum'` column of the `sector` DataFrame using the `.plot()` method: `sector['sum']`

- Add a title of `'Energy consumption by sector'` to the plot
- Assign the plot to the variable `sector_sum`
- Ensure your code cell starts with `plt.figure()`

See below code syntax for some guidance:
```python
plt.figure()
sector_sum = DataFrame_Name.plot(kind='bar', title=...);
```

```python
#add your code below
plt.figure(figsize=(10,6))
sector_sum = sector['sum'].plot(kind = 'bar', title = 'Energy consumption by sector');
```

**Q8.** Refer to the given `new_df_use` DataFrame, which is identical to the `use` DataFrame but excludes the `Total` column (see below for the code).

Create a *horizontal* and *stacked* bar chart from the `new_df_use` DataFrame, using the `.plot()` method:

- Assign the plot to the variable `use_type`
- Give it a `figsize` of `(12,12)`
- You may find [this page](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.barh.html) useful
- Ensure your code cell starts with `plt.figure()`

See below code syntax for some guidance:
```python
plt.figure()
use_type = DataFrame_Name.plot.barh(stacked=True, figsize=(...));
```

<br/>

Run the following code cell to create the DataFrame `new_df_use`:

```python
new_df_use = use.loc[:, ['Electricity', 'Natural Gas', 'Oil', 'District Heating', 'Other']]
new_df_use.head()
```

```python
#add your code below
plt.figure()
use_type = new_df_use.plot.barh(stacked = True, figsize = (12,12));
```
